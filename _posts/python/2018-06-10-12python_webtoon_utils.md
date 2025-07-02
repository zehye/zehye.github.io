---
layout: post
title: python을 활용한 웹툰 크롤링 완성 - utils.py
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
import sys

from crawl import Webtoon


def ini():
    """
    초기 메서드
    :return:
    """
    while True:
        # 제목으로 웹툰 검색
        print('---------------------------------')
        keyword = input('검색할 웹툰명을 입력해주세요 :')
        result_lists = Webtoon.search_webtoon(keyword)
        # 검색 키워드의 결과가 없는 경우
        if not result_lists:
            print(f'일치하는 웹툰이 없습니다. 다시 입력해주세요\n')
            continue
        break

    # 검색한 키워드에 해당하는 웹툰 리스트 출력
    select_webtoon = print_search_list(result_lists)

    # 웹툰을 고르고 난 후 웹툰 정보/저장/ 관련 메서드 실행
    webtoon_select(select_webtoon)


def webtoon_select(webtoon):
    """
    선택한 웹툰의 정보, 저장을 위해 실행되는 메서드
    :param webtoon:(ini함수에서 선택된 Webtoon 인스턴스)
    :return:
    """
    while True:
        print('--------------------------------------------')
        print(f'현재 "{webtoon.title}" 웹툰이 선택되어 있습니다. ')
        print(f'1. {webtoon.title} 정보 보기')
        print(f'2. {webtoon.title} 저장 하기')
        print(f'3. 다른 웹툰 검색해서 선택하기')
        print(f'0. 종료하기')
        select = input("선택 :")

        # 웹툰 정보 출력 : 작가, 설명, 총 연재 수
        if select is '1':
            print('--------------------------------------------')
            print(webtoon.info)

        # 웹툰 저장
        elif select is '2':
            select_episode_download(webtoon)

        elif select is '3':
            ini()

        elif select is '0':
            print('웹툰 검색기를 종료합니다.')
            sys.exit(1)
        else:
            print(f'입력하신 번호가 올바르지 않습니다.\n')
            continue


def print_search_list(result_lists):
    """
    검색한 키워드에 해당하는 웹툰 리스트 출력 및 선택
    :param result_lists:
    :return:
    """
    while True:
        # 검색한 웹툰 리스트 출력
        for num, webtoon in enumerate(result_lists):
            # list는 0부터 시작하기때문에 사용자 편의를 위해 num+1
            print(f'{num+1}: {webtoon.title}')

        # input 경우 문자열이기 때문에 int형으로 변환
        select_str = input('만화를 선택해주세요 :')
        select = int(select_str) - 1

        # 선택 번호가 웹툰 리스트 인덱싱 값에 포함되지 않는 경우
        if select not in range(num + 1):
            print(f'입력하신 번호가 올바르지 않습니다.\n')
            continue

        # 입력받은 선택 번호에 맞는 웹툰 설정
        select_webtoon = result_lists[select]
        break
    return select_webtoon


def select_episode_download(webtoon):
    """
    선택한 웹툰 에피소드 저장 관련 메뉴 메서드
    :param webtoon:
    :return:
    """
    while True:
        print('--------------------------------------------')
        print(f'현재 "{webtoon.title}" 웹툰이 선택되어 있습니다. ')
        print(f'1. 모든 에피소드 저장하기')
        print(f'2. 한 에피소드만 저장하기')
        print(f'3. 뒤로 가기')
        select_download = input('선택 :')
        # 해당 웹툰 에피소드를 다운 받을때 1화부터 다운로드 하기 위해
        # reversed()함수를 이용하여 오름차순으로 변경
        reverse_webtoon_episode_list = list(reversed(webtoon.episode_list))
        if select_download is '1':
            # 전체 에피소드 다운로드
            for episode in reverse_webtoon_episode_list:
                episode.download_all_images()
                print(f'{episode.no}화가 다운되었습니다.')
        elif select_download is '2':
            while True:
                # 특정 에피소드만 다운로드
                for num, episode in enumerate(reverse_webtoon_episode_list):
                    print(f'{num+1}.{episode.title}')
                select_episode = input('선택 :')

                if int(select_episode) not in range(num + 1):
                    print(f'입력하신 번호가 올바르지 않습니다.\n')
                    continue
                e = reverse_webtoon_episode_list[int(select_episode) - 1]
                e.download_all_images()
                print(f'{e.no}화가 다운되었습니다.')
                break
        elif select_download is '3':
            break
        else:
            print(f'입력하신 번호가 올바르지 않습니다.\n')


if __name__ == '__main__':
    ini()
```
