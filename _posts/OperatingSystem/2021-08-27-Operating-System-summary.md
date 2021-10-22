---
title: "Operating System 정리"
last_modified_at: 2021-10-16
categories:
  - OperatingSystem
tags: OS, Interrupt
---

# Chapter 1. Introduction

## 1.1 OS가 하는 일 (What Operating System do)

- 컴퓨터 시스템은 4가지로 나눠 볼 수 있으며, 역할을 아래와 같음 

  - 하드웨어 : CPU, I/O, Memory와 같은 자원

  - 어플리케이션 프로그램 : 하드웨어 자원으로 문제를 해결하는 수단

  - 유저 : 어플리케이션 프로그램을 사용하는 사람

  - **운영체제 : 하드웨어 자원을 어플리케이션에게 적절히 배분해주는 수단** 

![image](https://user-images.githubusercontent.com/80370113/137588010-0d095534-55f5-4570-b9ba-300ca849839d.png)

- 유저 관점에서의 운영체제는인터페이스에 따라 정의가 달라짐 ex ) 터치스크린, 음성인식 등 유저에겐 OS로 정의

- 시스템 관점에서의 운영체제는 자원 할당자(Resource Allocator)가 됨 ex) CPU, I/O, 메모리 공간 관리

- 운영체제의 정의 ? 관점에 따라 용도가 너무 다양하므로 정의 하기 어려움 =>  **OS의 목표는 문제를 해결하기 위한 프로그램 실행시키기 위함** => 프로그램을 실행시키는 역할을 하면 OS가 될 수 있음

- **왜 OS를 공부하는가** ? 모든 프로그램은 OS 위에서 동작하므로, **OS를 이해해야 효율,효과,적절, 안전하게 프로그램 작성할 수 있음**

- **OS상에서 항상 동작하는 프로그램을 커널(kernel)**이라고 하며, 커널과 관련된 프로그램을 시스템 프로그램이라고 부름

- 이 책은 주로 커널과 관련된 OS를 위주로 설명할 것.



## 1.2 컴퓨터 시스템 구성 (Computer-System Architecture)

- CPU, Memory, Disk 등 장치들은 그들만의 장치 컨트롤러를 가지고 있으며, 이들은 서로 Bus라는 것으로 이어져 있음

- CPU는 Interrupt 방식을 이용하여 장치의 Signal을 처리함

![image](https://user-images.githubusercontent.com/80370113/137736519-f27d9658-7b2e-4677-aa12-16e2e3125c8b.png)

### 1.2.1 Interrupt

- Interrupt : CPU에게 Event를 알려 이를 처리하도록 하는 메커니즘

- Interrupt Service Routine : Interrupt 신호가 발생할 경우, 처리해야하는 일련의 과정

- Interrupt Vector : 인터럽트 서비스 루틴의 시작주소를 저장하고 있는 Index Table 

- Interrupt 동작 방식 : CPU가 유저 프로그램 실행 도중 인터럽트 신호가 발생하면, Interrupt Vector를 참조하여 Interrupt Service Routine 을 찾아서 실행한 후 인터럽트 처리가 완료되면, 다시 실행하던 유저프로그램을 실행함 **(비동기 Events 처리)**

- 중요 처리작업 진행중일 때, 인터럽트 발생할 경우 ? 즉시 처리용 인터럽트, 후순위 처리 인터럽트를 구분하여 처리, 우선순위를 부여하여 처리

- 여러 장치로부터 인터럽트 요청이 왔을 때, 모든 인터럽트 처리 루틴을 다 확인 해야하는가 ? 인터럽트 벡트 테이블 이용, Chainning 구조로 처리 루틴 실행

![image](https://user-images.githubusercontent.com/80370113/137736563-7b8839a0-c9e4-41d7-9b85-e4edcc3b0635.png)

## 1.2.2 Storage Structure

- CPU는 메인 메모리에 적재된 프로그램의 명령어를 읽어와 실행함

- 컴퓨터가 켜질 때 가장 먼저 실행되는 프로그램을 Bootstrap Program이라 하고, 주로 운영체제를 메인 메모리로 로드하는 역활을 함

- RAM은 전원이 차단되면 내용이 삭제되는 휘발성이 있음

- EEPROM은 전원이 차단되어도, 내용이 삭제되지 않고, ROM에 탑재된 프로그램을 변경할 수 있으므로 주로 부트 스트랩 프로그램을 저장하는 용도로 사용됨

- 프로그램 실행은 Fetch(메인 메모리에 저장된 명령어를 CPU의 Register로 인출하는 과정) -> Encode (명령어의 Operand를 메인메모리로부터 인출하여 Register에 저장 하는 과정) -> Execute (명령어와 Operand를 가지고 실행) 과정으로 실행됨 -> Store (명령어 실행 결과를 다시 메인 메모리에 저장함)

- 폰노이만 구조 : CPU <-> Data Memory + Instruction Memory 

- 하버드 구조 : Data Memory <->CPU <->Instruction Memory

- 메인메모리의 휘발성으로 인해, 데이터 저장을 위한 비휘발성 저장장치를 사용함 (Secondary Storage)

- CPU -> Register -> Cache -> Main Memory -> SSD -> HDD

## 1.2.3 I/O Structure

- CPU는 다양한 장치로부터 인터럽트 신호를 받고, 이를 처리함

- 인터럽트로 인해 CPU에 부하가 많이 가는것을 해결하기 위해, DMA(Direct Memory Access) 기법을 사용함

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

