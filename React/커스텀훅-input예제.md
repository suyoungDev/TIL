[참고링크](https://kyounghwan01.github.io/blog/React/custome-hook/#%ED%9B%85-%EC%93%B0%EA%B8%B0-%EC%A0%84-%EB%A1%9C%EC%A7%81)

# 커스텀 훅

`스타일드 컴포넌트`를 반복해서 재사용하는 것과 마찬가지로, `custom hook`을 사용하면서 반복되는 로직을 재사용해서 코드의 양을 줄이고 더 읽기 쉽게 한다.

## 사용 전

```js
import React, { useState, useCallback } from 'react';

const [input, setInput] = useState({
  email: '',
  password: '',
});

const { email, password } = input;

const onChange = useCallback(
  (e) => {
    const { value, name } = e.target;
    setText({ ...input, [name]: value });
  },
  [input]
);

return (
  <>
    <input name='email' value={email} onChange={onChange} />
    <input name='password' value={password} onChange={onChange} />
  </>
);
```

이렇게 긴 함수를 매번 input이 필요할 때마다 반복해서 쓰는건 불필요하다.

## 커스텀 훅 생성

- 커스텀 훅의 이름은 `use`으로 시작하는게 약속이다.

```js
import { useState, useCallback } from 'react';

export default (initalValue = null) => {
  const [input, setInput] = useState(initalValue);

  const handler = useCallback(
    (e) => {
      const { name, value } = e.target;
      setData({
        ...input,
        [name]: value,
      });
    },
    [input]
  );
  return [input, handler];
};
```

## 커스텀 훅 반영

```js
// Login.jsx
import React from 'react';
import useInput from 'useInput';

// state
const [text, setText] = useInput({
  email: '',
  password: '',
});

return (
  <>
    <input name='email' value={text.email} onChange={setText} />
    <input name='password' value={text.password} onChange={setText} />
  </>
);
```

### 장점

- useInput 커스텀 훅 안에 `useState`과 `onChange함수`인 `useCallback`를 미리 정의 했기 때문에, 컴포넌트에서는 훅을 사용하기만 하면 된다.

- `useState`, `onChange` 함수가 없어져서 코드양도 적어지고, 저 커스텀 훅을 다른 컴포넌트에서도 재활용 가능하기 때문에 동일한 로직을 수행하는 컴포넌트가 많을 경우 커스텀 훅을 더욱 유용히 사용할 수 있다.
