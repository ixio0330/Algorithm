# 문제 해결 접근 방법

문제 해결 능력 향상을 위해선 두 가지 방법이 있습니다.

1. 문제 해결을 위한 계획 수립
2. 문제 해결 패턴 파악

먼저 1번 문제 해결을 위한 계획 수립에 대해 알아보겠습니다.

문제 해결 방법은 5가지로 이루어져 있습니다.

1. 문제 이해
2. 구체적인 예시
3. 문제 세분화
4. 해결/단순화
5. 복습/재구성

하나씩 어떤 방식으로 문제를 해결 해 나가는지 알아보겠습니다.

## 첫 번째, 문제 이해하기

1. 문제를 내 방식대로 다시 생각할 수 있는가?
2. 문제가 어떤 입력값을 담고 있는가 이해했는가?
3. 문제에 대한 해결책에서 나와야할 출력값은?
4. 입력값이 출력값을 결정할 수 있는가? (문제를 해결할 충분한 정보를 갖고 있는가?)
5. 문제의 일부인 데이터의 중요한 부분에 어떻게 라벨을 지정할 수 있을까?

## 두 번째, 구체적인 예시 작성하기

문자열을 인자로 받아서 char의 개수를 객체에 담아 반환하는 함수를 만든다고 가정하겠습니다.

입력값과 출력값의 예시를 순서대로 두 개, 혹은 세 개 작성합니다.

```
charCount('aaaa') // {a: 4}
charCount('hello') // {h:1, e:1, l:2, o:1}
charCount('my phone number is 12345678') // ?
charCount('Hello hi') // ?
```

이렇게 작성하고 나면, 띄어쓰기, 대소문자 구분은 어떻게 처리해야 하는지 등의 여러 의문점이 생깁니다. 의문을 갖은 상태로 다음 단계로 넘어갑니다.

## 세 번째, 문제를 세분화하기

문제에 대한 단계들을 실제로 수행하면서 작성하는 단계입니다. 해결책의 기본적인 구성 요소만 작성합니다.

우선 함수의 뼈대를 구성합니다.

```
function charCount(string) {
  const result = {};
  // do something...
  return result;
}
```

이제 살을 붙여 나갑니다.

```
function charCount(string) {
  const result = {};
  // 모든 char을 다 체크해야 하기 때문에 string의 길이 만큼 반복합니다.
  for (let i = 0; i < string.length; i++) {
    const char = string[i];
    // result 객체에 char 키가 존재 한다면 1을 더하고
    if (result[char]) {
      result[char] += 1;
    }
    // result 객체에 char 키가 존재하지 않는다면 1로 설정
    else {
      result[char] = 1;
    }
  }
  return result;
}
```

함수를 실행시켜서 결과를 볼까요?

```
const result = charCount('hello');
console.log(result);
```

console에 { h: 1, e: 1, l: 2, o: 1 } 이렇게 출력이 됩니다. 하지만 아직 끝나지 않았습니다. 앞서 구체적인 예시를 작성하면서 띄어쓰기, 대소문자 구분을 어떻게 처리해야 한다는걸 알게 되었고, 이를 해결해야 합니다.

우선 대소문자 구분을 어떻게 할지부터 정합니다. 대소문자 구분을 하지 않고, count를 세기로 합시다. 이 문제는 javascript 문자열 method인 toLowerCase를 사용해서 해결합니다.

```
function charCount(string) {
  const result = {};
  for (let i = 0; i < string.length; i++) {
    const char = string[i].toLowerCase();
    if (result[char]) {
      result[char] += 1;
    }
    else {
      result[char] = 1;
    }
  }
  return result;
}
```

이번에는 대문자를 포함해서 실행시킵니다.

```
const result = charCount('HelLo');
console.log(result);
```

아까와 같이 { h: 1, e: 1, l: 2, o: 1 }이 찍힙니다.

그럼 이제 특수문자는 어떻게 처리 할지 고민해야 합니다. 여러 방법이 있는데, 특수문자를 담은 배열을 만들고 각 char마다 특수문자가 담긴 배열을 순회하면서 같은지 확인 한 후 특수문자가 아닐 경우에만 추가하면 되겠죠?

