---
title: Code Smell 유형 및 개선 방법
last_modified_at: 2021-10-27
categories: 
  - Programming
tags:
  - Code Smell
---

# CodeSmell 이란?

- 코드에서 더 심오한 문제를 일으킬 가능성이 있는 소스코드

- ex) 코드 수정 시 또 다른 오류가 검출되는 코드

# CodeSmell 유형 및 개선방법

- Code Smell 유형을 찾아 Clean Code로 개선하면, S/W는 가독성,유지보수, 확장성이 높아지고 좋은 설계를 갖춘 구조가 된다.

1. Duplicated Code

   - 중복코드; 똑같은 코드 구조가 두 군데 이상 있는 코드

   - 개선 기법 : 메서드 추출(Extract Method), 메서드 상향(Pull Up Method), 알고리즘 전환(Substitute Algorithm)

2. Long Method

   - 장황한 메서드; 메서드의 길이가 지나치게 길고 중복된 코드가 많은 코드

   - 개선 기법 : 메서드 추출(Extract Method), 조건문 쪼개기(Decompose Conditional)

   - 메소드 길이가 10~20라인 정도로 작성하는 것이 좋음 ( 조직마다 기준을 다르게 가져갈 수 있음)

   - 메서드 명은 이해하기 쉽도록 목적을 나타내는 이름으로 정함

3. Long Parameter List

   - 과다한 매개변수; 루틴에 필요한 대부분을 매개변수를 사용해 전달하는 경우. 이해하기 어렵고, 다른 데이터가 필요할 때마다 수정해야 함

   - 개선 기법 : 매개변수 세트를 객체로 전환(Introduce Parameter Object), 객체를 통째로 전달(Preserve Whole Object), 매개변수 세트를 메서드로 전환(Replace Parameter with Method)

4. Divergent Change

   - 수정의 산발; 한 클래스가 다양한 원인 때문에 다양한 방식으로 자주 수정 되는 경우

   - 개선 기법 : 클래스 추출(Extract Class)

5. Shotgun Surgery

   - 기능의 산재; 수정할 때마다 여러 클래스에서 수많은 자잘한 부분을 고쳐야 하는 경우

   - 개선 기법 : 메서드 이동(Move Method), 클래스 내용 삽입 (inline Class), 필드 이동(Move Field)

   - 메서드 이동과 필드 이동을 적용해서 수정할 부분들을 하나의 클래스에 모음

6. Feature Envy

   - 잘못된 소속; 어떤 메서드가 자신이 속하지 않은 클래스에 더 많이 접근하는 경우

   - 개선 기법 : 메서드 추출(Extract Method), 메서드 이동(Move Method)

7. Swtich Statements

   - Switch 문; Switch문의 사용이 많은 경우 중복이 발생하여 유지보수성이 떨어짐

   - 개선 기법 : 메서드 추출(Extract Method), 메서드 이동(Move Method), 분류 부호를 하위 클래스로 전환(Replace Type Code with Subclasses),  분류 부호를 상태/전략 패턴으로 전환(Replace Type Code With State/Strategy)

8. Magic Number

   - 매직 넘버; 코드에 기입된 상수는 추 후, 유지보수 때 의도를 파악하기 어려움

   - 개선 기법 : final 또는 const와 같은 변수 상수화 키워드로 상수를 변수로 네이밍하여 의도를 표현
   
