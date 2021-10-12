---
title: "Oracle Database 이해"
last_modified_at: 2021-08-27
categories:
  - Database
tags:
---

## Database

Oracle Database 구조

# 오라클 메모리 구조
- PGA =Program Global Area = 서버 프로세스에게 할당되는 메모리
- 용도 : 유저로부터 요청받은 작업을 처리하는데 사용되는 메모리 영역
- SGA = System Global Area = 프로세스 전체 간 공유되는 메모리
- 데이터를 디스크로부터 읽어 메모리로 적재한 후 Read/Write/Update/Delete하는데 활용할 떄 사용되는 메모리 공간

# PGA 구성
- 정렬 공간 (Sort Area) : Order by, Group by 수행 시 정렬할 때 사용되는 공간, 메모리 부족 시 디스크 공간 활용
- 세션 정보 (Session Information) : 서버 <-> 유저 연결 정보
- 커서 상태 정보(Cursor State) : SQL 파싱 정보가 저장되어 있는 주소
- 변수 저장 공간 (Stack Area) : Bind 변수를 저장하는 공간
- 유저 프로세스 -> 서버 프로세스 -> PGA 로 할당

# SGA 구성 (중요)
- 공유풀(Shared Pool)
- 고정 영역(Permanent Area) : SGA를 관리하는 파라메터 정보
- 동적 영역(Dynamic  Area) : 라이브러리 캐쉬(SQL, Parse Tree, 실행계획을 저장), 데이터 딕셔너리 캐쉬 (Oracle Object 정보를 저장)
- 데이터 버퍼 캐쉬 (Data Buffer Cache) : 디스크에서 읽어온 데이터를 저장하는 공간
- 서버 프로세스 : 디스크 -> 데이터 버퍼 캐쉬에로 데이터를 저장하는 프로세스
- DBWR(Database Writer) Background Process : 데이터 버퍼 캐쉬 -> 디스크로 데이터를 기록하는 프로세스
- 리두 로그 버퍼(Redo Log Buffer) : 데이터 변경에 대한 로그를 저장하는 영역
- LGWR (Log Writer) : 로그 버퍼의 내용을 로그 파일에 기록하는 프로세스 (Commit 수행 시, DBWR에 의해 DIsk로 바로 기록되지 않고, Redo Log File에 기록해둔 뒤, 일괄적으로 Log File의 내용을 기준으로 Disk에 실제 데이터를 기록함, 유저 입장에서는 Commit 실행 시, Disk에 기록된 것으로 생각할 수 있음)
- 대형 풀 (Large Pool), 자바 풀 (Java Pool)

# SQL SELECT 쿼리 실행순서
- FROM > WHERE > GROUP BY > HAVING > SELECT > ORDER BY
- 실행 순서로 인하여 ALIAS 는 주로 FROM에서 지정해주어야 WHERE, GROUP BY, HAVING에서 사용 가능.

