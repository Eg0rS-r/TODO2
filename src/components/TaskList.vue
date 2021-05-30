<template>
  <ul class="task-list">
    <transition-group name="slide-top-fade">
      <li v-for="task in tasks_prop" :key="task.id" class="task-list__item">
        <TaskButton @deleteTaskEvent="deleteTask" :task_prop="task" />
      </li>
    </transition-group>
  </ul>
</template>

<script>
import TaskButton from "./TaskButton.vue";

export default {
  props: ["tasks_prop"],
  components: {
    TaskButton,
  },
  data() {
    return {
      tasks: this.tasks_prop
    }
  },
  methods: {
    deleteTask(task) {
      this.$emit('removeTaskEvent', task)
    },
  },
};
</script>

<style scoped>
.task-list {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.task-list__item {
  width: 100%;
  margin-bottom: 10px;
  background-color: var(--white);
}

.task-list__item:last-child {
  margin-bottom: 0;
}
</style>
