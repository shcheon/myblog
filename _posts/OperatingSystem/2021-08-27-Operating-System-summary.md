---
title: "Operating System 정리"
last_modified_at: 2021-10-16
categories:
  - OperatingSystem
tags:

---

# Operating System

# Chapter 1. Introduction

## 1.1 OS가 하는 일 (What Operating System do)

- 컴퓨터 시스템은 4가지로 나눠 볼 수 있으며, 역할을 아래와 같음 
  - 하드웨어 : CPU, I/O, Memory와 같은 자원
  - 어플리케이션 프로그램 : 하드웨어 자원으로 문제를 해결하는 수단
  - 유저 : 어플리케이션 프로그램을 사용하는 사람
  - **운영체제 : 하드웨어 자원을 어플리케이션에게 적절히 배분해주는 수단** 

![image-20211016201810149](C:\Users\천승현\AppData\Roaming\Typora\typora-user-images\image-20211016201810149.png)

- 유저 관점에서의 운영체제는인터페이스에 따라 정의가 달라짐 ex ) 터치스크린, 음성인식 등 유저에겐 OS로 정의
- 시스템 관점에서의 운영체제는 자원 할당자(Resource Allocator)가 됨 ex) CPU, I/O, 메모리 공간 관리
- 운영체제의 정의 ? 관점에 따라 용도가 너무 다양하므로 정의 하기 어려움 =>  **OS의 목표는 문제를 해결하기 위한 프로그램 실행시키기 위함** => 프로그램을 실행시키는 역할을 하면 OS가 될 수 있음
- **왜 OS를 공부하는가** ? 모든 프로그램은 OS 위에서 동작하므로, **OS를 이해해야 효율,효과,적절, 안전하게 프로그램 작성할 수 있음**

- **OS상에서 항상 동작하는 프로그램을 커널(kernel)**이라고 하며, 커널과 관련된 프로그램을 시스템 프로그램이라고 부름
- 이 책은 주로 커널과 관련된 OS를 위주로 설명할 것.



## 1.2 컴퓨터 시스템 조직 (Computer-System Organization)

- 컴퓨터 시스템은 CPU, Memory, Keyboard 등 각각의 요소가 있고, 이를 Bus를 통해 이어주고, 장치를 통제하기 위한 디바이스 드라이버가 존재함

![image-20211016205556166](C:\Users\천승현\AppData\Roaming\Typora\typora-user-images\image-20211016205556166.png)

- 인터럽트 (Interrupt) ![image-20211016210832712](C:\Users\천승현\AppData\Roaming\Typora\typora-user-images\image-20211016210832712.png)



## 1.3 컴퓨터 시스템 구성 (Computer-System Architecture)

## 1.4 OS 연산 (Operating-System Operation)

## 1.5 자원 관리 (Resource Management)

## 1.6 보안 및 보호 (Security and Protection)

## 1.7 가상화 (Virtualization)

## 1.8 분산 시스템 (Ditributed System)

## 1.9 커널 데이터 구조 (Kernel Data Struture)

## 1.10 컴퓨팅 환경 (Computing Environments)

## 1.11 Free and Open-Source Operating System 



# Chpater 2. OS 구조

