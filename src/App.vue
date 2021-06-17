<template>
  <ListHeader
    v-if="isListOpen"
    :listStarred_prop="selectedList.isStarred"
    :listName_prop="selectedList.name"
    @listStarred="selectedList.isStarred = !selectedList.isStarred"
    @changeListNameEvent="changeListName"
  />
  <ListCatalogue
    v-if="!isListOpen"
    :lists_prop="lists"
    @openListEvent="openList"
    @addNewListEvent="addNewList"
  />

  <ListCatalogue
    v-if="!isListOpen"
    :lists_prop="statisticList"
    @openListEvent="openList"
    @addNewListEvent="addNewList"
  />

  <TaskCatalogue
    v-if="isListOpen"
    :taskList_prop="selectedList.tasks"
    @spliceTaskEvent="spliceTask"
  />
  <ListMenu
    v-if="isListOpen"
    @newTask="addNewTask"
    @closeList="isListOpen = !isListOpen"
    @deleteListEvent="deleteList"
  />
</template>

<script>
import ListHeader from "./components/ListHeader.vue";
import ListCatalogue from "./components/ListCatalogue.vue";
import TaskCatalogue from "./components/TaskCatalogue.vue";
import ListMenu from "./components/ListMenu.vue";

export default {
  name: "App",
  components: {
    ListHeader,
    ListCatalogue,
    TaskCatalogue,
    ListMenu,
  },
  methods: {
    openList(list) {
      this.selectedList = list;
      this.isListOpen = true;
    },
    changeListName(newName) {
      this.selectedList.name = newName;
    },
    addNewTask(newTask) {
      this.selectedList.tasks.push({
        id: this.selectedList.tasks.length,
        name: newTask.name,
        isCheck: false,
        priority: newTask.priority,
      });
    },
    deleteList() {
      const index = this.lists.findIndex(
        (item) => item.id === this.selectedList.id
      );
      if (index !== -1) {
        this.lists.splice(index, 1);
      }

      this.isListOpen = false;
    },
    spliceTask(task) {
      const index = this.selectedList.tasks.findIndex(
        (item) => item.id === task.id
      );
      if (index !== -1) {
        this.selectedList.tasks.splice(index, 1);
      }
    },
    addNewList() {
      this.lists.push({
        name: "List",
        id: this.lists.length,
        isStarred: false,
        tasks: [],
      });
      this.openList(this.lists[this.lists.length - 1]);
      console.log(document.querySelector("header__input"));
      document.querySelector("header__input").focus();
    },
  },
  data() {
    return {
      isListOpen: false,
      selectedList: undefined,
      lists: [
        {
          name: "List 1",
          id: 0,
          isStarred: false,
          tasks: [
            {
              id: 0,
              name: "Example task 1",
              isCheck: true,
              priority: "Month",
            },
            {
              id: 1,
              name: "Example task 2",
              isCheck: false,
              priority: "Day",
            },
          ],
        },
        {
          name: "List 2",
          id: 1,
          isStarred: false,
          tasks: [
            {
              id: 0,
              name: "Another task 1",
              isCheck: false,
              priority: "Month",
            },
            {
              id: 1,
              name: "Another task 2",
              isCheck: false,
              priority: "Week",
            },
            {
              id: 2,
              name: "Another task 2",
              isCheck: true,
              priority: "Day",
            },
          ],
        },
      ],
    };
  },
  watch: {
    selectedList() {
      const index = this.lists.findIndex(
        (item) => item.id === this.selectedList.id
      );
      if (index !== -1) {
        this.lists[index] = this.selectedList;
      }
    },
  },
  computed: {
    statisticList() {
      let taskList = [];
      this.lists.forEach((item) => {
        taskList = taskList.concat(
          item.tasks.filter((element) => {
            return element.isCheck === true;
          })
        );
      });
      
      return [
        {
          name: "Checked",
          id: 9999,
          isStarred: false,
          tasks: taskList,
        },
      ];
    },
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
  color: var(--white);
}

body {
  background-color: #f9f9f9;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: var(--font-black);
}

.btn-blank {
  background: none;
  border: none;
  /* width: 100%; */
}

.input-blank {
  border: none;
  outline: none;
  background: none;
}

.container {
  padding: 20px;
}

.icon {
  width: 34px;
}

.icon--small {
  width: 24px;
}

.icon--starred {
  fill: var(--grey);
  transition-duration: 0.4s;
}

.icon--starred-active {
  fill: var(--yellow);
}

.icon--rotate {
  transform: rotate(180deg);
}

/* Something */

.arrow-img {
  width: 30px;
}

.back-button {
  transform: rotate(180deg);
}

.empty-list {
  text-align: center;
  font-size: 20px;
  margin-top: 30px;
  opacity: 0.7;
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
