<template>
  <div class="wrapper-task">
    

    <ListSeparate :list_prop="notDoneTaskList" />

    <transition name="fade">
      <div v-if="doneTaskList.length != 0" class="copmlete-task">
        <button
          @click="isCompleteTaskOpen = !isCompleteTaskOpen"
          class="btn-blank complete-task__button"
        >
          <h3
            class="complete-task__title"
            :class="{ 'complete-task__title--active': isCompleteTaskOpen }"
          >
            Complete
          </h3>
        </button>
        <transition name="slide-bottom-fade">

          <ListSeparate :list_prop="doneTaskList" v-if="isCompleteTaskOpen"/>

        </transition>
      </div>
    </transition>
  </div>
</template>

<script>
import MenuBottom from "./MenuBottom.vue";
import ListSeparate from "./ListSeparate.vue";
// import TaskButton from "./TaskButton.vue";

export default {
  name: "TaskList",
  components: {
    // TaskButton,
    ListSeparate,
    MenuBottom,
  },
  data() {
    return {
      isCompleteTaskOpen: false,
      isListStarred: false,
      listName: "List 1",
      taskList: [
        {
          id: 0,
          name: "Example task 1",
          isCheck: false,
          priority: "Month",
          // dateCreate: new Date(2021, 4, 19),
        },
        {
          id: 1,
          name: "Example task 2",
          isCheck: false,
          priority: "Day",
          // dateCreate: new Date(2021, 4, 18),
        },
      ],
    };
  },
  methods: {
    checkTask(task) {
      task.isCheck = !task.isCheck;
    },
    
  },
  watch: {
    doneTaskList() {
      if (this.doneTaskList.length === 0) {
        this.isCompleteTaskOpen = false;
      }
    },
  },
  computed: {
    doneTaskList() {
      return this.taskList.filter((e) => {
        return e.isCheck;
      });
    },
    notDoneTaskList() {
      return this.taskList.filter((e) => {
        return !e.isCheck;
      });
    },
  },
};
</script>

<style scoped>
.wrapper-task {
  padding: 0 20px;
}


.complete-task__button {
  width: 100%;
  padding: 5px 0;
  padding-left: 10px;
}

.complete-task__title {
  width: fit-content;
  display: flex;
  justify-content: center;
  transition-duration: 0.4s;
  font-size: 1.25rem;
  opacity: 0.8;
}

.complete-task__title::before {
  content: "";
  display: inline-block;
  width: 20px;
  height: 20px;
  margin-right: 10px;
  -webkit-mask-image: url("../assets/image/icon/arrow-down.svg");
  background-color: var(--font-black);
  background-size: cover;
  transition-duration: 0.4s;
}

.complete-task__title--active {
  opacity: 1;
}

.complete-task__title--active::before {
  transform: rotate(180deg);
}
</style>
