# 모델2 방식으로 효율적으로 개발하기


## 🖥️ 프로젝트 소개
- 1.웹 애플리케이션 모델
- 2.MVC 디자인 패턴
- 3.MVC를 이용한 회원 관리
- 4.모델2로 답변형 게시판 구현하기
<br>

## 🕰️ 개발 기간
* 23.06.05일 - 23.06.09일


### ⚙️ 개발 환경
- `Java 11`
- `JDK 1.8
- **Database** : Maria DB(11xe)
- **ORM** : Mybatis

## 📌 주요 기능
#### 회원 정보 조회 기능 구현
- /mem.do 요청
- 서블릿 MemberController가 요청을 받아 MemberDAO의 listMembers() 메서드 호출
- MemberDAO의 listMembers() 메서드에 SQL문으로 회원 정보를 조회한 후 회원 정보를 MemberVO에 설정하여 반환
- 다시 MemberController에서는 조회한 회원 정보를 회원 목록창(listMembers.jsp)으로 포워딩
- 회원 목록창에서 포워딩한 회원 정보를 목록으로 출력
#### 회원 정보 추가 기능 구현
- 회원 가입창에서 회원 정보를 입력하고 URL 패턴을 /member/addMember.do로 서버에 요청
- MemberController에서 getPathInfo() 메서드를 이용해 요청명인 /addMember.do를 받아옴
- 요청명에 대해 MemberDAO의 addMember() 메서드를 호출
- addMember() 메서드에서 SQL문으로 테이블에 회원 정보 추가
#### 회원 정보 수정 및 삭제 기능 구현
- 회원창에서 수정,삭제 클릭하여 /member/modMember.do 컨트롤러 요청.
- 컨트롤러는 요청명을 가져옴.
- 수정을 마친후 다시 회원창, 회원ID를 SQL문으로 전달해 테이블에서 회원 정보 삭제.

####  게시판 글쓰기 구현
- 글 목록창에서 글쓰기창을 요청
- 글쓰기창에서 글을 입력하고 컨트롤러에 글쓰기 요청
- 컨트롤러에서 Service 클래스로 글쓰기창에서 입력한 글 정보를 전달해 테이블에 글을 추가
- 새 글을 추가하고 컨트롤러에서 다시 /board/listAticles.do로 요청하여 전체 글 표시

#### 글 상세 기능 구현
- 글 목록창에서 글 제목을 클릭해 컨트롤러에 글번호로 요청
- 컨트롤러는 전송된 글 번호로 글 정보를 조회하여 글 상세창으로 포워딩
- 글 상세창에 글 정보와 이미지 파일이 표시
#### 글 수정 기능 구현
- 글 상세창에서 수정하기를 클릭해 글 정보를 표시하는 입력창들을 활성화
- 글 정보와 이미지를 수정한 후 수정반영하기를 클릭해 컨트롤러에 요청
- 컨트롤러는 요청에 대해 upload() 메서드를 이용하여 수정된 데이터를 Map에 저장하고 반환
- 컨트롤러는 수정된 데이터를 테이블에 반영한 후 temp 폴더에 업로드된 수정 이미지를 글 번호 폴더로 이동
- 마지막으로 글 번호 폴더에 있던 원래 이미지 파일 삭제

#### 글 삭제 기능 구현
- 글 상세창에서 삭제하기를 클릭하면 요청
- 컨트롤러에서는 글 상세창에서 전달받은 글 번호에 대한 글과 이에 관련되 자식 글들을 삭제
- 삭제된 글에 대한 이미지 파일 저장 폴더도 삭제

#### 글 삭제 기능 구현
- 글 상세창에서 답글쓰기를 클릭하여 부모글 번호를 컨트롤러에 전송
- 답글 쓰기창에서 답변 글을 작성한 후 컨트롤러로 요청
- 컨트롤러에서는 전송된 답글 정보를 게시판 테이블에 부모 글 번호와 함께 추가

#### 게시판 페이징 기능 구현
- 기존 계층형 구조로 글 목록을 일단 조회
- 그 결과에 대해 다시 ROWNUM이 표시되도록 서브 쿼리문을 이용해 다시 한번 조회
- ROWNUM이 표시된 두 번째 결과에서 section과 pageNum으로 계산된 where절의 between 연산자 사이의 값에 해당하는 ROWNUM이 있는 레코드들만 최종 조회
