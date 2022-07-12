<template>
  <div class="black-bg" v-if="modal">
    <div class="white-bg">
      <h4>상세페이지</h4>
      <p>상세페이지 내용임</p>
      <button @click="modal = false">닫기</button>
    </div>
  </div>
  <div>
    <div class="menu">
      <a href="" v-for="(item, index) in menu" :key="index">{{ item }}</a>
    </div>
    <div v-for="(item, index) in products" :key="index">
      <img :src="item.image" :alt="item.title" class="room-image" />
      <h4 @click="modal = true">{{ item.title }}</h4>
      <span>{{item.content}}</span>
      <p>{{item.price}}</p>
      <button @click="count[index]++">허위매물신고</button
      ><span>신고수 : {{ count[index] }}</span>
    </div>
  </div>
</template>

<script>
import data from "./assets/json/data";
export default {
  name: "App",
  // 모든 데이터 보관함
  data() {
    return {
      modal: false,
      count: [0, 0, 0],
      style: "color: red",
      menu: ["Home", "Products", "About"],
      products: data,
    };
  },
  // 모든 함수 보관함
  methods: {
    countPlus() {
      return this.count++;
    },
  },
  components: {},
};
</script>

<style>
body {
  margin: 0;
}
div {
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.menu {
  background-color: orange;
  padding: 15px;
  border-radius: 5px;
}
.menu a {
  color: white;
  padding: 10px;
}
.room-image {
  width: 300px;
  margin-top: 30px;
}
.black-bg {
  position: fixed;
  z-index: 1;
  width: 100%;
  height: 100%;
  padding: 20px;
  background-color: rgba(0, 0, 0, 0.15);
}
.white-bg {
  width: 300px;
  margin: 0 auto;
  height: 300px;
  background-color: white;
  padding: 20px;
}
</style>
