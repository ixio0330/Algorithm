# 배열과 객체에 대한 성능 평가

## 배열의 Big-O

- insertion: 사정에 따라 다름...
- removal: 사정에 따라 다름...
- searching: O(n)
- access: O(1)

### 배열 method의 Big-O (JavaScript)

- push: O(1)
- pop: O(1)
- shift: O(n)
- unshift: O(n)
- concat: O(n)
- slice: O(n)
- splice: O(n)
- sort: O(N × logN)
- forEach | map | filter | reduce | etc...: O(n)

**insertion, removal이 사정에 따라 다른 이유**
JavsScript는 push로 배열의 맨 끝에 요소를 추가하고, pop으로 배열의 맨 끝에 있는 요소를 삭제합니다.

또한 unshift로 배열의 맨 앞에 요소를 추가하고, shift로 배열의 맨 앞에 있는 요소를 삭제합니다.

예를 들어, ['One', 'Two', 'Three']라는 배열이 있습니다. 만약 배열의 맨 앞에 Zero라는 요소를 추가하기 위해 unshift method를 사용하면 어떻게 될까요?

아마도 아래와 같은 작업이 실행될 것입니다. 배열을 추가하고, 각 index를 1씩 미루어 다시 정렬을 합니다.

```
Zero        One        Tow        Three
             0          1           2
```

```
Zero        One        Tow        Three
             0          1           2
```

```
Zero        One        Tow        Three
  0                     1           2
```

```
Zero        One        Tow        Three
  0          1                      2
```

```
Zero        One        Tow        Three
  0          1          2
```

```
Zero        One        Tow        Three
  0          1          2           3
```

## 객체의 Big-O

- insertion: O(1)
- removal: O(1)
- searching: O(n)
- access: O(1)

### 객체 method의 Big-O (JavaScript)

- Object.keys: O(n)
- Object.values: O(n)
- Object.entries: O(n)
- Object.hasOwnProperty: O(1)
