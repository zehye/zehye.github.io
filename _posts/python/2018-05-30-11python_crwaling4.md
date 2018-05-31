---
layout: post
title: python을 활용한 웹툰 크롤링 4단계 - 최종 과제
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
from urllib.parse import urlencode

import requests
from bs4 import BeautifulSoup


def webtoon_crawler(webtoon_id):
    """
   webtoon_id 를 매개변수로 입력받아서
   웹툰 title, author,description을 딕셔너리로 넘겨주는 함수
   :param webtoon_id:
   :return:
   """

    file_path = 'data/episode_list-{webtoon_id}.html'.format(webtoon_id=webtoon_id)
    # file_path 바뀐부분 참고!
    url_episode_list = 'http://comic.naver.com/webtoon/detail.nhn'
    params = {
        'titleId': webtoon_id,
    }

    if os.path.exists(file_path):
        html = open(file_path, 'rt').read()
    else:

        response = requests.get(url_episode_list, params)
        print(response.url)
        html = response.text
        open(file_path, 'wt').write(html)

    soup = BeautifulSoup(html, 'lxml')

    h2_title = soup.select_one('div.detail > h2')
    title = h2_title.contents[0].strip()
    author = h2_title.contents[1].get_text(strip=True)
    description = soup.select_one('div.detail > p').get_text(strip=True)

    # print(title)
    # print(author)
    # print(description)

    info = dict()
    # 웹툰의 정보를 딕셔너리 형태로 받아준다.(title, author, description)
    info['title'] = title
    info['author'] = author
    info['description'] = description

    return info
    # return된 info안에는 웹툰의 title, author, description정보가 들어가있다.
    # 이렇게 return된 info값은 class Webtoon에 활용된다.

def episode_crawler(webtoon_id):
    """
    webtoon_id를 매개변수로 입력받아서
    webtoon_id, title, url_thumbnail, no, rating, created_date의 정보를 가져오는 크롤러 함수
    :param webtoon_id:
    :return:
    """
    file_path = 'data/episode_list-{webtoon_id}.html'.format(webtoon_id=webtoon_id)
    # file_path 바뀐 부분 참고

    if os.path.exists(file_path):
        html = open(file_path, 'rt').read()

    soup = BeautifulSoup(html, 'lxml')
    table = soup.select_one('table.viewList')
    tr_list = table.select('tr')

    episode_lists = list()
    # for문 바깥에서 episode_lists라는 빈 리스트를 만든다.

    for index, tr in enumerate(tr_list[1:]):
        if tr.get('class'):
            continue

        url_thumbnail = tr.select_one('td:nth-of-type(1) img').get('src')

        from urllib import parse
        url_detail = tr.select_one('td:nth-of-type(1) > a').get('href')
        query_string = parse.urlsplit(url_detail).query
        query_dict = parse.parse_qs(query_string)
        no = query_dict['no'][0]

        title = tr.select_one('td:nth-of-type(2) > a').get_text(strip=True)
        rating = tr.select_one('td:nth-of-type(3) strong').get_text(strip=True)
        created_date = tr.select_one('td:nth-of-type(4)').get_text(strip=True)

        # print(url_thumbnail)
        # print(title)
        # print(rating)
        # print(created_date)
        # print(no)

        new_episode = Episode(
            # 크롤링한 결과를 class Episode의 new_episode 인스턴스로 생성
            # class Episode의 인스턴스들을 만든것 (이 부분 이해가 잘 안가는데, 용어가 헷갈려서 그런듯)
            # 클래스에 인스턴스 생성 부분 공부가 필요하다.

            webtoon_id= webtoon_id,
            no=no,
            url_thumbnail =url_thumbnail,
            title=title,
            rating = rating,
            created_date = created_date,
        )

        episode_lists.append(new_episode)
        # episode_lists에 Episode인스턴스를 추가
        # print(episode_lists)
        # 이 정보들을 넣고 새로운 인스턴스들을 만든다.

    return episode_lists

# ep = episode_crawler(703845)
# print(q)
# for item in ep:
#     print(item.no, item.title)


class Episode:
    def __init__(self, webtoon_id, no, url_thumbnail, title, rating, created_date):
        self.webtoon_id = webtoon_id
        self.no = no
        self.url_thumbnail = url_thumbnail
        self.title = title
        self.rating = rating
        self.created_date = created_date

    @property
    def url(self):
        url = 'https://comic.naver.com/webtoon/detail.nhn?'
        params = {
            'titleId': self.webtoon_id,
            # 내가 원하는 webtoon의 id
            'no': self.no,
            # 내가 원하는 webtoon의 no
        }

        episode_url = url + urlencode(params)
        # urllib, parse, urlencode 다시 읽어보기
        return episode_url


class Webtoon:
    def __init__(self, webtoon_id):
        self.webtoon_id = webtoon_id

        info = webtoon_crawler(webtoon_id)
        # 위에 만들어놓았던 webtoon_id를 매개변수로 받은 webtoon_crawler함수에서
        # return했던 info를 가져온다.
        self.title = info['title']
        self.author= info['author']
        self.description = info['description']
        self.episode_list = list()

    def update(self):
        """
        update함수를 실행하면
        해당 웹툰 아이디에 따른 에피소드 정보들을 에피소드 인스턴스로 저장
        :return:
        """

        result = episode_crawler(self.webtoon_id)
        # self.webtoon_id를 하는 이유는 해당 웹툰의 id(즉 내가 얻고 싶은 웹툰의 id)를 가져와야 하니까 -> self
        # print(result)
        self.episode_list = result
        # 내가 원하는 episode의 list = result
        # print(self.episode_list) 역시 위의 print(result)와 같은 결과를 가져오는 것을 볼 수 있었다.


if __name__ == '__main__':
# __name__ =='__main__'이 무엇인지는 알겠는데, 이걸 왜 여기서 써야하는 지 모르겠다.
    webtoon2 = Webtoon(651673)
    print(webtoon2.title)
    webtoon2.update()
# print(webtoon2.title)

    for episode in webtoon2.episode_list:
      # webtoon2의 episode_list를 쭉 돌아서
        print(episode.url)
        # 결국 우리가 출력하고자 하는 것은 각 episode들의 url

```
