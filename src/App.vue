<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <TaskList />
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TaskList from "./components/TaskList.vue";

export default {
  name: "App",
  components: {
    // HelloWorld,
    TaskList,
  },
  data() {
    return {
      taskList: [
        {
          name: "Example task",
          isCheck: false,
        },
      ],
    };
  },
};
</script>

<style>
/* Colors */
:root {
  --font-black: #2c3e50;
  --font-grey: #465e75;
  --white: #fff;
  --grey: #c5cacd;
  --yellow: #ffcc00;
  --blue: #007bff;
  --red: #dc3545;
}

/* Default */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

ul > li {
  list-style-type: none;
}

input {
  outline: none;
}

[type="radio"]:checked,
[type="radio"]:not(:checked) {
  position: absolute;
  left: -9999px;
}

[type="radio"]:checked + label {
  background-color: var(--blue);
  color: var(--white)
}

svg {
   width: 27px;
}

body {
  background-color: #f9f9f9;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--font-black);
}

.btn-blank {
  background: none;
  border: none;
  /* width: 100%; */
}

/* Something */

.header-input {
  border: none;
  outline: none;
  background: none;
  width: 240px;
  font-size: 35px;
  font-weight: 700;
}

.back-button {
  transform: rotate(180deg);
}

/* Animation */

/* slide bottom fade */
.slide-bottom-fade-enter-from,
.slide-bottom-fade-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

.slide-bottom-fade-enter-to,
.slide-bottom-fade-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.slide-bottom-fade-enter-active,
.slide-bottom-fade-leave-active {
  transition: all 0.4s ease;
}

/* fade */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
}

/* slide bottom fade */
.slide-top-fade-enter-from,
.slide-top-fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

.slide-top-fade-enter-to,
.slide-top-fade-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.slide-top-fade-enter-active,
.slide-top-fade-leave-active {
  transition: all 0.4s ease;
}

/* Dev */
button:focus {
  background-color: #e1e1e1;
}
</style>
