# vue-second-app
- 개발환경 세팅
- 폴더 별 파일이 무슨 일을 하는지
- vue에서 쓰이는 개념들


# 개발환경 세팅
1. vue create ~~app
2. menually select features 누르고
3. spacebar로 Babel(default - 구 브라우저 지원), Linter(default - 코딩컨벤션), Router(라우팅처리), Vuex(상태관리) check!
4. vue 버전 뭐로할래? 
    - version 3.x 
5. Use history mode for router? - 라우터에서 히스토리 모드 사용할건지? 
    - Y 엔터
6. 코딩컨벤션 뭐 적용할래? 
    - ESLint + standar config 체크
7. Pick additional lint features - 언제 문법 체크 할까? 
    - Lint on Save 체크
8. Where do you prefer placing config for Babel, ESLint, etc.? - Babel이랑 ESLint 어떻게 관리할래? package.json에서 한번에 관리할래? 아님 따로 파일을 만들래?
    - In package.json 체크
9. Save this as a preset for future projects - 즐겨찾기처럼 설정에 추가할래?


# src/main.js
```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(store).use(router).mount('#app')
```
- npm run serve를 실행하는 순간 제일 먼저 실행된다.
- createApp 이란 모듈을 vue 프레임워크에서 가져온다.
- App 이란 모듈을 src/App.vue 파일을 가져온다
- createApp()함수 안에 App.vue 파일을 넣고, public/index.html에 #app이라는 document안에 mount시키겠다라는 의미

# 페이지 라우팅
### src/App.vue - 템플릿을 보여주는 파일

```html
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>

  <router-view/>
</template>
```

- `<nav>`태그안에 `<router-link to="/">Home</router-link>` 의 의미는 누르면 `"/"`으로 이동한다 라는 뜻이다.
- `<router-view/>`는 라우터의 경로에 맞는 template을 보여준다는 의미이다.

### src/router/index.js - 라우팅 처리를 담당하는 파일
```js
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```
- `vue-router`모듈에서 `createRouter, createWebHistory`를 가져온다. `createRouter`는 router를 생성하게 도와주는 함수이고, `createWebHistory`는 브라우저의 히스토리 모드를 지원하게 도와준다.
- `routes` 배열안에 객체형태로 path, name, component의 키를 넣어줘야한다. path는 대응할 경로를 입력하고, name은 라우트를 구분지어준다. component는 path및 name명이 ~일때 출력될 template를 의미한다.


### `routes`에 `component`에 출력하는 형식이 다른데 왜 다른거죠?
```js
{
path: '/',
name: 'home',
component: HomeView
},
{
path: '/about',
name: 'about',
component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
}
```
- 자세히보면 component에 출력하는 형식이 좀 다르다. 하나는 HomeView라는 component를 값으로 출력하고, 하나는 import()함수에 파일명을 넣어서 출력하게 한다.

## Lazy load
- VueJS같은 SPA 프론트엔드 프레임워크는 현재 보고 있는 화면과 무관한 것들까지 모든 리소스를 한번에 다운받게 된다. 프로젝트가 크다면 리소스를 다운받는것이 느려질 수 있는데, Lazy load는 리소스를 컴포넌트 단위로 구분시키고, 컴포넌트 or 라우터 단위로 필요한 것들만 다운 받을 수 있게 해준다. 렌더링 시간 단축에 쓰이지만 잘못 사용 시 오히려 렌더링 시간이 증가하게 된다. 

## prefetch
- pre+fetch 이며 fetch를 선행한다는 뜻
- VueJS에선 prefetch=true 로 설정되어 있고, 이를 vue.config.js에서 수정이 가능하다.
- 브라우저가 미리 cache(캐시)하는 것으로 브라우저를 사용하는데 빠르다고 느낄 수 있다.

###### prefetch 삭제는... /vue.config.js 파일에 아래 코드 붙여넣기
```js
chainWebpack: config => {
    config.plugins.delete('prefetch'); //prefetch 삭제
}
```

### Lazy load 특징
- Lazy Load가 적용된  Components는 모두 prefetch 기능이 적용 된다. (= 캐시가 미리 저장된다)
- 비동기 컴포넌트로 정의된 모든 리소스들을 캐시에 담기 때문에 요청(request) 수가 증가한다.
- 초기 화면은 오히려 느림
    => 마지막에 다운로드 받기 때문이다.
        => 따라서 초기 렌더링은 prefetch를 사용하지 않는 것이 더 빠르다.
- Vue Application 개발 시 기본적으로 prefetch는 끄는 것을 권장한다.
- vue-router에서 주석으로 처리된 "webpackPrefetch: true" 를 넣어주면 prefetch가 적용된다.
> https://code-hoon.tistory.com/163

###### `routes`의 name은 뭔가요?
```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
- `routes`의 name의 값은 `<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>` 혹은 `router.push({ name: 'user', params: { userId: 123 }})`같이 쓰일 수 있으며 라우터는 이 경우 `/user/123`로 이동된다.


