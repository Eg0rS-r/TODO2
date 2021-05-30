<template>
  <div class="task-button">
    <div class="task-button__view">
      <div class="task-button__content">
        <span class="task-button__name" :class="{'task-button__name--check': task.isCheck}">{{ task.name }}</span>
        <span class="task-button__priority">{{ task.priority }}</span>
        <button
          class="btn-blank task-button__check-button"
          @click="task.isCheck = !task.isCheck"
        >
          <svg
            viewBox="0 0 512 512"
            xmlns="http://www.w3.org/2000/svg"
            class="icon icon--small"
          >
            <path
              v-if="task.isCheck"
              d="m 375 200c 7 7 7 20 0 28l-134 134c-7 7-20 7-28 0l-63-63c-7-7-7-20 0-28 7-7 20-7 28 0l 49 49 120-120c 7-7 20-7 28 0z"
            />

            <path
              d="m512 256c0 141.503906-114.515625 256-256 256-141.503906 0-256-114.515625-256-256 0-141.503906 114.515625-256 256-256 141.503906 0 256 114.515625 256 256zm-40 0c0-119.394531-96.621094-216-216-216-119.394531 0-216 96.621094-216 216 0 119.394531 96.621094 216 216 216 119.394531 0 216-96.621094 216-216zm0 0"
            />
          </svg>
        </button>
      </div>

      <div class="task-button__menu">
        <button
          class="btn-blank task-button__menu-button task-button__menu-button--red"
          @click="$emit('deleteTaskEvent', task)"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["task_prop"],
  data() {
    return {
      task: this.task_prop,
    };
  },
  mounted() {
    const task = document.querySelectorAll(".task-button__view"),
      taskMenu = document.querySelector(".task-button__menu");

    const taskMenuWidth = taskMenu.clientWidth;

    var touchStart = 0,
      touchMove = 0,
      taskPos = 0;

    task.forEach((item) => {
      item.addEventListener("touchstart", (e) => {
        touchStart = e.touches[0].clientX;
      });

      item.addEventListener("touchmove", (e) => {
        e.preventDefault();
        touchMove = -(touchStart - e.touches[0].clientX);
        if (touchMove > -taskMenuWidth && touchMove < 0) {
          item.style.transform = "translateX(" + touchMove + "px)";
        }
      });

      item.addEventListener("touchend", () => {
        taskPos = (touchMove < -taskMenuWidth / 1.2) ? -taskMenuWidth : 0
        item.style.transform = "translateX(" + taskPos + "px)";
        setTimeout(() => {
          item.style.transform = "translateX(" + 0 + "px)";
        }, 5000);
      });
    });
  },
};
</script>

<style scoped>
.task-button {
  border: 1px solid #2c3e50;
  border-radius: 8px;
  overflow: hidden;
}

.task-button__view {
  display: flex;
  transform: translateX(0);
  transition-duration: 0.2s;
}

.task-button__content {
  min-width: 100%;
  padding: 9px 13px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-button__name {
  font-size: 14px;
  padding: 5px 0;
  width: 100%;
}

.task-button__name--check {
  text-decoration: line-through;
  color: var(--font-grey);
}

.task-button__priority {
  color: var(--font-grey);
}

.task-button__check-button {
  margin-left: 8px;
}

.task-button__menu {
  display: flex;
}

.task-button__menu-button {
  padding: 0 7px;
  color: var(--white);
}

.task-button__menu-button--red {
  background-color: #d23242;
}
</style>
