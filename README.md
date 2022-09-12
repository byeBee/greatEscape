# greatEscape
### Springframework 기반 여행사 웹페이지 구현
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
