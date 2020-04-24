<template>
  <div id="app">
    <h1>{{ title.name }}</h1>
    <div>{{ user }}</div>
    <button @click="updateUser">修改名称</button>
    <div>当前count：{{ computedCount }}</div>
    <button @click="increment">修改count</button>
  </div>
  <HellWorld msg="hello"></HellWorld>
</template>

<script>
import { reactive, ref, onMounted, computed, watchEffect, watch } from "vue";
import HellWorld from "./components/HelloWorld.vue";
export default {
  name: "App",
  components: {
    HellWorld
  },
  setup() {
    const title = reactive({
      name: "Hello world"
    });
    const user = ref("vue3");
    //如果需要修改值，可通过value

    const updateUser = () => {
      user.value = "vue3-ref变量已更新";
      title.name += "title更新了";
    };

    //生命周期方法
    onMounted(() => {
      console.log("init mounted...");
    });
    // 初始化count值
    const count = ref(0);
    const increment = () => {
      count.value++;
    };
    // 调用计算属性函数Hook
    const computedCount = computed(() => count.value * 2);

    //响应数据改变会立即触发该监听函数
    watchEffect(() => {
      console.log("watchEffect监听count变化", count.value);
      console.log("watchEffect监听title变化", title.name);
    });

    //watch可以监听单一数据源，也可以监听多个数据源 ([],[])
    watch(count, (count, prevCount) => {
      console.log("watch监听count", count, prevCount);
    });
    return { title, user, updateUser, count, increment, computedCount };
  }
};
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
