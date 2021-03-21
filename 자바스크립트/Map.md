# 새로운 타입 Map에 대해 알아보자

[참고](https://blog.webdevsimplified.com/2020-12/javascript-maps/)

dictionary/lookup 와 관련된 작업을 할 때 유용하다.

- ES6에서 소개된 **새로운 자료구조인 Map**을 사용한다
- [mdn map 참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)
- Map 객체는 키-값 쌍을 저장하며 각 쌍의 삽입 순서도 기억하는 콜렉션입니다. 아무 값(객체와 원시 값)이라도 키와 값으로 사용할 수 있습니다.
- 명시적으로 제공한 키 외에 어떠한 키도 없음 (의도치 않은 키 없음)
- Map의 키는 함수, 객체 등을 포함한 모든 키 가능
  - Object는 string 또는 symbol이어야함
- Map의 키는 정렬이 가능함. 순회는 삽입 순으로 이뤄짐
  - Object는 정렬되지 않음
- Map.prototype.size를 통해 쉽게 항목수를 알아 낼 수 있음
- Map은 순회가능 하므로 바로 순회 할 수 있음.
  - Object를 순회하려면 먼저 모든 키를 알아낸 후, 그 키의 배열을 순회해야 합니다.
- 잦은 키-값 쌍의 추가와 제거에서 더 좋은 성능을 보임.

## Object vs Map

### Key Type

- 객체는 string만 키타입으로 가질 수 있다.
- Map은 숫자, 객체, 불리언, 함수 등 모든 타입을 키로 가질 수 있다.

```js
const map = new Map();
map.set('a', 'b');
map.set(1, 'b'); // 숫자 허용
map.set({ key: 'value' }, 'obj'); // obj 허용
// Map(1) { { key: 'value' } => 'obj' }

console.log([...map.keys()]);
// // [ 'a' ]
console.log(map.keys());
// //  [Map Iterator] { 'a' }
console.log(...map);
// // [ 'a', 'b' ]
console.log(map);
// Map(1) { 'a' => 'b' }
// Map(1) { 1 => 'b' }
```

### 순서

- 객체의 키 순서를 정확하게 신뢰할 수 없다.
- 맵은 배열과 비슷하게, 입력된 순서를 기억한다.
  - 이건 Map을 반복할 때 유용하다.

### 반복

- 맵은 객체보다 훨씬 쉽게 작업할 수 있다.
- 객체는 반복기능이 내장되어 있지 않아서, 키/값 쌍을 반복하려면 특정코드를 사용해야한다.
- 맵은 반복가능해서 바로 forEach와 같은 메소드를 사용할 수 있다.

### 길이

- 객체의 길이를 재는 것은 쉽지 않다. 매번 코드를 작성해야한다.
- 맵은 size라는 property가 있기 때문에 (배열과 비슷하게) 바로 측정이 가능하다.

### 성능

- 빈번한 키/값 쌍의 추가/제거에 최적화 되어 있다.
- 객체는 추가/제거에 최적화되어있지 않음.

## Map 사용 방법

### 생성하기

맵은 클래스이기때문에 매개변수를 넣지않으면 그냥 빈 배열과 마찬가지로 빈 맵이 된다. 기본값이 있길 원한다면 반복가능한 값(배열안에 배열)을 넣어주면 된다. 배열의 첫번째 값은 키이고, 두번째 값은 값이다. 그렇게 한쌍이 키/값 쌍이다.

```js
const map = new Map([
  ['key', 'value'], // 배열 안에 배열(key/value)를 넣는다!
  ['hi', 'ho'],
]);

console.log(map);
// Map(1) { 'key' => 'value' }
```

### setting values

맵에 값을 추가하는 방법은 `set`을 사용하여 key/value를 넣어주면 된다.

```js
const map = new Map();
map.set('key', 'value');
map.set(true, 'boolean');

// Map(2) { 'key' => 'value', true => 'boolean' }
```

### getting values

`get`에 key를 넣으면 value를 반환한다.
해당하는 key가 없다면 undefined를 반환한다.

```js
console.log(map.get('key'));
// value
```

### checking for values

`has`를 이용하여 해당하는 키가 있는지 확인할 수 있다.

```js
const map = new Map([
  ['key', 'value'],
  ['hi', 'ho'],
]);

console.log(map.has('hi')); // key인 'hi'는 true 반환
console.log(map.has('ho')); // value : 'ho'는 false 반환
```

```js
const map = new Map();
map.set(1, 'number');
console.log(map.has(1)); // true
console.log(map.has('1')); // false
```

타입도 확인하기 때문에 스트링인 '1'은 false를 반환한다.

### removing values

crud의 마지막 종착역인 삭제기능은 아주 쉽다.
그냥 `delete`에 삭제하고 싶은 키를 args로 전달하면된다.

```js
const map = new Map();
map.set(1, 'number');
console.log(map.has(1)); // true
map.delete(1); // 삭제
console.log(map.has(1)); // false
```

### 맵 반복하기

많은 방법이 있지만 `forEach`가 가장 흔한다.
배열의 `forEach`와 동일하게 작동하지만, 콜백에 두개의 매개변수가 있다. 하나는 value, 하나는 key이다.
value가 앞에 있고, 뒤에있는게 key인 것에 명심하자.

```js
const map = new Map();
map.set(1, 'number');
map.set('a', 'b');

map.forEach((value, key) => {
  console.log(`${key} => ${value}`);
});
// 1 => number
// a => b
```

### 다른 메소드

- size
  - `map.size`로 크기를 잴 수 있다.
- keys()
  - key만 반복한다.
- values()
  - 해당 맵의 values만 반복한다.
- entries()
  - 배열내의 배열을 키/값 쌍을 반환한다.
  - `[Map Entries] { [ 1, 'number' ], [ 'a', 'b' ] }`
  - 원래: `Map(2) { 1 => 'number', 'a' => 'b' }`
- clear()
  - 모든 키/값 쌍을 지울 수 있다.
