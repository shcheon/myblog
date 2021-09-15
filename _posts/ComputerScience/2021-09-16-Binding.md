---
title: 동적바인딩, 정적바인딩
last_modified_at: 2021-09-16
categories: 
  - ComputerScience
tags:
  - Static Binding
  - Dynamic Binding
---

# 바인딩
- Binding, 소스코드 내 속성, 식별자, 함수를 메모리에 구속시키는 과정

# 정적 바인딩
- Static Binding, 컴파일 시점에 이루어 지는 바인딩을 말함

# 동적 바인딩
- Dynamic Binding, 실행 시점에 바인딩
- 가상함수, 다형성은 동적 바인딩으로 처리됨

# 동적 바인딩의 원리
- 가상함수 테이블을 통해 함수 메모리 번지수를 찾아냄
- 가상함수가 있는 클래스는 가상함수 테이블을 참조하는 포인터 변수가 추가됨
