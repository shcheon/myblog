---
title: 파이썬 문법 정리
last_modified_at: 2021-09-11
categories: 
  - Programming
tags:
  - python
---
# 파이썬 문법 정리

## 용어

표현식 : 값을 만들어내는 코드

```python
273 # 값
10 + 20 30 * 10 # 값
```

문장 : 표현식이 하나 이상 모인 것

프로그램 : 문장이 모인것

표현식 -> 문장 -> 프로그램

키워드 : 예약어로도 부름.

```python
>>> import keword
>>> print(keyword.kwlist) # python 키워드들 모두 출력가능

['False', 'None', 'True', '__peg_parser__', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

```

식별자 : 변수 또는 함수.

식별자 생성 시 주의 사항 

- 키워드, 숫자, 공백 사용금지
- 특수문자는 언더 바(_)만 허용
- SnakeCase(=단어 사이 언더바로 추가) 또는 CameCase(= 단어들의 첫 글자를 대문자 시작)로 주로 작성

식별자 구분하기

```python
CamelCase : 클래스 ex) PrintHello
SnakeCase : 함수, 변수 ex)print_hello(), print.hello
```

주석 : # 기호로 처리



##  자료형

- 문자열, 숫자, 불(boolean)이 있음

- **type()**로 자료형 확인 가능

```python
>>> type("hh")
<class 'str'>
>>> type(1)
<class 'int'>
>>> type(1.2)
<class 'float'>
```



## 문자열 만들기

### 큰따움표로 문자열 만들기

```python
>>> print("hello")
hello
```

### 작은따움표로 문자열 만들기

```python
>>> print('hello')
hello
```

### 문자열 내부에 따옴표 넣기

```python
>>> print("'hello' world")
'hello' world
>>> print('"hello" world')
"hello" world
```

### 이스케이프 문자 사용법

이스케이프 문자 : 역 슬래쉬(\) 기호와 함께 조합해서 사용하는 특수문자

```python
>>> print("\"hello\" world")
"hello" world
\n : 줄바꿈, \t : 탭
```



### 문자열 연산자

```python
# + : 문자열 연결 연산자
>>> print('hello' + 'world')
helloworld

# * : 문자열 반복 연산자
>>> print('hello'*3)
hellohellohello

# [] : 문자열 선택 범위 연산자(인덱싱) 
>>> print('helloworld'[0])
h
>>> print('helloworld'[1])
e
>>> print('helloworld'[2])
l

# [:] : 문자열 선택 범위 연산자(슬라이싱) 
# 양수는 문자열 시작점에서 앞으로 이동, 음수는 문자열 시작점에서 뒤로 이동
>>> print('helloworld'[0:5])
hello
>>> print('helloworld'[0:-1])
helloworl
>>> print('helloworld'[0:-4])
hellow
>>> print('helloworld'[0:4])
hell
>>> print('helloworld'[1:]) # 인덱스가 생략될 경우, 처음 또는 끝을 지정함
elloworld
>>> print('helloworld'[:3])
hel
```

인덱싱 : 문자를 참조하는 것

슬라이싱 : 문자열 일부를 추출하는 것



### 문자열 길이 구하기

```python
>>> len('hello')
5
```



