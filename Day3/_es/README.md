<details>
  <summary>목차</summary>
  <div markdown="1">

- [ES6](#es6)
  - [1. Javascript와 ES*?](#1-javascript와-es)
    - [Javascript ES*?](#javascript-es)
    - [ECMAScript?](#ecmascript)
    - [ECMAScript 6?](#ecmascript-6)
    - [es6 이전에 발생하던 문제](#es6-이전에-발생하던-문제)
    - [ES6 특징](#es6-특징)
    - [ES6의 primitive data type](#es6의-primitive-data-type)
  </div>
</details>

---

# ES6
## 1. Javascript와 ES*?
### Javascript ES*? 
- 자바스크립트 표준화를 위해 ECMAScript로 정리

### ECMAScript?
- 스크립트언어가 어떻게 생겨야 하는지에 관한 사양
- 브라우저는 해당 버전이 올라는 거에 따라 지원하는 기능들이 달라짐
  - ex) 브라우저별 호환성 문제

### ECMAScript 6?
> - ECMAScript 6버전
> - 버전은 매년 하나씩 나옴
>   - ex) 2020년에는 ES11 이었다면 2021년에는 ES12 일것
- 2015년 6월에 업데이트된 ECMAScript의 6th Edition
- [그럼 ES11 or ES12를 말해야지 왜 6버전이 강조되지?](#es6-이전에-발생하던-문제) <small> 유난히 내성적이었던 ES6...</small>
  - 유독 추가된 문법이나 개념이 많았고, 그로인해 지금도 중요하게 여겨짐

### es6 이전에 발생하던 문제
- 자바스크립트 파일별로 겹치는 변수명이 있으면 오류 
  - main.js 와 app.js 두 파일에 a라는 변수가 있으면 나중에 로딩된 자바스크립트 파일로 덮어 쓰여짐
  - 덮여씌여지면 의도한 데이터를 얻을 수 없음
 -> let의 등장으로 해결
 
 ### ES6 특징
- Class 지원 
  - 자칫하면 자바스크립트가 객체지향이라 오해하지만 **자바스크립트는 객체지향언어가 아님** ES6에서 지원된 문법기능으로 [프로토타입 체이닝 참고](#자바스크립트의-prototype)
- let & const 문법 추가 (전역 지역 변수)
- Arrow Functions (람다)
- Modules
- Promises
- 그 외
  - 비구조화 할당 지원
  - 객체 리터럴
  - Template Literals
  - 매개변수 기본값 

### ES6의 primitive data type
