# [7주차 - Day1] 240408 정리

# 📌 도서 API 설계 (추가)

## ✔️ 좋아요 API

### 1️⃣ **좋아요 추가**

likes에 좋아요 행 추가

| HTTP Method      | POST            |
| ---------------- | --------------- |
| URI              | /likes/{bookId} |
| HTTP status code | 성공(200)       |
| Request Body     |                 |
| Response Body    |                 |

### 2️⃣ **좋아요 취소**

// likes에 좋아요 행 삭제

| HTTP Method      | DELETE          |
| ---------------- | --------------- |
| URI              | /likes/{bookId} |
| HTTP status code | 성공(200)       |
| Request Body     |                 |
| Response Body    |                 |

## ✔️ 주문 API

### 1️⃣ **장바구니에서 선택한 주문 상품 목록 조회 (수정)**

request body에 (여러 개) 선택한 장바구니 상품 id들을 리스트로 모아서 보내준다.
| HTTP Method | GET |
| --- | --- |
| URI | /cart |
| HTTP status code | 성공(200) |
| Request Body | [ cartItemId, cartItemId, … ] |
| Response Body | {
[
cartItemId: “장바구니 도서 id”,
bookId: “도서 id”,
title: “도서 제목”,
summary: “요약 정보”,
price: “가격”,
count: “도서 수량”
],
[
cartItemId: “장바구니 도서 id”,
bookId: “도서 id”,
title: “도서 제목”,
summary: “요약 정보”,
price: “가격”,
count: “도서 수량”
], …
} |

## ✔️ 결제 API

### 1️⃣ **결제 요청**

결제를 한다 = 주문 한다 </br>
주문을 하면 주문 DB에 주문결과를 INSERT하게 된다.
| HTTP Method | POST |
| --- | --- |
| URI | /orders |
| HTTP status code | 성공(200) |
| Request Body | {
items: [{
cartItem: “장바구니 도서 id”,
bookId: “도서 id”,
count: 수량
},
{
cartItem: “장바구니 도서 id”,
bookId: “도서 id”,
count: 수량
}, …
],
delivery: {
address: “주소”,
recipient: “수령인”,
contact: “전화번호”
},
totalPrice: 총 금액
} |
| Response Body | |

💡 장바구니에서 주문(결제) 완료된 상품은 장바구니 DB에서 삭제된다.

### 2️⃣ **주문 목록 조회**

배송정보는 delivery에 JSON형식으로 담아서 보내준다.

| HTTP Method      | GET       |
| ---------------- | --------- |
| URI              | /orders   |
| HTTP status code | 성공(200) |
| Request Body     |           |
| Response Body    | [         |

    {
        order_id: “주문 id”,
        orderedDate: “주문 일자”,
        delivery: {
            address: “주소”,
            recipient: “수령인”,
            contact: “전화번호”
        },
        bookTitle: “대표 책 제목”,
        totalPrice: “총 결제 금액”,
        totalCount: “총 수량”
    },
    …

] |

### 3️⃣ **주문 상세 상품 조회 (주문 상세)**

주문이 완료된 도서의 id를 uri로 받고 해당 도서 id에 해당하는 정보를 응답으로 준다.
| HTTP Method | GET |
| --- | --- |
| URI | /orders/{bookId} |
| HTTP status code | 성공(200) |
| Request Body | |
| Response Body | [
{
bookId: “도서 id”,
bookTitle: “도서 제목”,
author: “작가”,
price: “가격”,
count: “수량”
},
{
bookId: “도서 id”,
bookTitle: “도서 제목”,
author: “작가”,
price: “가격”,
count: “수량”
},
…
] |

# 📌 도서 DB 예시

### 1️⃣ **users (회원)**

- id: 회원 id
- username: 회원 이름
- role: 회원 or 관리자
- created_at: 회원 가입일

| id  | username | role | created_at |
| --- | -------- | ---- | ---------- |
| 1   | 짱구     | user | 2024-01-01 |
| 2   | 철수     | user | 2024-01-01 |
| 3   | 훈이     | user | 2024-01-03 |
| 4   | 유리     | user | 2024-01-04 |
| 5   | 맹구     | user | 2024-01-04 |

### 2️⃣ **books (도서 정보)**

- id: 도서 id
- title: 도서 제목
- summary: 요약 정보
- price: 도서 가격
- ... (도서API에서 설계된 정보들)

| id  | title | summary | price | author | pubDate |
| --- | ----- | ------- | ----- | ------ | ------- |
| 1   | book1 | ...     |       |        |         |
| 2   | book2 | ...     |       |        |         |
| 3   | book3 | ...     |       |        |         |
| 4   | book4 | ...     |       |        |         |
| 5   | book5 | ...     |       |        |         |

### 3️⃣ **likes (좋아요)**

- user_id: 좋아요를 한 회원 id
- liked_book_id: 해당 회원이 좋아요를 한 도서 id

| user_id | liked_book_id |
| ------- | ------------- |
| 1       | 1             |
| 1       | 5             |
| 1       | 7             |
| 3       | 1             |
| 4       | 5             |
| 6       | 5             |

### 4️⃣ **cartsItems (장바구니)**

- cart_id: 장바구니 id
- book_id: 장바구니에 담은 도서의 id
- count: 장바구니에 담은 도서 수량

| cart_id(PK) | book_id(FK) | count |
| ----------- | ----------- | ----- |
| 1           | 1           | 1     |
| 2           | 3           | 2     |
| 3           | 2           | 1     |
| 4           |             |       |
| 5           |             |       |
| 6           |             |       |

### 5️⃣ **delivery (배송 정보)**

- id: 배송 id
- address: 배송지
- recipient: 수령인 (주문자)
- contact: 전화번호

| id  | address       | recipient | contact       |
| --- | ------------- | --------- | ------------- |
| 1   | 서울시 송파구 | 강정윤    | 010-1111-2222 |
| 2   | 경기도 용인시 | 김땡땡    | 010-3333-4444 |

### 6️⃣ **orders (장바구니에서 파생된 주문내역)**

- order_id: 주문 id
- delivery_id: 배송 id
- total_price: 총 결제 금액
- created_at: 주문일자
- book_title: 대표 책 제목
- total_count: 주문 도서 총 수량

💡 장바구니에서 선택한 내역을 화면에 출력하는 것

| order_id | delivery_id | total_price | created_at | book_title    | total_count |
| -------- | ----------- | ----------- | ---------- | ------------- | ----------- |
| 1        | 1           | 28000       |            | 대표 책 제목1 | 4           |
|          |             |             |            |               |             |

### 7️⃣ **orderedBook (장바구니에서 결제된 도서 정보)**

- order_id: 주문 id
- book_id: 주문 도서 id
- count: 도서 주문 수량

| order_id | book_id | count |
| -------- | ------- | ----- |
| 1        | 2       | 1     |
| 1        | 3       | 2     |
| 1        | 4       | 1     |
