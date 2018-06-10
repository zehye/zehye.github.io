---
layout: post
title: python을 활용한 웹툰 크롤링 완성
category: python
tags: [python, 크롤링]
comments: true
---

> 패스트캠퍼스 웹 프로그래밍 수업을 듣고 중요한 내용을 정리했습니다.     
개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.      
> 이 포스팅에서는 크롤링하는 방법에 대해 설명합니다.
> 본 스크립트는 학습 목적으로만 사용해주세요.

<hr>

```python
import os
from urllib import parse

import requests
from bs4 import BeautifulSoup


class EpisodeImage:
    def __init__(self, episode, url):
        self.episode = episode
        self.url = url


class Episode:
    def __init__(self, webtoon, no, url_thumbnail,
                 title, rating, created_date):
        self.webtoon = webtoon
        self.no = no
        self.url_thumbnail = url_thumbnail
        self.title = title
        self.rating = rating
        self.created_date = created_date
        self.image_list = list()

    @property
    def url(self):
        """
        self.webtoon, self.no 요소를 사용하여
        실제 에피소드 페이지 URL을 리턴
        :return:
        """
        url = 'http://comic.naver.com/webtoon/detail.nhn?'
        params = {
            'titleId': self.webtoon.webtoon_id,
            'no': self.no,
        }

        episode_url = url + parse.urlencode(params)
        return episode_url

    def get_image_url_list(self):

        file_path = f'./data/{self.webtoon.webtoon_id}-{self.no}.html'

        if not os.path.exists(file_path):
            with open(file_path, 'wt') as f:
                response = requests.get(self.url)
                f.write(response.text)
            html = response.text
        else:
            html = open(file_path, 'rt').read()

        soup = BeautifulSoup(html, 'lxml')
        wt_viewer = soup.select_one('div.wt_viewer').select('img')

        for img in wt_viewer:
            new_ep_image = EpisodeImage(
                episode=self.no,
                url=img.get('src')
            )
            self.image_list.append(new_ep_image)
        return self.image_list

    def download_all_images(self):
        for url in self.get_image_url_list():
            self.download(url)

    def download(self, url_img):
        """
        :param url_img: 실제 이미지의 URL
        :return:
        """
        # 서버에서 거부하지 않도록 HTTP헤더 중 'Referer'항목을 채워서 요청
        url_referer = f'http://comic.naver.com/webtoon/list.nhn?titleId={self.webtoon}'
        headers = {
            'Referer': url_referer,
        }
        response = requests.get(url_img.url, headers=headers)
        # 이미지 URL에서 이미지명을 가져옴
        file_name = url_img.url.rsplit('/', 1)[-1]

        # 이미지가 저장될 폴더 경로, 폴더가 없으면 생성해준다
        dir_path = f'data/{self.webtoon.webtoon_id}/{self.no}'
        os.makedirs(dir_path, exist_ok=True)

        # 이미지가 저장될 파일 경로, 'wb'모드로 열어 이진데이터를 기록한다
        file_path = f'{dir_path}/{file_name}'
        open(file_path, 'wb').write(response.content)

        # 저장된 이미지를 인터넷에서 볼 수 있도록 html 파일로 생성
        with open('data/{}/{}.html'.format(self.webtoon.webtoon_id, self.no), 'a') as f:
            f.write('<img src = {}/{}>'.format(self.no, file_name))


class Webtoon:
    def __init__(self, webtoon_id):
        self.webtoon_id = webtoon_id
        self._title = None
        self._author = None
        self._description = None
        self._episode_list = list()
        self._html = ''
        self.page = 1

    def _get_info(self, attr_name):
        if not getattr(self, attr_name):
            self.set_info()
        return getattr(self, attr_name)

    @property
    def episode_list(self):
        if not self._episode_list:
            self.crawl_episode_list()
        return self._episode_list

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
        # 인스턴스의 html속성값이 False(빈 문자열)일 경우
        # HTML파일을 저장하거나 불러올 경로
        file_path = 'data/_episode_list-{webtoon_id}-{page}.html'.format(webtoon_id=self.webtoon_id,
                                                                         page=self.page)
        # HTTP요청을 보낼 주소
        url_episode_list = 'http://comic.naver.com/webtoon/list.nhn'
        # HTTP요청시 전달할 GET Parameters
        params = {
            'titleId': self.webtoon_id,
            'page': self.page,
        }
        # HTML파일이 로컬에 저장되어 있는지 검사
        if os.path.exists(file_path):
            # 저장되어 있다면, 해당 파일을 읽어서 html변수에 할당
            html = open(file_path, 'rt').read()
        else:
            # 저장되어 있지 않다면, requests를 사용해 HTTP GET요청
            response = requests.get(url_episode_list, params)
            # 요청 응답객체의 text속성값을 html변수에 할당
            html = response.text
            # 받은 텍스트 데이터를 HTML파일로 저장
            open(file_path, 'wt').write(html)
        self._html = html
        return self._html

    @property
    def info(self):

        return '{title} \n' \
               '작가 : {author} \n' \
               '설명 : {description} \n' \
               '총 연재횟수 : {episode_list} 회'.format(title=self.title,
                                                  author=self.author,
                                                  description=self.description,
                                                  episode_list=len(self.episode_list))

    @classmethod
    def all_webtoon_crawler(cls, keyword):
        url = 'https://comic.naver.com/webtoon/weekday.nhn'
        response = requests.get(url)

        soup = BeautifulSoup(response.text, 'lxml')
        all_webtoon_list = soup.select('div.col_inner > ul > li > a')

        result = list()
        for webtoon in all_webtoon_list:
            title = webtoon.get_text()
            if keyword in title:
                href = webtoon.get('href', '')
                query_string = parse.urlsplit(href).query
                query_dict = dict(parse.parse_qsl(query_string))
                titleId = query_dict['titleId']
                check = [item for item in result if item['titleId'] == titleId]
                if not check:
                    result.append({
                        'titleId': titleId,
                        'title': title,
                    })
        return result

    @classmethod
    def search_webtoon(cls, keyword):
        """
        keyword와 일치하는 웹툰 제목 검색
        :param keyword:
        :return:
        """
        search_result = cls.all_webtoon_crawler(keyword)
        result_list = list()
        if search_result:
            for webtoon in search_result:
                webtoon_title_id = cls(webtoon_id=webtoon['titleId'])
                result_list.append(webtoon_title_id)
        return result_list

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
        while True:
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

                # 매 에피소드 정보를 Episode 인보스턴스로 생성
                # new_episode = Episode 인스턴스
                new_episode = Episode(
                    webtoon=self,
                    no=no,
                    url_thumbnail=url_thumbnail,
                    title=title,
                    rating=rating,
                    created_date=created_date,
                )
                # episode_lists Episode 인스턴스들 추가
                self._episode_list.append(new_episode)
            # no가 1인경우 break
            if no == '1':
                break
            # 그 경우가 아니면 page를 1씩 추가하여 다음 페이지 웹툰 리스트 크롤링
            else:
                self.page += 1
```
