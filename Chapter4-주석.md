
- [Chapter 4. 주석](#chapter-4-주석)
  - [코드 그 자체로 말하게 하라 - 나쁜 주석을 없애는 리팩터링](#코드-그-자체로-말하게-하라---나쁜-주석을-없애는-리팩터링)
  - [1. 주석은 왜 존재하는가?](#1-주석은-왜-존재하는가)
  - [2. 나쁜 주석: 코드의 냄새](#2-나쁜-주석-코드의-냄새)
    - [2.1 주절거리는 주석](#21-주절거리는-주석)
    - [2.2 오해의 여지가 있는 주석](#22-오해의-여지가-있는-주석)
    - [2.3 의무적으로 다는 주석](#23-의무적으로-다는-주석)
    - [2.4 이력을 기록하는 주석](#24-이력을-기록하는-주석)
    - [2.5 위치를 표시하는 주석](#25-위치를-표시하는-주석)
    - [2.6 닫는 괄호에 다는 주석](#26-닫는-괄호에-다는-주석)
    - [2.7 공로를 돌리거나 저자를 표시하는 주석](#27-공로를-돌리거나-저자를-표시하는-주석)
    - [2.8 주석으로 처리한 코드](#28-주석으로-처리한-코드)
    - [2.9 HTML 주석](#29-html-주석)
    - [2.10 전역 정보](#210-전역-정보)
    - [2.11 너무 많은 정보](#211-너무-많은-정보)
    - [2.12 모호한 관계](#212-모호한-관계)
    - [2.13 함수 헤더](#213-함수-헤더)
    - [2.14 비공개 코드에서 Javadocs](#214-비공개-코드에서-javadocs)
  - [3. 좋은 주석: 드물지만 가치 있는 존재](#3-좋은-주석-드물지만-가치-있는-존재)
    - [3.1 법적인 주석](#31-법적인-주석)
    - [3.2 정보를 제공하는 주석](#32-정보를-제공하는-주석)
    - [3.3 의도를 설명하는 주석](#33-의도를-설명하는-주석)
    - [3.4 결과를 경고하는 주석](#34-결과를-경고하는-주석)
    - [3.5 TODO 주석](#35-todo-주석)
  - [4. 결론](#4-결론)
  - [5. 감상(?)평](#5-감상평)



# Chapter 4. 주석


## 코드 그 자체로 말하게 하라 - 나쁜 주석을 없애는 리팩터링

**"주석은 나쁜 코드를 감추기 위한 향수가 아니다!"**

코드는 주석 없이도 읽기 쉽고 명확해야 합니다. 주석은 오히려 코드의 의도를 왜곡하고, 유지보수를 어렵게 만들 수 있습니다. 이번 챕터에서는 나쁜 주석을 식별하고 제거하는 방법과 좋은 주석을 작성하는 방법에 대해 알아보겠습니다.

## 1. 주석은 왜 존재하는가?

주석은 코드만으로는 충분히 설명하기 어려운 부분을 명확히 하기 위해 사용됩니다. 하지만 주석이 항상 필요한 것은 아닙니다. 오히려 주석 없이도 코드를 이해하기 쉽도록 작성하는 것이 중요합니다.

**나쁜 주석은 코드의 악취이며, 좋은 주석은 드물지만 가치 있는 존재입니다.**

## 2. 나쁜 주석: 코드의 냄새

### 2.1 주절거리는 주석

불필요하게 장황하고 코드의 내용을 그대로 반복하는 주석은 오히려 코드 이해를 방해합니다. 코드 자체를 명확하게 작성하여 주석이 필요 없도록 해야 합니다.

```tsx
// BAD :(
// 사용자 ID를 입력받아서 해당 사용자의 정보를 데이터베이스에서 조회하여 반환하는 함수입니다.
function getUserInfo(userId) {
  // 데이터베이스 연결
  const connection = database.connect();
  // SQL 쿼리 실행
  const result = connection.query(`SELECT * FROM users WHERE id = ${userId}`);
  // 연결 종료
  connection.close();
  // 결과 반환
  return result;
}

// GOOD :)
function getUserInfo(userId) {
  const connection = database.connect();
  const user = connection.query(`SELECT * FROM users WHERE id = ${userId}`);
  connection.close();
  return user;
}

```

### 2.2 오해의 여지가 있는 주석

모호하거나 잘못된 정보를 제공하는 주석은 코드에 대한 오해를 불러일으킬 수 있습니다. 주석은 명확하고 정확하게 작성되어야 하며, 코드와 일치해야 합니다.

```tsx
// BAD :(
// 사용자 정보를 업데이트하는 함수
function updateUserData(userId, data) {
  // 실제로는 사용자 정보를 삭제하는 로직
  database.deleteUser(userId);
}

// GOOD :)
function deleteUser(userId) {
  database.deleteUser(userId);
}

```

### 2.3 의무적으로 다는 주석

단순히 형식적으로 작성된 주석은 아무런 의미가 없습니다. 모든 함수나 변수에 대해 기계적으로 주석을 달 필요는 없습니다.

```tsx
// BAD :(
// 사용자 이름을 반환합니다.
function getUserName(user) {
  return user.name;
}

// GOOD :)
function getUserName(user) {
  return user.name;
}

```

### 2.4 이력을 기록하는 주석

코드 변경 이력을 주석으로 남기는 것은 좋지 않습니다. 버전 관리 시스템을 사용하여 변경 이력을 관리하는 것이 훨씬 효율적입니다.

```tsx
// BAD :(
// 2023-11-01: 함수 추가 (작성자: 홍길동)
// 2023-11-02: 버그 수정 (작성자: 김철수)
function calculateTotalAmount(items) {
  // ...
}

// GOOD :)
// Git 커밋 메시지 활용
// feat: 장바구니 총 금액 계산 기능 추가

// refactor: 계산 로직 오류 수정
function calculateTotalAmount(items) {
  // ...
}

```

### 2.5 위치를 표시하는 주석

코드의 특정 위치를 표시하기 위해 주석을 사용하는 것은 좋지 않습니다. 코드 구조를 개선하거나 적절한 함수/클래스 분리를 통해 코드의 가독성을 높여야 합니다.

```tsx
// BAD :(
// =======================
// 사용자 관련 함수
// =======================
function getUserInfo(userId) {
  // ...
}

function updateUserInfo(user) {
  // ...
}

// =======================
// 상품 관련 함수
// =======================
function getProductInfo(productId) {
  // ...
}

function updateProductInfo(product) {
  // ...
}

// GOOD :)
// user.js
function getUserInfo(userId) {
  // ...
}

function updateUserInfo(user) {
  // ...
}

// product.js
function getProductInfo(productId) {
  // ...
}

function updateProductInfo(product) {
  // ...
}

```

### 2.6 닫는 괄호에 다는 주석

코드 블록의 끝을 표시하는 주석은 불필요합니다. 코드의 들여쓰기를 명확하게 하면 코드 블록의 범위를 쉽게 파악할 수 있습니다.

```tsx
// BAD :(
function calculateTotalAmount(items) {
  let total = 0;
  for (let i = 0; i < items.length; i++) {
    total += items[i].price;
  } // for문 끝
  return total;
} // 함수 끝

// GOOD :)
function calculateTotalAmount(items) {
  let total = 0;
  for (let i = 0; i < items.length; i++) {
    total += items[i].price;
  }
  return total;
}

```

### 2.7 공로를 돌리거나 저자를 표시하는 주석

코드 작성자를 나타내는 주석은 일반적으로 불필요합니다. 버전 관리 시스템을 통해 코드 작성자 정보를 확인할 수 있습니다.

```tsx
// BAD :(
// 작성자: 홍길동
function calculateDiscountRate(price) {
  // ...
}

// GOOD :)
// Git을 통해 작성자 정보 확인 가능
function calculateDiscountRate(price) {
  // ...
}

```

### 2.8 주석으로 처리한 코드

더 이상 사용하지 않는 코드는 주석 처리하는 대신 과감하게 삭제하는 것이 좋습니다. 버전 관리 시스템을 사용하면 삭제된 코드도 쉽게 복구할 수 있습니다.

```tsx
// BAD :(
function calculateTotalAmount(items) {
  // let total = 0;
  // for (let i = 0; i < items.length; i++) {
  //   total += items[i].price;
  // }
  // return total;

  return items.reduce((total, item) => total + item.price, 0);
}

// GOOD :)
function calculateTotalAmount(items) {
  return items.reduce((total, item) => total + item.price, 0);
}

```

### 2.9 HTML 주석

코드 내에 HTML 주석을 사용하는 것은 적절하지 않습니다. 주석은 코드에 대한 설명을 제공해야 하며, HTML 태그와 혼용되어서는 안 됩니다.

```tsx
// BAD :(
function renderUserInfo(user) {
  return `
    <div>
      <!-- 사용자 이름 -->
      <h1>${user.name}</h1>
      <!-- 사용자 이메일 -->
      <p>${user.email}</p>
    </div>
  `;
}

// GOOD :)
function renderUserInfo(user) {
  return `
    <div>
      <h1>${user.name}</h1>
      <p>${user.email}</p>
    </div>
  `;
}

```

### 2.10 전역 정보

특정 코드 블록과 직접적인 관련이 없는 전역 정보를 주석으로 남기는 것은 피해야 합니다. 전역 정보는 별도의 문서나 주석으로 관리하는 것이 좋습니다.

```tsx
// BAD :(
// 이 파일은 사용자 인증 관련 함수들을 포함합니다.
function authenticateUser(username, password) {
  // ...
}

function logoutUser() {
  // ...
}

// GOOD :)
// README.md 파일에 명시
// ## 사용자 인증

// - `authenticateUser(username, password)`: 사용자 인증 처리
// - `logoutUser()`: 사용자 로그아웃 처리

function authenticateUser(username, password) {
  // ...
}

function logoutUser() {
  // ...
}

```

### 2.11 너무 많은 정보

지나치게 자세한 설명이나 당연한 사실을 언급하는 주석은 오히려 코드 이해를 방해할 수 있습니다. 주석은 꼭 필요한 정보만 간결하게 제공해야 합니다.

```tsx
// BAD :(
function isAdult(age) {
  // 이 함수는 입력받은 나이가 성인인지 여부를 판단합니다.
  // 한국에서는 만 19세 이상부터 성인으로 간주됩니다.
  // 따라서 age 변수의 값이 19보다 크거나 같으면 true를 반환하고,
  // 그렇지 않으면 false를 반환합니다.
  return age >= 19;
}

// GOOD :)
function isAdult(age) {
  // 한국 기준 성인 여부 판단
  return age >= 19;
}

```

### 2.12 모호한 관계

주석이 어떤 코드를 설명하는지 명확하지 않다면 주석의 효과는 반감됩니다. 주석은 항상 관련된 코드와 가까이 위치해야 하며, 명확한 연관성을 가져야 합니다.

```tsx
// BAD :(
// 데이터 유효성 검증 로직
function processUserData(data) {
  // ...
  return data;
}

// GOOD :)
function processUserData(data) {
  // 데이터 유효성 검증
  if (!isValid(data)) {
    throw new Error("Invalid data");
  }
  // ...
  return data;
}

```

### 2.13 함수 헤더

함수의 기능을 설명하는 긴 헤더 주석은 피해야 합니다. 함수의 이름과 매개변수, 반환 값을 명확하게 작성하면 주석 없이도 함수의 역할을 쉽게 파악할 수 있습니다.

```tsx
// BAD :(
/**
 * 이 함수는 두 개의 숫자를 입력받아서 더한 결과를 반환합니다.
 *
 * @param {number} a - 첫 번째 숫자
 * @param {number} b - 두 번째 숫자
 * @returns {number} 두 숫자의 합
 */
function addNumbers(a, b) {
  return a + b;
}

// GOOD :)
function sum(a, b) {
  return a + b;
}

```

### 2.14 비공개 코드에서 Javadocs

Javadocs는 공개 API 문서를 생성하기 위한 주석 스타일입니다. 내부적으로만 사용되는 비공개 코드에 Javadocs를 사용하는 것은 과도한 문서화입니다.

```tsx
// BAD :(
/**
 * @private
 * @param {number} a - 첫 번째 숫자
 * @param {number} b - 두 번째 숫자
 * @returns {number} 두 숫자의 합
 */
function sum(a, b) {
  return a + b;
}

// GOOD :)
function sum(a, b) {
  return a + b;
}

```

## 3. 좋은 주석: 드물지만 가치 있는 존재

주석은 최대한 자제하는 것이 좋지만, 때로는 주석이 코드의 이해를 돕는 데 유용할 수 있습니다. 좋은 주석은 다음과 같은 특징을 갖습니다.

### 3.1 법적인 주석

저작권이나 라이선스 정보와 같이 법적으로 명시해야 하는 주석은 반드시 포함해야 합니다.

```tsx
// Copyright (c) 2023 by Your Name. All rights reserved.

```

### 3.2 정보를 제공하는 주석

코드의 동작을 간략하게 설명하거나 추가적인 정보를 제공하는 주석은 유용할 수 있습니다.

```tsx
// 정규 표현식을 사용하여 이메일 주소 유효성 검증
function isValidEmail(email) {
  // ...
}

```

### 3.3 의도를 설명하는 주석

코드의 의도를 명확하게 설명하는 주석은 코드의 가독성을 높이는 데 도움이 됩니다.

```tsx
// 사용자의 활동 기록을 업데이트하여 추천 시스템에 활용
function updateUserActivity(userId, activity) {
  // ...
}

```

### 3.4 결과를 경고하는 주석

특정 코드 블록을 수정할 때 주의해야 할 사항이나 발생 가능한 부작용을 경고하는 주석은 유용할 수 있습니다.

```tsx
// 주의: 이 함수는 데이터베이스의 데이터를 직접 수정합니다.
function deleteUser(userId) {
  // ...
}

```

### 3.5 TODO 주석

아직 구현되지 않았거나 개선이 필요한 부분을 나타내는 TODO 주석은 개발 과정에서 유용하게 활용될 수 있습니다.

```tsx
// TODO: 사용자 인증 기능 구현
function loginUser(username, password) {
  // ...
}

```

## 4. 결론

**"주석은 부패하기 쉽다. 주석은 작성 당시에만 정확하며, 시간이 지남에 따라 코드와 일치하지 않을 수 있다."**

주석은 코드를 대체할 수 없습니다. 코드 자체가 명확하고 읽기 쉽도록 작성하는 것이 중요하며, 주석은 코드만으로는 설명하기 어려운 부분을 보완하는 데 사용해야 합니다. 나쁜 주석을 식별하고 제거하는 노력을 통해 코드의 품질을 향상시키고 유지보수를 용이하게 만들 수 있습니다.

## 5. 감상(?)평

주석은 코드를 더 이해하기 쉽게 만드는 것이 아니라, 코드 자체를 이해하기 쉽게 만들어야 한다는 점을 강조하는 글이네요. 나쁜 주석의 유형을 통해 주석을 남발하지 않도록 경각심을 주고, 좋은 주석 작성법을 제시하여 코드 품질 향상에 도움이 될 만한 내용입니다. 전반적으로 주석을 줄이고 깔끔한 코드 작성의 중요성을 일깨워주는 유익한 글이었습니다.