하지만 이 방법은 너무 번거롭습니다. Javascript는 정규 표현식(regular expression)을 사용할 수 있습니다.

영어 문자열과 숫자만 담은 정규 표현식을 만들어서, js에서 제공하는 test method를 통해 특수문자가 아닌지 확인한 후 객체에 넣으면 됩니다.

```
function charCount(string) {
  const result = {};
  const regex = /[a-z0-9]/;
  for (let i = 0; i < string.length; i++) {
    const char = string[i].toLowerCase();

    if (regex.test(char)) {
      if (result[char]) {
        result[char] += 1;
      } else {
        result[char] = 1;
      }
    }
  }
  return result;
}
```

이번에는 띄어쓰기를 포함해서 실행시킵니다.

```
const result = charCount('HelL    o');
console.log(result);
```

아까와 같이 { h: 1, e: 1, l: 2, o: 1 }이 찍힙니다.

## 네 번째, 해결하고 단순화하기

하지만 코드가 조금 길어보이네요. 짧은 코드라고 해서 좋은 코드는 아니겠지만, 가독성을 위해 단순화 해보겠습니다.

```
function charCount(string) {
  const result = {};
  const regex = /[a-z0-9]/;
  for (let i = 0; i < string.length; i++) {
    const char = string[i].toLowerCase();

    if (regex.test(char)) {
      result[char] ? (result[char] += 1) : (result[char] = 1);
    }
  }
  return result;
}
```

Js의 삼항연산자를 사용했습니다.

## 다섯 번째, 복습/재구성 (리팩토링)

리팩토링을 할 때에는 여러 질문을 해볼 수 있습니다.

- 결과를 확인할 수 있는가?
- 결과를 다른 방식으로 도출할 수 있는가? (문제에 대한 해결책이 하나만 있지 않기 때문)
- 해결책이 얼마나 직관적인가?
- 결과나 방법을 다른 문제에도 적용할 수 있는가? (문제 해결 능력의 이점은 직감을 발달시켜 다른 문제를 해결할 수 있는 직관력을 길러줌)
- 해결책의 성능을 향상시킬 수 있는가?
- 다른 사람들은 어떻게 문제를 해결하는가?

위에서 사용한 정규표현식은 특정 패턴을 빠르게 확인할 수 있기 때문에 많이 사용되지만, 실제로 얼마나 효율적인지는 모릅니다. 또한 Js에서는 정규 표현식이 수행 중인 작업과 사용 주인 브라우저에 따라 성능이 달라질 수 있다고도 합니다.

정규표현식이 아닌 charCodeAt를 사용하는 방법도 있습니다. 강의자가 제공한 사이트에 의하면 정규표현식은 charCodeAt보다 55%더 느리다고 합니다.

```
function isAlphaNumberic(char) {
  let code = char.charCodeAt(0);
  if (
    !(code > 47 && code < 58) &&
    !(code > 96 && code < 123) &&
    !(code > 64 && code < 91)
  ) {
    return false;
  }
  return true;
}

function charCount(str) {
  const result = {};
  for (let char of str) {
    if (isAlphaNumberic(char)) {
      char = char.toLowerCase();
      // result[char]이 존재한다면 더해주고, 아니면 1으로 설정
      result[char] = ++result[char] || 1;
    }
  }
  return result;
}
```

문자열과 숫자인지 파악하는 함수를 따로 분리했습니다. result객체에 연산을 해주는 부분도 수정했습니다. 이제 완성입니다!

## 결론

초보가 처음부터 완벽한 코드를 짜는건 매우 어렵습니다. 초보일 경우 성급하게 코드를 작성하는 것 보단, 어려운 부분을 파악하는 것이 우선입니다. 이를 파악하면 코드를 연결할 수 있도록 올바른 부분, 적합한 위치에 둘 수 있는데 빠르게 코드를 작성하는 것보다 훨씬 좋습니다!
