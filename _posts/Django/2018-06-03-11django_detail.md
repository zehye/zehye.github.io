djangogitls-tutorial/
  mysite/ <- django프로젝트 관련 코드 컨테이너 폴더 이름
  manage.py
  mysite/ <-django 프로젝트의 설정 관련 패키지
    __init__.py
    settings.py

1. django코드 컨테이너 폴더의 이름을 app으로 변경
2. django프로젝트 설정 패키지의 이름을 config로 변경
djangogitls-tutorial/ <- 이 프로젝트의 컨테이너 폴더 (Root폴더)
  app/ <- django프로젝트 과년 코드 컨테이너 폴더
    config/ <- django 프로젝트의 설정 관련 패키지
      settings.py

  .gitignore <-django프로젝트(애플리케이션) 코드와 관계없지만 프로젝트를 위해 필요한 파일/폴더
  .git/
  requirements.txt
  ...

pycharm은 현재 프로젝트 구조에서 가장 상위폴더를 파이썬 source root로 인식한다.
-> 모듈을 import할때, 그때 만약에

project/
  project_module1.py
  project_module2.py
  pac1
    __init__.py
    pac1_modile.py
      pac2
        __init__.py
        pac2_modile.py
          pac3
            __init__.py
