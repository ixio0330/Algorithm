# Big-O

Big-O는 함수를 비교하고 성능을 판단하는 방법이다.

Big-O를 사용하지 않고, 어떻게 함수의 성능을 판단할 수 있을까?

## 코드 실행 시간 재보기

숫자를 인자로 받아서, 1부터 해당 숫자까지의 합을 구하는 두 함수가 있습니다.

첫번째 함수인 addUpTo1은 반복문 for을 사용해서 결과를 도출해 냅니다.
두번째 함수인 addUpTo2는 수학 공식을 사용해서 결과를 도출해 냅니다.

두 함수 모두, 함수를 실행한 시간을 출력하고 있습니다.

```
function addUpTo1(n) {
    let total = 0;
    for (let i = 1; i <= n; i++) {
      total += i;
    }
    return total;
  }

const addUpTo1Time1 = performance.now();
console.log(addUpTo1(100));
const addUpTo1Time2 = performance.now();
console.log(`Time Elapse: ${(addUpTo1Time2 - addUpTo1Time1) / 1000}s.`);

function addUpTo2(n) {
    return (n * (n + 1)) / 2;
}

const addUpTo2Time1 = performance.now();
console.log(addUpTo2(100));
const addUpTo2Time2 = performance.now();
console.log(`Time Elapse: ${(addUpTo2Time2 - addUpTo2Time1) / 1000}s.`);
```

실행 결과 1부터 100까지의 합을 구하는데 첫번째 함수는 0.003175000011920929초, 두번째 함수는 0.003112999975681305초가 걸렸습니다.

하지만 이런 방식으로 코드의 성능을 파악하는건 매우 큰 단점들이 있습니다.

1. 시간을 측정해서 비교할 수 없이 빠른 알고리즘이 있다면, 시간으로 측정하는 것은 무의미합니다.
2. 1시간이 걸리는 코드, 4시간이 걸리는 코드를 각각 테스트를 실행하는건 낭비입니다.

**시간을 측정하지 않고 좋은 코드인지 파악하는 방법은 없을까요?**

### 컴퓨터가 처리해야하는 연산 갯수를 세면 됩니다!

```
function addUpTo1(n) {
  let total = 0;
  for (let i = 1; i <= n; i++) {
    total += i;
    return total;
  }
}
```

위 코드는 총 5 × N 번의 연산을 합니다. 인자에 따라 반복문이 수행하는 횟수도 늘어나기 때문입니다.

```
function addUpTo2(n) {
    return (n * (n + 1)) / 2;
}
```

위 코드는 곱하기, 더하기, 나누기 총 3번의 연산을 합니다.

그렇다면 두 함수를 Big-O에 따라 정의해 봅시다.

## Big-O 소개

### Bio-O 정의

N이 커질수록 컴퓨터가 f(n) 상수 곱하기 f(n)보다 간단한 연산을 덜 해야한다면 그 알고리즘을 O(f(n))이라고 표현한다.

### Big-O 설명

빅오는 대략적으로 숫자를 세는 것의 붙인 공식적인 표현이다.

정식으로 입력된 내용이 늘어날수록 알고리즘에 실행 시간이 어떻게 변하는지 설명하는 공식적인 방식이다.

입력의 크기와 실행시간의 관계를 표현한다.

```
function addUpTo1(n) {
  let total = 0;
  for (let i = 1; i <= n; i++) {
    total += i;
    return total;
  }
}
```

다시 addUpTo1 함수를 가져왔습니다. addUpTo1은 총 n번의 연산을 하기 때문에 Big O 표현법으로는 O(n)이라고 표현합니다.

그렇다면 addUpTo2는 어떻게 표현할까요? addUpTo2는 총 3번의 연산을 하기 때문에 O(1)이라고 표현합니다.

Big-O를 표현할 때는 엄청나게 넓은 범주를 생각해서 표기합니다. addUpTo1은 100을 넣으면 5 × 100번의 연산을 수행하지만, 만약 1억이라는 수를 넣게 되면 5 × 1억 번의 연산을 수행합니다.

반면 addUpTo2는 100을 넣든, 1억을 넣든 똑같이 총 세 번의 연산만 수행합니다.

### Big-O 표현식 단순화하기

Big-O 표현식을 생각할 때는 정확히 큰 그림을 보는 것이 중요합니다.

1. O(2n) | O(n)
   ⇒ 상수는 중요하지 않기 때문에 O(n)으로 표현한다.

2. O(500) | O(1)
   ⇒ 어떤 상황이든 500개의 연산이 있기 때문에 O(1)으로 표현한다.

3. O(13n²) | O(n²)
   ⇒ 방대한 수를 통해 그래프로 보면 13n², n², 1000n² 전부 비슷하게 보인다. O(n²)으로 표현한다.

4. O(n+10) | O(n)
   ⇒ 작은 연산들도 중요하지 않다. O(n)으로 표현한다.

5. O(1000n+50) X O(n)
   ⇒ 4번과 같은 이유로 O(n)으로 표현한다.

#### 명심 해야할 점

- 산수는 상수다. 덧셈, 곱셉, 뺄셈, 나눗셈 등 (컴퓨터가 2+2를 처리하는 시간과 100만+2를 처리하는 시간은 비슷함)
- 변수 배정도 상수다. (x=1000를 처리하는 것과 x=20000, 100만 등을 처리하는 시간은 비슷함)
- 인덱스를 사용해서 배열 엘리먼트에 접근하는 것은 배열에서 첫번째 아이템을 찾던지, 10번째 아이템을 찾던지 인덱스를 사용하면 똑같은 시간이 걸린다. 객체의 키로 데이터에 접근하는 실행 시간도 상수다.
- 루프가 있으면 복잡도가 루프의 길이 X 루프안의 연산들

#### 다양한 Big-O 사례

1. O(n)

```
function countUpAndDown(n) {
  console.log('Going up!');
  // O(n)
  for (let i = 0; i <= n; i++) {
    console.log(i);
  }

  console.log(`At the top!
Going down...`);
  // O(n)
  for (let j = n - 1; j >= 0; j--) {
    console.log(j);
  }

  console.log('Back down. Bye!');
  // countUpAndDown(10);
}
```

2. O(n²), O(n^2)

```
function printAllPairs(n) {
  // O(n)
  for (let i = 0; i < n; i++) {
    // O(n)
    for (let j = 0; j < n; j++) {
      console.log(i, j);
    }
  }
}
```

3. O(n)

```
function logAtLeast5(n) {
  for (let i = 0; i <= Math.max(5, n); i++) {
    console.log(i);
  }
}
```

4. O(1)
   function logAtMost5(n) {
   for (let i = 0; i <= Math.min(5, n); i++) {
   console.log(i);
   }
   }

### 공간복잡도 (메모리)

공간복잡도는 입력이 커질수록 얼마나 많은 공간을 차지하는 것인지 뜻한다.

#### JavaScript의 공간복잡도

- boolean, number, undefined, null은 모두 불변 공간
- string은 O(n)공간이 필요
- reference(참조) 타입. 배열과 객체도 O(n)공간이 필요

1. O(1)

```
function sum(arr) {
  let total = 0;
  for (let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  return total;
}
```

total은 number type이기 때문에 공간복잡도에 따르면 O(1)이다.

2. O(n)

```
function dobule(arr) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(2 * arr[i]);
  }
  return newArr;
}
```

newArr는 배열(참조 타입)이기 때문에 공간복잡도에 따르면 O(n)이다.

