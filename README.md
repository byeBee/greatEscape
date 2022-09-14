# greatEscape
### 프로젝트 개요
#### 프로젝트 명 : GreatEscape 
- 인원: 4명
- 제작 기간 : 2022-08-08 ~ 2022-09-04 (약 26일)
- 사용 기술 : Java / JavaScriptES6 / HTML5 / CSS3 / My-SQL / STS3
- 주요 기능  
	+ 회원가입,회원,비회원 예약기능 
	+ 관리자계정으로 상품등록,예약관리,웹사이트 관리기능
	+ 고객센터(자주묻는질문,공지사항,질문과답변 게시판) 
	+ 여행상품 검색 기능 및 기타 기본적인 여행사 웹사이트에서 필요한 기능들
#### 제작 목적 
- 코로나19로 인해 침체 되어있던 여행시장에서 조금씩 규제가 완화되고 국제선 운행이 시작되며, '갇혀있던 일상에서 도망가자' 라는 <br/>의미에서 여행사 웹페이지를 제작 
- 담당 역할 : DB 테이블 설정 및 검수, 고객센터 파트 백엔드 개발 및 프론트 수정, 오류테스트
#### 개발환경
- 개발Tool

![개발도구](https://user-images.githubusercontent.com/107594290/190176165-857319b3-cf72-4468-96cc-b6204d7f90e6.png)
- 개발언어

![개발언어](https://user-images.githubusercontent.com/107594290/190176200-9a766435-0cd9-4121-9ea7-5f65d4d1cf17.png)
- 프로젝트 관리

![프로젝트관리](https://user-images.githubusercontent.com/107594290/190176214-dcf1e345-f795-49d6-9e74-628b53e4c1ae.png)
****
## 주요 개발 담당: 고객센터 파트(자주묻는질문, 질문과 답변, 공지사항)
<h2 >메인페이지</h2>

![main](https://user-images.githubusercontent.com/107594290/190180797-41efefb2-6bb8-4060-9927-18c0bf248710.png)

*****
# 고객센터
## 자주묻는질문
![1 0-비로그인-faq](https://user-images.githubusercontent.com/107594290/189573281-df773a05-8eea-4102-8e82-ca63f56a5adf.png)
- 헤더의 고객센터 클릭시 자주묻는질문 게시판으로 이동
![메뉴이동](https://user-images.githubusercontent.com/107594290/189573647-3a3bed83-63fa-4a08-9aae-3851e5326d10.png)
- 카테고리별로 버튼 클릭시 해당 내용만 나오도록 구성
![1 4검색기능](https://user-images.githubusercontent.com/107594290/189574087-6f065915-d0ec-422d-a3f8-04e7b6adb26e.png)
- 통합검색으로 FAQ제목과 내용 중 키워드가 포함되면 검색결과에 포함되도록 구현
![x클릭](https://user-images.githubusercontent.com/107594290/189574531-f8deca15-cddc-4e1a-8e07-bfb035d668ad.png)
![2 4faq삭제](https://user-images.githubusercontent.com/107594290/189574168-69151f75-10be-4f10-9f36-9986725670b4.png)
- 관리자의 경우에만 게시글 추가, 삭제가 가능 [X]클릭시 Modal창을 띄워주고 삭제 클릭시 삭제
## 질문과답변
### 관리자 로그인
![질문답변](https://user-images.githubusercontent.com/107594290/189583251-d1413b36-5f6a-41ad-9f19-ccd59594b351.png)
### 일반회원 로그인
![문답내꺼아님안보여](https://user-images.githubusercontent.com/107594290/189586688-dcf2f64b-a4fe-4594-ba46-73b3a4b01ec8.png)
- 위에서 3개의 글까지는 공지사항을 최신순으로 보여주도록 구현
- selectbox에서 확인중, 답변완료를 선택하면 해당 글만 표시되도록 구현
- 관리자, 작성자 본인 외에는 글을 확인할수 없도록 구현
- jstl function 라이브러리를 이용해 작성자의 이름을 처음과 끝글자만 표시되고, 가운데는 * 마스킹되도록 구현 
- jstl formatter 라이브러리를 이용해 작성일을 요일까지만 표시(DB에서는 TIMESTAMP, java에서는 Date로 저장)
### 해당글 상세보기
![질문답변상세](https://user-images.githubusercontent.com/107594290/189584053-9666e33c-6ab8-4982-be55-427d0c30be97.png)
![답변달린글](https://user-images.githubusercontent.com/107594290/189584368-099a800c-b593-4c09-b2aa-bcf08c1ad477.png)
- 관리자의 경우에만 답글을 달수 있는 에디터가 표시되고, 등록을 클릭시 해당 글에 바로 표시되도록 AJAX로 리스트를 받아오도록 구현
- 관리자의 답이 달리면 카테고리를 확인중->답변완료로 변경
+ Controller
```java
@GetMapping("getCommentList")
@ResponseBody
public List<CommentVO> getCommentList(@RequestParam("qna_num")int qna_num)throws Exception{
	CommentVO vo= new CommentVO();
	vo.setQna_num(qna_num);
	return bs.getCommentList(qna_num);
}
```
+ javascript
```javascript
function getCommentList(){
	var qna_num= $('input[name=qna_num]').val();
	$.ajax({
		type: 'GET', url:"getCommentList", data:{"qna_num":qna_num},
		beforeSend : function(xhr) { 
			xhr.setRequestHeader("${_csrf.headerName}", "${_csrf.token}");
		}, success: function(result) {
			for(var i=0; i<result.length; i++){
				var str='<div class="comment">';
				str += '관리자 답변';
				str += '</div><div>&nbsp;&nbsp;&nbsp;';
				str += result[i].qna_answer;
				str += '</div><hr/>';
				$("#commentList").append(str);
			}
		},error: function(err){
			console.log(err);
		} 
	});//ajax			
}
```
### 질문글쓰기, 나의 글 보기
![로그인필요함](https://user-images.githubusercontent.com/107594290/189587552-2f67df08-c808-4203-9ee3-beb61421068e.png)

- 비로그인시 로그인페이지로 이동
<img src="https://user-images.githubusercontent.com/107594290/189587947-0d80ae63-2b34-4b1e-84ff-bab56649e230.png" width="700" height="500"/> 

![내가쓴글](https://user-images.githubusercontent.com/107594290/189587937-661cfc77-f20a-45c4-bb26-de7561687d2a.png)

- 에디터를 사용해 글쓰기 구현
- 로그인정보를 바탕으로 내가 쓴 질문만 
## 공지사항
