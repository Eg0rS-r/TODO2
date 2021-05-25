<template>
  <div class="task-list__button">
    <div class="task">
      <div class="task__content">
        <input
          v-model="task.name"
          type="text"
          class="task__name-input"
        />
        {{task.priority}}
        <button class="btn-blank task__check-button" @click="checkTask">
          <svg
            viewBox="0 0 512 512"
            xmlns="http://www.w3.org/2000/svg"
            class="task-list__check"
          >
            <path
              v-if="task.isCheck"
              class="check"
              d="m 375 200c 7 7 7 20 0 28l-134 134c-7 7-20 7-28 0l-63-63c-7-7-7-20 0-28 7-7 20-7 28 0l 49 49 120-120c 7-7 20-7 28 0z"
            />

            <path
              d="m512 256c0 141.503906-114.515625 256-256 256-141.503906 0-256-114.515625-256-256 0-141.503906 114.515625-256 256-256 141.503906 0 256 114.515625 256 256zm-40 0c0-119.394531-96.621094-216-216-216-119.394531 0-216 96.621094-216 216 0 119.394531 96.621094 216 216 216 119.394531 0 216-96.621094 216-216zm0 0"
            />
          </svg>
        </button>
      </div>

      <div class="task-menu">
        <button class="btn-blank task-menu__button task-menu__button--blue">
          Rename
        </button>
        <button
          class="btn-blank task-menu__button task-menu__button--red"
          @click="deleteTask"
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
      task: this.task_prop
    }
  },
  methods: {
    checkTask() {
      this.$emit("completeTask", this.task);
    },
    deleteTask() {
      this.$emit("deleteTask", this.task);
    },
  },
  mounted() {
    const task = document.querySelectorAll(".task"),
      taskMenu = document.querySelector(".task-menu");

    const taskMenuWidth = taskMenu.clientWidth;

    var touchStart = 0,
      touchMove = 0,
      taskPos = 0;

    var isWait = true;

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
        if (touchMove < -taskMenuWidth / 1.2) {
          taskPos = -taskMenuWidth;
        } else {
          taskPos = 0;
        }
        item.style.transform = "translateX(" + taskPos + "px)";
        if (isWait) {
          setTimeout(() => {
            item.style.transform = "translateX(" + 0 + "px)";
          }, 5000);
        }
      });
    });
  },
};
</script>

<style scoped>
textarea {
  display: block;
  resize: both;
}

.task-list__button {
  border: 1px solid #2c3e50;
  border-radius: 8px;
  overflow: hidden;
}

.task-list__check {
  width: 22px;
}

.task {
  display: flex;
  transform: translateX(0);
  transition-duration: 0.2s;
}

.task__content {
  min-width: 100%;
  padding: 9px 13px;
  text-align: left;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task__name-input {
  border: none;
  font-size: 14px;
  padding: 5px 0;
  width: 100%;
}

.task__check-button {
  padding: 4px;
}

.task-menu {
  display: flex;
}

.task-menu__button {
  padding: 0 7px;
  color: var(--white);
}

.task-menu__button--blue {
  background-color: #006fe6;
}

.task-menu__button--red {
  background-color: #d23242;
}
</style>
