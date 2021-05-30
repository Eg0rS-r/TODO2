<template>
  <ul class="task-list">
    <transition-group name="slide-top-fade">
      <li v-for="task in list_prop" :key="task.id" class="task-list__item">
        <TaskButton
          @deleteTask="removeTask($event)"
          @newTaskName="renameTask($event)"
          :task_prop="task"
        >
        </TaskButton>
      </li>
    </transition-group>
  </ul>
</template>

<script>
import TaskButton from "../TaskButton.vue";

export default {
  props: ["list_prop"],
  data() {
    return {
      list: this.list_prop,
    };
  },
  components: {
    TaskButton,
  },
  removeTask(task) {
    const index = this.list.findIndex((item) => item.id === task.id);
    if (index !== -1) {
      this.list.splice(index, 1);
    }
  },
};
</script>

<style scoped>
.task-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 0;
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
