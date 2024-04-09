# [7주차 - Day2] 240409 정리

### 1️⃣ express-generator 프로젝트 구조

- bin/www : 웹 서버를 구축하는 데 필요한 설정 데이터가 정의되어 있는 파일
- node_modules : Node.js, Express에 필요한 모듈들이 설치되는 폴더
- public/ images, javascripts, stylesheets : 정적 파일에 필요한 폴더
- routes : 각 경로를 담당하는 모듈들이 들어 있는 폴더
  ➡️ 라우팅 로직을 구현하는 모듈들
- app.js : 서버의 시작점 ➡️ url에 따라 라우팅을 따로 해줌
- views : 클라이언트에게 html 코드로 “화면을 보내는 파일”
- package.json : 이 프로젝트에 설치된 모듈 이름, 버전 등의 정보들이 작성되어 있는 파일

  ❗️ 백엔드 입장에서 public과 views는 잘 안씀

### 2️⃣ Book Shop 새 프로젝트 생성

- 모듈 설치하기
  ```shell
  npm i express dotenv express-validator jsonwebtoken mysql2
  ```
- app.js 파일 생성
- routes 폴더 생성
  - users.js 파일 생성
  - books.js 파일 생성
  - likes.js 파일 생성
  - carts.js 파일 생성
  - orders.js 파일 생성
- routes 폴더 안 파일들 기본 모듈화한 후 외부 모듈 exports
  ```javascript
  module.exports = router;
  ```
