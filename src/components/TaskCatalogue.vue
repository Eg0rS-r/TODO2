<template>
  <div class="container">
    <TaskList :tasks_prop="notDoneTaskList" @removeTaskEvent="removeTask" />

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
          <TaskList :tasks_prop="doneTaskList" v-if="isCompleteTaskOpen" />
        </transition>
      </div>
    </transition>
  </div>
</template>

<script>
import TaskList from "./TaskList.vue";

export default {
  props: ["taskList_prop"],
  components: {
    TaskList,
  },
  data() {
    return {
      tasks: this.taskList_prop,
      isCompleteTaskOpen: false,
    };
  },
  methods: {
    removeTask(task) {
      this.$emit("spliceTaskEvent", task)
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
      return this.tasks.filter((e) => {
        return e.isCheck;
      });
    },
    notDoneTaskList() {
      return this.tasks.filter((e) => {
        return !e.isCheck;
      });
    },
  },
};
</script>

<style scoped>
.copmlete-task {
  margin-top: 30px;
}

.complete-task__button {
  width: 100%;
  padding-left: 10px;
  margin-bottom: 18px;
}

.complete-task__title {
  width: fit-content;
  display: flex;
  align-items: center;
  transition-duration: 0.4s;
  font-size: 24px;
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
