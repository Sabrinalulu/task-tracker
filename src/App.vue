<template>
  <div class="container">
    <Function />
    <Header @toggle-add-task="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask" />

    <router-view :showAddTask="showAddTask"></router-view>
    <Footer />
  </div>
</template>

<script>
import { reactive, ref, defineComponent } from "vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Function from "./components/Tab.vue";

export default defineComponent({
  name: "AppServer",
  components: {
    Header,
    Footer,
    Function,
  },
  setup() {
    let tasks = reactive([]);
    let showAddTask = ref(false);

    const toggleAddTask = () => {
      showAddTask.value = !showAddTask.value;
    };
    return {
      tasks,
      showAddTask,
      toggleAddTask,
    };
  },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Crimson+Pro");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Poppins", sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
