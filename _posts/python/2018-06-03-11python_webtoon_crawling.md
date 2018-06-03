---
layout: post
title: python을 활용한 웹툰 크롤링 - 강사님 코드 복습
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.

<hr>

```python
import os
from urllib import parse

import requests
from bs4 import BeautifulSoup


class Episode:
    def __init__(self, webtoon_id, no, url_thumbnail,
                 title, rating, created_date):
        self.webtoon_id = webtoon_id
        self.no = no
        self.url_thumbnail = url_thumbnail
        self.title = title
        self.rating = rating
        self.created_date = created_date

    @property
    def url(self):
        """
        self.webtoon_id, self.no 요소를 사용하여
        실제 에피소드 페이지 URL을 리턴
        :return:
        """
        url = 'http://comic.naver.com/webtoon/detail.nhn?'
        params = {
            'titleId': self.webtoon_id,
            'no': self.no,
        }
        episode_url = url + parse.urlencode(params)
        return episode_url

    def get_image_url_list(self):
        print('get_image_url_list start')
        # 해당 에피소드의 이미지들의 URL문자열들을 리스트에 담아 리턴
        # 1. html 문자열 변수 할당
        #  파일명: episode_detail-{webtoon_id}-{episode_no}.html
        #  없으면 자신의 url property값으로 requests사용 결과를 저장
        # 2. soup객체 생성 후 파싱
        #  div.wt_viewer의 자식 img요소들의 src속성들을 가져옴
        # 적절히 list에 append하고 리턴하자

        # 웹툰 에피소드의 상세 페이지를 저장할 파일 경로
        #  웹툰의 고유ID와 에피소드의 고유ID를 사용해서 겹치지 않는 파일명을 만들어준다
        file_path = 'data/episode_detail-{webtoon_id}-{episode_no}.html'.format(
            webtoon_id=self.webtoon_id,
            episode_no=self.no,
        )
        print('file_path:', file_path)

        # 위 파일이 있는지 검사
        if os.path.exists(file_path):
            print('os.path.exists: True')
            # 있다면 읽어온 결과를 html변수에 할당
            html = open(file_path, 'rt').read()
        else:
            # 없다면 self.url에 requests를 사용해서 요청
            #  요청의 결과를 html변수에 할당
            #  요청의 결과를 file_path에 해당하는 파일에 기록
            print('os.path.exists: False')
            print(' http get request, url:', self.url)
            response = requests.get(self.url)
            html = response.text
            open(file_path, 'wt').write(html)

        # html문자열로 BeautifulSoup객체 생성
        soup = BeautifulSoup(html, 'lxml')

        # img목록을 찾는다. 위치는 "div.wt_viewer > img"
        img_list = soup.select('div.wt_viewer > img')

        # 이미지 URL(src의 값)을 저장할 리스트
        # url_list = []
        # for img in img_list:
        #     url_list.append(img.get('src'))

        # img목록을 순회하며 각 item(BeautifulSoup Tag object)에서
        #  'src'속성값을 사용해 리스트 생성
        return [img.get('src') for img in img_list]

    def download_all_images(self):
        for url in self.get_image_url_list():
            self.download(url)

    def download(self, url_img):
        """
        :param url_img: 실제 이미지의 URL
        :return:
        """
        # 서버에서 거부하지 않도록 HTTP헤더 중 'Referer'항목을 채워서 요청
        url_referer = f'http://comic.naver.com/webtoon/list.nhn?titleId={self.webtoon_id}'
        headers = {
            'Referer': url_referer,
        }
        response = requests.get(url_img, headers=headers)

        # 이미지 URL에서 이미지명을 가져옴
        file_name = url_img.rsplit('/', 1)[-1]

        # 이미지가 저장될 폴더 경로, 폴더가 없으면 생성해준다
        dir_path = f'data/{self.webtoon_id}/{self.no}'
        os.makedirs(dir_path, exist_ok=True)

        # 이미지가 저장될 파일 경로, 'wb'모드로 열어 이진데이터를 기록한다
        file_path = f'{dir_path}/{file_name}'
        open(file_path, 'wb').write(response.content)


class Webtoon:
    def __init__(self, webtoon_id):
        self.webtoon_id = webtoon_id
        self._title = None
        self._author = None
        self._description = None
        self._episode_list = list()
        self._html = ''

    def _get_info(self, attr_name):
        if not getattr(self, attr_name):
            self.set_info()
        return getattr(self, attr_name)

    @property
    def title(self):
        return self._get_info('_title')

    @property
    def author(self):
        return self._get_info('_author')

    @property
    def description(self):
        return self._get_info('_description')

    @property
    def html(self):
        if not self._html:
            # 인스턴스의 html속성값이 False(빈 문자열)일 경우
            # HTML파일을 저장하거나 불러올 경로
            file_path = 'data/episode_list-{webtoon_id}.html'.format(webtoon_id=self.webtoon_id)
            # HTTP요청을 보낼 주소
            url_episode_list = 'http://comic.naver.com/webtoon/list.nhn'
            # HTTP요청시 전달할 GET Parameters
            params = {
                'titleId': self.webtoon_id,
            }
            # HTML파일이 로컬에 저장되어 있는지 검사
            if os.path.exists(file_path):
                # 저장되어 있다면, 해당 파일을 읽어서 html변수에 할당
                html = open(file_path, 'rt').read()
            else:
                # 저장되어 있지 않다면, requests를 사용해 HTTP GET요청
                response = requests.get(url_episode_list, params)
                print(response.url)
                # 요청 응답객체의 text속성값을 html변수에 할당
                html = response.text
                # 받은 텍스트 데이터를 HTML파일로 저장
                open(file_path, 'wt').write(html)
            self._html = html
        return self._html

    def set_info(self):
        """
        자신의 html속성을 파싱한 결과를 사용해
        자신의 title, author, description속성값을 할당
        :return: None
        """
        # BeautifulSoup클래스형 객체 생성 및 soup변수에 할당
        soup = BeautifulSoup(self.html, 'lxml')

        h2_title = soup.select_one('div.detail > h2')
        title = h2_title.contents[0].strip()
        author = h2_title.contents[1].get_text(strip=True)
        # div.detail > p (설명)
        description = soup.select_one('div.detail > p').get_text(strip=True)

        # 자신의 html데이터를 사용해서 (웹에서 받아오거나, 파일에서 읽어온 결과)
        # 자신의 속성들을 지정
        self._title = title
        self._author = author
        self._description = description

    def crawl_episode_list(self):
        """
        자기자신의 webtoon_id에 해당하는 HTML문서에서 Episode목록을 생성
        :return:
        """
        # BeautifulSoup클래스형 객체 생성 및 soup변수에 할당
        soup = BeautifulSoup(self.html, 'lxml')

        # 에피소드 목록을 담고 있는 table
        table = soup.select_one('table.viewList')
        # table내의 모든 tr요소 목록
        tr_list = table.select('tr')
        # list를 리턴하기 위해 선언
        # for문을 다 실행하면 episode_lists 에는 Episode 인스턴스가 들어가있음
        episode_list = list()
        # 첫 번째 tr은 thead의 tr이므로 제외, tr_list의 [1:]부터 순회
        for index, tr in enumerate(tr_list[1:]):
            # 에피소드에 해당하는 tr은 클래스가 없으므로,
            # 현재 순회중인 tr요소가 클래스 속성값을 가진다면 continue
            if tr.get('class'):
                continue

            # 현재 tr의 첫 번째 td요소의 하위 img태그의 'src'속성값
            url_thumbnail = tr.select_one('td:nth-of-type(1) img').get('src')
            # 현재 tr의 첫 번째 td요소의 자식   a태그의 'href'속성값
            from urllib import parse
            url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
            query_string = parse.urlsplit(url_detail).query
            query_dict = parse.parse_qs(query_string)
            # print(query_dict)
            no = query_dict['no'][0]

            # 현재 tr의 두 번째 td요소의 자식 a요소의 내용
            title = tr.select_one('td:nth-of-type(2) > a').get_text(strip=True)
            # 현재 tr의 세 번째 td요소의 하위 strong태그의 내용
            rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
            # 현재 tr의 네 번째 td요소의 내용
            created_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)

            # 매 에피소드 정보를 Episode 인스턴스로 생성
            # new_episode = Episode 인스턴스
            new_episode = Episode(
                webtoon_id=self.webtoon_id,
                no=no,
                url_thumbnail=url_thumbnail,
                title=title,
                rating=rating,
                created_date=created_date,
            )
            # episode_lists Episode 인스턴스들 추가
            episode_list.append(new_episode)
        self._episode_list = episode_list

    @property
    def episode_list(self):
        # self.episode_list가 빈 리스트가 아니라면
        #  -> self.episode_list를 반환
        # self.episode_list가 비어있다면
        #  채우는 함수를 실행해서 self.episode_list리스트에 값을 채운 뒤
        #  self.episode_list를 반환

        # 다했으면
        # episode_list속성이름을 _episode_list로 변경
        # 이 함수의 이름을 episode_list로 변경 후 property설정
        if not self._episode_list:
            self.crawl_episode_list()
        return self._episode_list

        # if self.episode_list:
        #     return self.episode_list
        # else:
        #     self.crawl_episode_list()
        #     return self.episode_list


if __name__ == '__main__':
    # 조건적으로 실행, 파이썬 자체가 내부적으로 사용하는 특별한 변수명. 우리가 실행했을때만 이 코드가 실행이 된다.
    webtoon1 = Webtoon(651673)
    print(webtoon1.title)
    print(webtoon1.author)
    print(webtoon1.description)
    e1 = webtoon1.episode_list[0]
    e1.download_all_images()
```
