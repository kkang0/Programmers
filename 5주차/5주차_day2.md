# [5주차 - Day2] 240326 정리

### 1️⃣ Server와 Router의 역할

- Server: 요청을 받음
- Router: 요청의 URL에 따라 어디로 갈지 루트를 정해줌

### 2️⃣ Node.js에서의 라우팅

- 요청이 왔을 때, 원하는 경로에 따라 적절한 방향으로 경로를 안내해주는 것
- URL, method ➡️ 호출 콜백 함수

### 3️⃣ require('모듈이름');

- Server도 모듈처럼 다른 자바스크립트 파일에서 사용 가능

  ```javascript
  const router = express.Router();
  ...
  module.exports = router;  // 모듈화
  ```

- **app.js**가 서버 역할을 하도록 함

  ```javascript
  const express = require("express");
  const app = express();

  app.listen(7777);

  const userRouter = require("./routes/users");
  const channelRouter = require("./routes/channels");

  app.use("/", userRouter);
  app.use("/", channelRouter);
  ```

- route 모듈이름 rename된 상태

### 4️⃣ ERD 만들어보기

- **회원**
  | user_id | password | name |
  | --- | --- | --- |
  | testId1 | 1234 | tester1 |
  | testId2 | 5678 | tester2 |
- **채널**
  | id | channel_name | user_id | sub_num | video_num |
  | --- | --- | --- | --- | --- |
  | 1 | kkang1 | testId1 | | |
  | 2 | kkang2 | testId1 | | |
  | 3 | kkang3 | testid2 | | |

(user_id => 누가 채널을 생성했는지 알려줌)

### 5️⃣ 채널 API 몇 가지 수정하기

1. 채널 “생성” ➡️ **POST** /channels

   - req: body(channelName, userId)
   - res(201): `${channelName}님의 채널이 생성되었습니다.`

2. 회원의 채널 전체 “조회 ➡️ **GET** /channels

   - req: ❌ → body(userId)
   - res(200): 채널 전체 데이터 리스트 or json or 배열

- userId는 body가 아닌 header로 숨겨서 받아와야 함

### 6️⃣ 채널(channels) 수정

채널 전체 조회

```javascript
.get((req, res) => {
    let { userId } = req.body;
    let channels = []; // json 객체 생성

    // db에 값이 존재하고 userId가 body에 있으면
    if (db.size && userId) {
      db.forEach(function (value, key) {
        if (value.userId === userId) {
          channels.push(value);
        }
      });

      // userId가 가진 채널이 있으면
      if (channels.length) {
        // 채널이 존재
        res.status(200).json(channels);
      } else {
        // 채널이 존재하지 않음
        notFoundChannel();
      }

      return;
    }

    // db에 값이 한 개도 없다면
    notFoundChannel();
  })
```

### 7️⃣ 회원 API 수정

3. 회원 개별 정보 조회 ➡️ **GET** /users/:id
   - req: body (userId)
   - res: id, name
4. 회원 개별 탈퇴 ➡️ **DELETE** /users/:id
   - req: body (userId)
   - res: `${name}님 다음에 또 뵙겠습니다.` or 메인페이지

- body에 userId를 담아서 보내주기!

### 8️⃣ 회원(users) 수정

회원 개별조회와 회원 개별탈퇴 모두 `/users/:id` 대신 `/users`만 받아오고 userId는 body로 보내주는 것으로 변경

```javascript
let { userId } = req.body;
const user = db.get(userId);
```
