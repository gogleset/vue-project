# vue-first-app
- 처음 맛보기 앱
## 설치 익스텐션(VSC)
1. Vetur
2. vue 3 snippets

## 실행방법
- npm run serve
- yarn serve

# View (src / App.vue)
- `<template>`태그 안에 html 작성
- `<script>`태그 안에 js 작성
- `<style>`태그 안에 css 작성

## 데이터 보관 방법
```html
<!-- 모든 데이터 변수가 이 안에 담긴다 (object 자료형)-->
<script>
export default {
  name: 'App',
  // 모든 데이터 보관함
  data(){
    return {
      data: 60,
    }
  },
  components: {
  }
}
</script>
```

## 데이터 출력 방법
```html
<template>
    <div>
        <h4>data = </h4>
        <p>{{data}}</p>
    </div>
</template>
```
- `{{ }}` 안에 `export default {}` 안 data()함수 안에 있는 리턴되는 object들을 출력 해줄수 있다. 
- Vue 실시간 자동 렌더링 기능 사용 가능

### html 속성에 데이터 바인딩 하고 싶어요!

```html
<h4 class="red" :style="style">원룸샵</h4>

<script>
export default {
  name: 'App',
  // 모든 데이터 보관함
  data(){
    return {
      style: "color : blue",
    }
  },
  components: {
  }
}
</script>
```

- html 속성명 앞에 `:` 붙히고 `{{}}`없이 선언해준 object key값을 문자열로 넣어주면 된다.

---

# 반복문

```html
<a href="" v-for="작명 in 4" :key="작명"></a>
```
- vue의 HTML 반복문 `<태그 v-for="작명 in 몇회" :key="작명">` 이런식으로 반복할 수 있음
- `:key`는 무조건 넣어야 한다. 구분하기 위한 속성으로 쓰인다.


# 동적 반복문

```html
<template>
  <a href="" v-for="(item,index) in menu" :key="index">{{item}}</a>
</template>

<script>
export default {
  name: "App",
  // 모든 데이터 보관함
  data() {
    return {
      menu:['Home', 'Products', 'About'],
    };
  },
  components: {},
};
</script>
```
- array 자료형이나, object 자료형을 집어넣는게 가능하다. 
- `<태그 v-for="작명변수 in array|object" :key="작명변수">{{작명변수}}</태그>`
- 자료 안 데이터 갯수만큼 반복
- 작명한 변수는 데이터 안의 자료가 된다.
- 예시에선 Home, Products, About이 출력된다.
- `v-for`에는 두가지 요소를 쓸 수 있는데 첫번째 요소는 배열의 요소가 반환되고 두번째 요소는 배열이 돌수록 정수 1이 증가되는 정수가 반환된다. 그래서 :key값엔 두번째 배열의 요소를 반환시켜 쓰기도 한다.



# 이벤트 감지

```html
<template>
  <button @click="count++">카운터 증가</button><span>카운터 수 : {{ count }}</span>
</template>

<script>
export default {
  name: "App",
  // 모든 데이터 보관함
  data() {
    return {
      count: 0,
    };
  },
};
</script>
```
- `@`events 같은 vue에서 제공하는 이벤트 리스너가 있다 ex) `@click`, `@mouseover` 등등..
- @evets = ""안에 실행하고 싶은 기능을 넣어주면 된다.


# 함수 호출


```html
<template>
  <button @click="countPlus">카운터 증가</button><span>카운터 수 : {{ count }}</span>
</template>

<script>
export default {
  name: "App",
  // 모든 데이터 보관함
  data() {
    return {
      count: 0,
    };
  },
  // 모든 함수 보관함
  methods: {
    countPlus() {
      // this를 붙여야 data안에 있는 count에 접근 가능하다.
      return this.count++;
    },
  },
};
</script>
```
- 함수 선언은 자리가 정해져있다. `data()`함수 밑 `method:{}`안에 작성하면 된다. (없으면 만들자)
- `data()` 함수 안 데이터에 접근하고 싶을 시 `this`를 붙여줘야 접근가능하다.
- @events = ""안에 함수명만 적어주면 실행된다.


# 조건부 렌더링
```html
<template>
  <div class="black-bg" v-if="modal">
    <div class="white-bg" @click="modal = false">
      <h4>상세페이지</h4>
      <p>상세페이지 내용임</p>
    </div>
  </div>
  <div @click="modal = true">
  </div>
</template>
<script>
export default {
  name: "App",
  // 모든 데이터 보관함(state)
  data() {
    return {
      modal: false,
    }
  },
};
</script>

```
- vue에선 `v-if`로 조건부 렌더링이 가능하다.
- `v-if`는 true일때만 렌더링