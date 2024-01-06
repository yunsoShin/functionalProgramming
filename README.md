# functionalProgramming
함수형 프로그래밍을 공부하며 쓰는 공부노트입니다.


# 자바스크립트에서의 함수형 프로그래밍

## 고차 함수의 사용

자바스크립트에서 함수형 프로그래밍 패러다임을 적용하는 일반적인 방법 중 하나는 "고차 함수"를 사용하는 것입니다. 고차 함수는 다른 함수를 인수로 받거나 함수를 결과로 반환하는 함수입니다. 본문을 함수로 감싸서 넘기는 경우는 대표적으로 콜백 함수나 프로미스, 그리고 async/await 패턴에서 볼 수 있습니다.

### 예제 코드

```javascript
function processData(callback) {
  fetchData().then((data) => {
    const processedData = transformData(data);
    callback(processedData);
  });
}

function displayData(data) {
  console.log('Processed Data:', data);
}

processData(displayData);

```



# 일급 값과 일급 함수

자바스크립트에서는 함수형 프로그래밍을 강력하게 지원하는 개념으로 "일급 값(First-Class Citizens)"과 "일급 함수(First-Class Functions)"가 있습니다. 이들은 코드의 재사용성과 모듈성을 크게 향상시키며, 더욱 유연하고 강력한 프로그래밍을 가능하게 합니다.

## 일급 값(First-Class Citizens)
일급 값이란 프로그래밍 언어에서 변수에 할당할 수 있고, 함수의 인자로 전달할 수 있으며, 함수의 결과로 반환할 수 있는 모든 값입니다. 자바스크립트에서는 숫자, 문자열, 배열, 객체 등 대부분의 것들이 일급 값에 해당합니다. 이러한 특성 덕분에 자바스크립트에서는 데이터를 매우 유연하게 다룰 수 있습니다.

### 예제 코드


```javascript
let number = 42; // 숫자는 일급 값
let text = "Hello World"; // 문자열도 일급 값
let array = [1, 2, 3]; // 배열도 일급 값
let obj = {name: "John"}; // 객체도 일급 값

```

# 일급 함수(First-Class Functions)

자바스크립트에서 함수는 "일급 객체"로 취급됩니다. 이는 함수가 일급 값의 모든 특성을 가진다는 의미이며, 결과적으로 함수는 변수에 할당될 수 있고, 다른 함수의 인자로 전달되거나, 함수의 결과로 반환될 수 있습니다. 이러한 특성은 자바스크립트에서 매우 강력한 프로그래밍 패러다임을 가능하게 합니다.

## 예제 코드

```javascript
// 함수를 변수에 할당
const greet = function(name) {
  return `Hello, ${name}!`;
};

// 함수를 다른 함수의 인자로 전달
function processUserInput(callback) {
  const name = prompt('Please enter your name.');
  alert(callback(name));
}

processUserInput(greet);
```


## 이러한 구조를 통해, 자바스크립트 개발자들은 함수를 더욱 유연하게 사용할 수 있게 되며, 이는 코드의 재사용성과 모듈성을 향상시키는 데 기여합니다. 함수를 일급 객체로 취급함으로써, 개발자는 보다 표현력 있는 코드를 작성할 수 있으며, 복잡한 작업을 더 간결하고 관리하기 쉬운 단위로 나눌 수 있습니다.


# 함수 본문을 콜백함수로 내보내어 리팩토링

함수 본문을 콜백으로 바꾸는 리팩토링은 코드의 특정 부분을 분리하여 더 범용적이고 재사용 가능한 함수를 만드는 과정입니다. 이 방식을 통해 함수를 고차 함수로 만들면, 해당 함수는 더 유연해지며 다양한 상황에 맞게 조정될 수 있습니다.

## 리팩토링 전:

아래의 `performCalculation` 함수는 주어진 배열의 각 요소에 대해 특정 계산(여기서는 각 요소를 2배로 하는 연산)을 수행한 후, 그 결과를 배열로 반환합니다.

```javascript
function performCalculation(numbers) {
  let result = [];
  for (let number of numbers) {
    // 특정 계산 수행
    result.push(number * 2);
  }
  return result;
}

const numbers = [1, 2, 3, 4];
console.log(performCalculation(numbers)); // [2, 4, 6, 8]
```


