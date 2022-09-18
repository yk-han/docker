**버전**

```
CUBRID 10.2  (java 1.6 이상설치 가능)
JAVA 1.8u212 (sts 세팅 버전)

설치파일 다운로드 - https://www.cubrid.org/downloads
```

**CUBRID 10.2 윈도우용 설치**

```
설치파일 다운로드 url - https://www.cubrid.org/downloads
1. Setup Wizard 실행
2. 라이선스 동의
3. 설치 경로지정
 -> Svr\Cubrid-10.2
4. 설치 (Install)
5. 설치완료 팝업(Finish) 
※ 참조 - https://mingd1og.tistory.com/2 
```

**CUBRID 10.2 윈도우용 매니징 설치**

```
CUBRIDManager-10.2-latest-windows-x64.zip
압축 푸는 경로 지정 또는 압축을 푼 후 원하는 위치로 이동
 -> SvrProgram\Cubridmanager
압축 푼 후 실행 시 경로 지정
 -> Svr\Cubrid-10.2 
지정 경로에서 cubridmanager.exe 실행
 -> Svr\Cubridmanager\
```

```
설치파일 다운로드 url - https://www.cubrid.org/downloads
1. 큐브리드매니저 설치파일 실행
2. 언어 선택
3. 설치 시작
4. 라이선스 동의
5. 구성요소 선택 (바탕화면 아이콘, 시작메뉴, 빠른실행)
3. 설치 경로지정 후 설치
 -> Svr\Cubrid-10.2
5. 설치완료 팝업(Finish) 
※ 참조 - https://mingd1og.tistory.com/3?category=831562 
```

**DB 백업**

```
* SSH, FTP 접속
 - SSH 접속 작업
  1. 계정 변경
   -> su - cubrid
  2. 백업 경로 이동
   -> cd /home/cubrid/backup
  3. 언로드 백업 (비밀번호 확인)
   -> cubrid unloaddb db@localhost 
 - FTP 접속 작업
  1. 경로 이동 
   -> /home/cubrid/db_backup
  2. 생성된 4개 파일
     로컬pc로 내려받은 후 외장하드로 복사, 이파일명 변경(파일명 예(220000)db@~)
   -> db@localhost_indexes
      db@localhost_objects
      db@localhost_schema
      db@localhost_unloaddb.log
```

**매니저를 통한 원복**

```
1. 큐브리드매니저 생행
2. 데이터베이스에서 마우스 우측 버튼 > 데이터베이스 생성 >데이터베이스 이름 등록 next
   -> 데이터베이스 이름 예) keepdb17
3. 생성된 db 마우스 우측 버튼 > 로그인, 데이터베이스 정지 상태로 만듦 > 데이터베이스 관리 > 데이터베이스 로드
   아래와 같이 매핑
 -> v 스키마 db@localhost_schema
    v 데이터 db@localhost_objects
	v 인덱스 db@localhost_indexes
	  문법검사       ->원복 속도 향상
	v OID 사용 안함  ->원복 속도 향상
4. 생성된 db 마우스 우측 버튼 > 데이터베이스 로그인 정보 편집
 -> 백업일자 표시
5. 원복완료
※ 참조 - https://suyou.tistory.com/126 
```
