---
layout: post
date: 2024-10-15
title: "Shell shock"
tags: [bash, command injection, ]
categories: [1-day, ]
---


> 💡 [Shell shock (CVE-2014-6271)]

undefined
---



## 1. 쉘 쇼크란?

- 2014년 9월 24일에 밝혀진 유닉스류 운영체제에서 터미널로 사용하는 Bash shell에 존재하는 보안취약점
- 리눅스 계열 및 MAC OS X운영체제에서 사용되는 GNU bash쉘에서 취약점을 발견
- bash shell에서 임의의 환경변수에 특정 코드를 삽입하여 실행할 수 있는 취약점

=> 서버에서 악의적 명령실행, 관리자 권한 획득등 가능
-> 리눅스 환경변수에 빈 함수로 넣으면 그뒤에 오는 코드는 무조건 실행되는 심각한 버그로 CGI 스크립트를 통해 공격 가능


(위험성)

- 2014년 4월 하트블러드(Heart Bleed)보다 더 심각한 보안 버그이다.
- => OpenSSL만이 문제 / 일부버전만 영향(약 2년간) / 하트비트 기능을 끄면 안전
- Bash shell 광범위하게 사용
- => 최초로 발견되기 전까지 22년이라는 긴 기간동안 존재 / 40년(?)
- 해당 취약점에 취약한 버전은 무슨 코드든 실행할 수 있게 된다. (root탈취)
- 서버os인 유닉스, 리눅스에서 많은 역할을 하고 있는 점에서 취약

(사례)
야후 해킹공격을 당했을때 먼저 생각한 것이 쉘코드일 정도로 큰 버그이다.



{% raw %}
```text
(영향받는 버전)
1. centos버전 5-7
2. 우분투 10.04,12.04, 14.04
3. 데비안
4. 맥OS
5. 레드헷 엔터프라이즈 리눅스 4-7
6. 버전 4.43이하
```
{% endraw %}




## 2. 환경변수


환경 변수를 정의시 환경변수에 변수와 함수를 정의가능



{% raw %}
```text
$ export x="asdf"
x
bash: x: command not found

$ prinenv x
asdf
```
{% endraw %}



=> 이유 :  x는 함수가 아니기 때문에
but printenv로 호출시 값이 나옴


(환경변수로 함수를 정의하는 방법)



{% raw %}
```text
$ x() { echo asdf; } // 형시

$ export -f x // f옵션으로 환경변수에 등록
x
```
{% endraw %}




## 3. 익스플로잇



### 3-1. 취약한 bash쉘 제작



## 3-2. 쉘코드의 원리



{% raw %}
```text
$ export x='() {echo Hello~;}'
```
{% endraw %}


- bash의 환경변수에 함수처럼 보이는 변수를 정의
- 함수처러 보이는 변수란?

(최초로 보고된 예제)



{% raw %}
```text
$ env x='() {:;}; echo vulnerable' bash -c "echo this is a test"
vulnerable
this is a test
```
{% endraw %}


- 해당 환경변수를 가지고 명령을 실행하라
- x='() {:;}; echo vulnerable 환경변수를 가지고
-> echo this is a test 라는 구문을 실행


### 3-3. 취약한 코드 확인


ㅁㄴㅇㄹ



### 3-4. payload


=> payload의 모음



## 4. 대처방안



### 4-1. 원론적인 대처방안

1. GNU bash 1.14 - 4.3버전에 취약점이므로 최신버전 업데이트
2. 사용하지 않는 CGI페이지 삭제 또는 관련 서비스 중지
3. IPS/IDS등을 이용하여 공격 시그니처를 등록하여 차단


### 4-2. 코드 패치


ㅁㄴㅇㄹ


---



## 참고자료


1 - [https://blog.naver.com/jsmb/222713293995?trackingCode=rss](https://blog.naver.com/jsmb/222713293995?trackingCode=rss)
2 - [https://security.stackexchange.com/questions/68448/where-is-bash-shellshock-vulnerability-in-source-code](https://security.stackexchange.com/questions/68448/where-is-bash-shellshock-vulnerability-in-source-code)
=> bash의 취약한 부분을 확인 가능
3 - [https://github.com/dlitz/bash-shellshock/tree/master](https://github.com/dlitz/bash-shellshock/tree/master)
=> 취약한 bash를 만들기 위한 링크
4 - [https://baobob1024.tistory.com/42](https://baobob1024.tistory.com/42)

