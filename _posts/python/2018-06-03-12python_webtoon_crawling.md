---
layout: post
title: python을 활용한 웹툰 크롤링 - 내 코드 복습(수정필요)
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


class Webtoon:
    def __init__(self, webtoon_id):
        self.webtoon_id = webtoon_id
        self._title = None
        self._author = None
        self._description = None
        self._html = ''

    @property
    def html(self):
        if not self._html:
            file_path = 'data/_episode_list-{webtoon_id}.html'.format(webtoon_id=self.webtoon_id)
            url_episode_path = 'https://comic.naver.com/webtoon/list.nhn'
            params = {
                'titleId': self.webtoon_id,
            }

            if os.path.exists(file_path):
                html = open(file_path, 'rt').read()

            else:
                response = requests.get(url_episode_path, params)
                html = response.text
                open(file_path, 'wt').write(html)
            self._html = html
        return self._html

    def set_info(self):

        soup = BeautifulSoup(self.html, 'lxml')

        h2_title = soup.select_one('div.detail > h2')
        title = h2_title.contents[0].strip()
        author = h2_title.contents[2].get_text(strip=True)
        description = soup.select_one('div.detail > p').get_text(strip=True)

        self._title = title
        self._author = author
        self._description = description

    @property
    def title(self):
        if not self._title:
            self.set_info()
        return self._title

    @property
    def author(self):
        if not self._author:
            self.set_info()
        return self._author

    @property
    def description(self):
        if not self._description:
            self.set_info()
        return self._description


class Episode(Webtoon):
    def __init__(self, title, url):
        super().__init__(self)
        self._title = title
        self.url = url
        self._episode_list = list()

    def crawl_episode_list(self):

        soup = BeautifulSoup(self.html, 'lxml')

        table = soup.select_one('table.viewList')
        tr_list = table.select('tr')

        for index, tr in enumerate(tr_list[1:]):
            # print('==={}===\n{}\n'.format(index, tr))
            if tr.get('class'):
                continue

            url_thumbnail = tr.select_one('td:nth-of-type(1) img').get('src')

            from urllib import parse
            url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
            query_string = parse.urlsplit(url_detail).query
            query_dict = parse.parse_qs(query_string)
            no = query_dict['no'][0]
            title = tr.select_one('td:nth-of-type(2) a').get_text(strip=True)
            rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
            create_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)

            episode_list = list()
            episode_list.append(url_thumbnail)
            episode_list.append(no)
            episode_list.append(title)
            episode_list.append(rating)
            episode_list.append(create_date)
            print(episode_list)
        return self._episode_list

    @property
    def episode_list(self):
        if not self._episode_list:
            self.crawl_episode_list()
        return self._episode_list

    def episode_url(self):
        url = 'http://comic.naver.com/webtoon/detail.nhn?'
        params = {
            'titleId': self.webtoon_id,
        }
        episode_url = url + parse.urlencode(params)
        return episode_url


class EpisodeImage:
    def __init__(self, webtoon_id, no, episode, url, file_path):
        self.episode = episode
        self.url = url
        self.file_path = file_path
        self.webtoon_id = webtoon_id
        self.no = no

    def get_image_url_list(self):
        file_path = 'data/episode_detail-{webtoon_id}-{episode_no}.html'.format(
            webtoon_id=self.webtoon_id,
            episode_no=self.no,
        )

        if os.path.exists(file_path):
            html = open(file_path, 'rt').read()

        else:
            response = requests.get(self.url)
            html = response.text
            open(file_path, 'wt').write(html)

        soup = BeautifulSoup(html, 'lxml')
        img_list = soup.select('div.wt_viewer > img').get('src')

        return [img.get('src') for img in img_list]

    def download_image_file_path(self, url_img):
        url_referer = f'http://comic.naver.com/webtoon/list.nhn?titleId={self.webtoon_id}'
        headers = {
            'Referer': url_referer,
        }
        response = requests.get(url_img, headers=headers)
        file_name = url_img.split('/', 1)[-1]

        dir_path = f'data/{self.webtoon_id}/{self.no}'
        os.makedirs(dir_path, exist_ok=True)

        file_path = f'{dir_path}/{file_name}'
        open(file_path, 'wb').write(response.content)

    def download_image_episode(self):
        for url in self.get_image_url_list():
            self.download_image_file_path(url)


if __name__ == '__main__':
    # 조건적으로 실행, 파이썬 자체가 내부적으로 사용하는 특별한 변수명. 우리가 실행했을때만 이 코드가 실행이 된다.
    webtoon1 = Webtoon(679519)
    print(webtoon1.title)
    print(webtoon1.author)
    print(webtoon1.description)
    e1 = webtoon1.episode_list[0]
    e1.download_image_episode()

```