```javascript
function performCalculation(numbers, operation) {
  let result = [];
  for (let number of numbers) {
    result.push(operation(number));
  }
  return result;
}

const numbers = [1, 2, 3, 4];
const doubled = performCalculation(numbers, x => x * 2);
const squared = performCalculation(numbers, x => x * x);

console.log(doubled); // [2, 4, 6, 8]
console.log(squared); // [1, 4, 9, 16]
```

## 장점

### 재사용성: 고차 함수는 다양한 연산과 상황에 재사용할 수 있습니다. 단지 적절한 콜백 함수를 전달하기만 하면 됩니다.
### 추상화: 구체적인 계산 로직을 함수의 외부로 옮겨, 함수가 보다 일반적인 작업을 수행할 수 있게 합니다. 이는 코드의 추상화 수준을 높여 줍니다.
### 유연성: 나중에 새로운 연산을 추가하고 싶을 때, 기존 함수를 수정하지 않고 새로운 콜백 함수만 제공하면 됩니다. 이는 개방/폐쇄 원칙을 따르는 좋은 예입니다.


## 단점

### 복잡성: 리팩토링을 통해 함수가 보다 범용적이고 유연해지지만, 동시에 코드의 복잡성이 증가할 수 있습니다. 콜백을 사용하는 코드는 때때로 직관적이지 않을 수 있습니다.
### 성능 고려사항: 고차 함수와 콜백을 많이 사용하면 함수 호출 오버헤드가 증가할 수 있으며, 이는 성능에 영향을 미칠 수 있습니다. 대부분의 현대 엔진은 이를 최적화하지만, 매우 성능에 민감한 애플리케이션에서는 고려해야 할 수 있습니다.
### 디버깅의 어려움: 콜백 체인이 길어지면 디버깅이 어려워질 수 있습니다. 오류 추적과 코드 흐름을 파악하기가 더 복잡해질 수 있습니다.이러한 장단점을 고려하여 프로젝트의 요구사항과 맥락에 맞게 리팩토링을 결정하는 것이 중요합니다. 일반적으로 코드의 재사용성과 유지보수성을 높이기 위해 고차 함수와 콜백을 사용하는 리팩토링은 매우 유용할 수 있습니다.





# 함수를 반환하는 함수, 

### 즉 고차 함수는 함수형 프로그래밍에서 매우 중요한 역할을 합니다. 이런 패턴은 코드의 추상화, 재사용성, 모듈성을 크게 향상시킬 수 있습니다.

### 함수를 반환하는 함수의 강력한 힘

### 클로저 생성: 내부 함수는 외부 함수의 스코프에 있는 변수에 접근할 수 있습니다. 이를 통해 데이터를 캡슐화하고, 상태를 안전하게 유지하며, 특정 데이터에 대한 연산을 수행하는 개인화된 함수를 생성할 수 있습니다.부분 적용과 커링: 함수를 반환하는 함수를 사용하면, 일부 인자를 미리 '고정'하고 나머지 인자를 나중에 받을 수 있는 새로운 함수를 만들 수 있습니다. 이는 코드를 보다 유연하게 재사용할 수 있게 해줍니다.

### 지연 평가(Lazy Evaluation): 함수를 반환함으로써 작업을 지연시키고, 필요한 순간까지 계산을 늦출 수 있습니다. 이는 특히 큰 데이터 셋을 다룰 때 성능을 향상시킬 수 있습니다.



## 클로저와 데이터 캡슐화

```javascript
function makeCounter() {
  let count = 0;
  return function() {
    return count++; // 외부 함수의 'count' 변수에 접근
  };
}

const counter = makeCounter();
console.log(counter()); // 0
console.log(counter()); // 1
```

## 부분적용과 커링

```javascript
function multiply(a, b) {
  return a * b;
}

function preMultiplyBy(multiplier) {
  return function(number) {
    return multiply(multiplier, number);
  };
}

const multiplyByTwo = preMultiplyBy(2);
console.log(multiplyByTwo(5)); // 10
```



## 지연평가

```javascript
function lazySum(a, b) {
  const result = () => a + b; // 실제 계산은 함수가 호출될 때까지 지연됩니다.
  return result;
}



function makeCounter() {
  let count = 0;
  return function() {
    return count++; // 외부 함수의 'count' 변수에 접근
  };
}

const counter = makeCounter();
console.log(counter()); // 0
console.log(counter()); // 1


const delayedSum = lazySum(3, 4);
console.log(delayedSum()); // 계산을 실행하고 결과를 얻습니다: 7
```
