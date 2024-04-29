# [8주차 - Day4] 240418 정리

## 1️⃣ Node.js 비동기

- 비동기: 실행되는 코드가 기다려야하는 시간이 생김
- ex) setTimeOut(), setInterval(), query()

## 2️⃣ 비동기 처리

코드를 순서대로 실행함. 비동기가 필요없을 때 (쿼리를 기다릴 때와 같이) 사용

- 콜백함수
- promise (resolve, reject)
- then & catch
- ES2017 promise ➡️ async, await

## 3️⃣ Promise 객체

```javascript
// Promise 객체 (약속을 지킴)
let promise = new Promise(function (resolve, reject) {
  // executor (성공하면 resolve, 실패하면 reject)
  // 할 일을 다 하면 두 콜백함수 중 하나를 무조건 호출
  setTimeout(() => resolve("성공"), 3000);
});

// then : promise 기본 메서드 (성공 후 호출하는 함수)
promise.then(
  function (result) {
    console.log(result);
  },
  function (error) {}
);
```

- promise chaining
  ```javascript
  let promise = new Promise(function (resolve, reject) {
    setTimeout(() => resolve("1"), 3000);
  })
    .then(
      function (result) {
        console.log(result);
        return result + "2";
      },
      function (error) {}
    )
    .then(
      function (result) {
        console.log(result);
        return result + "3";
      },
      function (error) {}
    )
    .then(
      function (result) {
        console.log(result);
      },
      function (error) {}
    );
  ```
- 결과
  ![8-4-1](../img/8주차_img/8-4-1.png)

## 4️⃣ async & await

```javascript
// async, await
// 1. Prmoise 객체 반환 (then method를 await로)
// 2. Promise 객체가 일이 끝날 때까지 기다릴 수 있는 공간 제공
async function f() {
  let promise = new Promise(function (resolve, reject) {
    setTimeout(() => resolve("완료"), 3000);
  });

  let result = await promise;
  // promise 객체가 일을 다 할 때까지 여기서 기다림
  console.log(result);
}

f();
```

- 세 개의 일을 비동기로 처리

  ```javascript
  // async, await
  // 1. Prmoise 객체 반환 (then method를 await로)
  // 2. Promise 객체가 일이 끝날 때까지 기다릴 수 있는 공간 제공
  async function f() {
    let promise1 = new Promise(function (resolve, reject) {
      setTimeout(() => resolve("첫 번째"), 3000);
    });

    let result1 = await promise1;
    // promise 객체가 일을 다 할 때까지 여기서 기다림
    console.log(result1);

    let promise2 = new Promise(function (resolve, reject) {
      setTimeout(() => resolve("두 번째"), 3000);
    });

    let result2 = await promise2;
    console.log(result2);

    let promise3 = new Promise(function (resolve, reject) {
      setTimeout(() => resolve("세 번째"), 3000);
    });

    let result3 = await promise3;
    console.log(result3);
  }

  f();
  ```

- 각각의 결과가 3초마다 실행됨
  ![8-4-2](../img/8주차_img/8-4-2.png)

