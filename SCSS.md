# 목차

- [목차](#목차)
  - [nested](#nested)
  - [변수](#변수)
    - [css에서는..](#css에서는)
  - [map-get](#map-get)
  - [선택자](#선택자)
    - [다른 예시](#다른-예시)
  - [partials](#partials)
  - [@function](#function)
  - [@mixin](#mixin)
    - [mixin 예제: theme](#mixin-예제-theme)
    - [mixin 예제: mobile (media quary)](#mixin-예제-mobile-media-quary)
  - [@extend](#extend)
  - [수학 연산자 사용 가능](#수학-연산자-사용-가능)

## nested

일반적인 css보다 강력한 scss의 장점은 nested인 것 같다.

```scss
body {
  // ...code
  .main {
    // ...code
  }
}
```

와 같이 쓴다.

## 변수

변수는 파일의 제일 윗부분에 `$변수명`으로 지정해준다
`$primary-color: #272727;`

사용할때도 아래와 같이 깔끔하게 사용가능 하다.

```scss
body {
  background: $primary-color;
}
```

### css에서는..

```scss
:root {
  --primary-color: #272727;
}

body {
  background: var(--primary-color);
}
```

## map-get

`javascript`의 map기능과 비슷하다.
(라고는 하는데 내가 보기엔 object랑 비슷한 거 같다 )

```scss
$font-weights: (
    'regular': 400,
    'medium': 500,
    'bold': 700,
  )
  // 위의 변수를 map-get 을 사용하여 적용한다
  body {
  font-weight: map-get($font-weight, bold);
}
```

아래가 규칙이다.
첫번째 args는 map을 적용하고싶은 변수명, 두번째는 변수의 key.

```
map-get($map: , $key: )
```

## 선택자

`&`는 부모를 가르킨다.

즉

```scss
.main {
  // ...CODE
  &__paragraph {
    // ...code
  }
}
```

는 아래와 같다

```scss
.main {
  //...
}

.main__paragraph {
  //...
}
```

하지만 이렇게 쓰면 우리가 원하는게 아니므로 아래와 같이 수정한다.

```scss
.main {
  // ...CODE
  #{&}__paragraph {
    // ...code
  }
}
```

는 아래와 같다

```scss
.main {
  //...
}

.main .main__paragraph {
  //...
}
```

### 다른 예시

```scss
.main {
  // ...CODE
  #{&}__paragraph {
    // ...code
    &:hover {
      color: pink;
    }
  }
}
```

이렇게 `:hover`와 같은 것을 사용 할 수 있다.

## partials

scss에서는 모듈화로 파일을 깔끔하게 관리 할 수 있다.

변수를 따로 추출해보자.
`_variables.scss`
와 같이 `_파일명`이 컨벤션이다.

그리고 변수파일을 가져오기 위해서는,
`@import './variables'`와 같이 import 구문을 써주면 된다.

## @function

`@mixin`과는 조금 다르다.
이건 계산한 뒤 `value`를 반환한다.
`mixin`은 스타일을 결정한다.

```scss
// function명(변수명)
@function weight($weight-name) {
  @return map-get($font-weights, $weight-name);
}

.main__paragraph {
  // 생성한 function을 적용한다.
  font-weight: weight(regular);
}
```

## @mixin

자주 쓰이는 코드를 반복하지 않기 위해 쓰인다.
`@mixin`과 `@include`는 짝이다.

```scss
@mixin flexCenter {
  display: flex;
  justify-content: center;
  align-items: center;
}

.main__paragraph {
  @include flexCenter;
}
```

또한 `@function`처럼 args를 전달할 수 있다.

```scss
@mixin flexCenter($direction) {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: $direction;
}

.main__paragraph {
  @include flexCenter(row);
}
```

### mixin 예제: theme

```scss
@mixin theme($light-theme: true) {
  @if $light-theme {
    background: lighten($primary-color, 100%);
    color: darken($text-color, 100%);
  }
}

.light {
  // 그냥 theme(true) 라고만 적어도 가능하다
  @include theme($light-theme: true);
}
```

### mixin 예제: mobile (media quary)

```scss
// mixin으로 반복하여 사용하는 모바일쿼리를 생성해보자.
@mixin mobile {
  // 아래의 $mobile 은 변수로 생성한 것.
  // $mobile : 800px
  @media (max-width: $mobile) {
    @content;
  }
}

.main {
  @include mobile {
    // 미디어 쿼리처럼 이제 사용 가능하다
    // 계속 반복되는 미디어 쿼리를 깔끔하게 처리한 것.
    flex-direction: column;
  }
}
```

## @extend

`@extend 상속받을스타일을 한 이름`
으로 스타일을 상속 받을 수 있다.

```scss
.main {
  #{&}__paragraph1 {
    font-weight: weight(bold);
    &:hover {
      color: pink;
    }
  }

  #{&}__paragraph2 {
    // extend만 작성했을 뿐인데 위의 스타일을 그대로
    // 상속받을 수 있다!
    @extend .main__paragraph1;
  }
}
```

## 수학 연산자 사용 가능

보통의 css에서는 `calc(80% - 40%)`를 사용해야하는 반면,
scss에서는 `+`, `-`, `/`, `*`을 그대로 사용 가능 하다

```scss
.main {
  width: 100% - 20%;
}
```

그러나 항상 같은 type으로 써야한다

```scss
.main {
  width: 100% - 200px;
}
```

처럼 `%`을 쓰다가 `px`를 쓰면 작동되지 않는다.
