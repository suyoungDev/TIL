# 여러개의 input 상태 관리

여러개의 `input`을 쓰는 일이 많다.
그때 각각의 `useState`와 `onChange`를 여러개를 만들어 `input`의 상태를 따로 관리하게 되면, 코드의 양이 많아지고 번거로워 지는 법이다.

한번에 관리 하는 방법은, `input`에 `name`을 설정하고 이벤트가 발생했을 때 이 값을 참조하는 것이다. 그리고, `useState`에서는 **문자열**이 아니라 **객체 형태의 상태**를 관리해주어야 한다.

## 여러개 상태 관리하는 방법

```js
import React, { useState, useCallback } from 'react';

const [input, setInput] = useState({
  email: '',
  password: '',
});

const { email, password } = input;
// 비구조화로 추출해야 input에 할당 가능함

const onChange = useCallback(
  (e) => {
    const { value, name } = e.target;
    // e.target에서 value, name을 비구조화로 추출
    setText({ ...input, [name]: value });
    // 위에서 추출한 value와 name값을 새로 넣어준다
  },
  [text]
);

return (
  <>
    <input name='email' value={email} onChange={onChange} />
    <input name='password' value={password} onChange={onChange} />
  </>
);
```

위와 같이 `input value`의 상태를 각각의 `state`로 관리할 필요 없이 하나의 `state`로 관리할 수 있다. 이것만으로도 해당 `input`마다 `state`를 새로 생성하는 것보다 훨씬 깔끔해졌다.

### 포인트

1. `onChange`함수에서 `input`의 `value`와 `name`을 비구조화로 추출해야한다.
   즉, `e.target.name`과 `e.target.value`인 `email`과 `email의 value`가 추출되어 할당이 가능하다.

```js
const { value, name } = e.target;
```

2. 아래와 같이 `[name]` 대괄호로 감싸야 상태 업데이트가 가능하다.

```js
setInput({ ...input, [name]: value });
```
