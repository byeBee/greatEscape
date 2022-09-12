# greatEscape
### Springframework 기반 여행사 웹페이지 구현
#### 프로젝트 개발환경
+ Mybatis ORM 사용
+ spring-security 적용
+ Springframework v5.3.21
+ My-SQL v8.0.29
****

## 메인페이지
![mainpage](https://user-images.githubusercontent.com/107594290/189569907-562287e8-b8be-4efc-b32d-7726eb0d78c2.png)
- 헤더부분의 [로그아웃 관리자페이지] 부분은 관리자, 일반회원, 비로그인 여부에따라 변경되도록 jstl core 라이브러리를 이용해 jsp페이지 구성
- 헤더의 각 부분을 클릭하면 해당하는 페이지로 이동할수 있도록 Controller와 View페이지를 구성
- 여행상품 검색에는 Provider를 이용해 동적쿼리를 구성하여 각각의 조건을 모두 고려할수 있게 구성
```java
public String getListBySearch(Map<String, String> map) {
		SQL sql = new SQL();
		sql.SELECT("*");
		sql.FROM("tbl_product");
		String country = map.get("country");
		if (country != null && !country.equals("")) {
			sql.WHERE("product_country LIKE CONCAT('%', #{country}, '%')");
		}
		String departure = map.get("departure");
		if (departure != null && !departure.equals("")) {
			sql.WHERE("DATE(product_departure) = DATE(#{departure})");
		}
		String plan = map.get("plan");
		if (plan != null && !plan.equals("")) {
			sql.WHERE("product_plan = #{plan}");
		}
		String seat = map.get("seat");
		if (seat != null && !seat.equals("")) {
			sql.WHERE("(product_seat * 4) >= #{seat}");
		}
		String city = map.get("city");
		if (city != null && !city.equals("")) {
			sql.WHERE("product_city LIKE CONCAT('%', #{city}, '%')");
		}
		String money = map.get("money");
		if (money != null && !money.equals("")) {
			if (seat != null && !seat.equals("")) {
				sql.WHERE("product_adult * #{seat} <= #{money}");
			} else {
				sql.WHERE("product_adult <= #{money}");
			}
		}
		return sql.toString();
	}
```
*****
# 회원관리
## 로그인 및 회원가입
![로그인](https://user-images.githubusercontent.com/107594290/189579119-58b77d76-02b6-456e-97d9-ad91147d0a06.png)
![회원가입](https://user-images.githubusercontent.com/107594290/189579142-4abcb861-8ddf-430c-bfcb-1147e8a84e41.png)
- DB에서 ID 중복검사및 비밀번호 등 유효성검사
- DAUM 주소찾기 API 적용
## 아이디비밀번호찾기 

![비번찾기](https://user-images.githubusercontent.com/107594290/189579300-4fe5e649-2503-4b76-b9ae-5b5410e09820.png)
- spring-security 에 의해 암호화된 비밀번호가 DB에 저장되므로 DB데이터로는 기존 비밀번호를 찾을수 없기 때문에 메일로 임시 비밀번호를 발송하여 그 번호로 로그인 후 회원이 직접 수정할수 있도록 구현 
- 가입시 입력한 이메일을 통한 인증
- 비밀번호의 경우 spring-
## 
*****
# 고객센터
![1 0-비로그인-faq](https://user-images.githubusercontent.com/107594290/189573281-df773a05-8eea-4102-8e82-ca63f56a5adf.png)
- 헤더의 고객센터 클릭시 자주묻는질문 게시판으로 이동
![메뉴이동](https://user-images.githubusercontent.com/107594290/189573647-3a3bed83-63fa-4a08-9aae-3851e5326d10.png)
- 카테고리별로 버튼 클릭시 해당 내용만 나오도록 구성
![1 4검색기능](https://user-images.githubusercontent.com/107594290/189574087-6f065915-d0ec-422d-a3f8-04e7b6adb26e.png)
- 통합검색으로 FAQ제목과 내용 중 키워드가 포함되면 검색결과에 포함되도록 구현
![x클릭](https://user-images.githubusercontent.com/107594290/189574531-f8deca15-cddc-4e1a-8e07-bfb035d668ad.png)
![2 4faq삭제](https://user-images.githubusercontent.com/107594290/189574168-69151f75-10be-4f10-9f36-9986725670b4.png)
- 관리자의 경우에만 게시글 추가, 삭제가 가능 [X]클릭시 Modal창을 띄워주고 삭제 클릭시 삭제
