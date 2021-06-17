<template>
  <div class="menu">
    <div class="task-add">
      <input
        class="input-blank task-add__input"
        placeholder="Add new task"
        type="text"
        v-model="newTask.name"
        @keyup.enter="taskApply"
      />

      <ul class="task-priority">
        <li class="task-priority__item">
          <input
            type="radio"
            id="month"
            name="priority-group"
            v-bind:value="'Month'"
            v-model="newTask.priority"
            checked
          />
          <label class="task-priority__label" for="month">Month</label>
        </li>
        <li class="task-priority__item">
          <input
            type="radio"
            id="week"
            name="priority-group"
            v-bind:value="'Week'"
            v-model="newTask.priority"
          />
          <label class="task-priority__label" for="week">Week</label>
        </li>
        <li class="task-priority__item">
          <input
            type="radio"
            id="day"
            name="priority-group"
            v-bind:value="'Day'"
            v-model="newTask.priority"
          />
          <label class="task-priority__label" for="day">Day</label>
        </li>
      </ul>

      <button
        class="btn-blank task-add__button"
        @click="taskApply"
        aria-label="Add new task"
      >
        <svg
          class="icon icon--small"
          viewBox="0 0 426 426"
          xmlns="http://www.w3.org/2000/svg"
          style="fill: white"
        >
          <path
            d="m405.332031 192h-170.664062v-170.667969c0-11.773437-9.558594-21.332031-21.335938-21.332031-11.773437 0-21.332031 9.558594-21.332031 21.332031v170.667969h-170.667969c-11.773437 0-21.332031 9.558594-21.332031 21.332031 0 11.777344 9.558594 21.335938 21.332031 21.335938h170.667969v170.664062c0 11.777344 9.558594 21.335938 21.332031 21.335938 11.777344 0 21.335938-9.558594 21.335938-21.335938v-170.664062h170.664062c11.777344 0 21.335938-9.558594 21.335938-21.335938 0-11.773437-9.558594-21.332031-21.335938-21.332031zm0 0"
          />
        </svg>
      </button>
    </div>

    <div class="list-option">
      <button
        class="btn-blank"
        @click="$emit('closeList')"
        aria-label="Close list"
      >
        <img
          class="icon icon--rotate"
          src="../assets/image/icon/arrow-right.svg"
          alt="Arrow right icon"
        />
      </button>
      <button
        class="btn-blank"
        @click="$emit('deleteListEvent')"
        aria-label="Delete list"
      >
        <img
          class="icon"
          src="../assets/image/icon/trash.svg"
          alt="Trash icon"
        />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: {
        name: "",
        priority: "Month",
      },
    };
  },
  methods: {
    taskApply() {
      if (this.newTask !== "") {
        document.querySelector(".task-add__input").blur();
        this.$emit("newTask", this.newTask);
        this.newTask.name = "";

        const menu = document.querySelector(".menu");
        menu.style.transform = "translateY(" + -menu.clientHeight + "px)";
      }
    },
  },
  mounted() {
    const menu = document.querySelector(".menu");
    const taskAddInput = document.querySelector(".task-add__input");

    var defaultPos = -37,
      menuHeight = menu.clientHeight,
      nowPos = defaultPos;

    var touchStart = 0,
      touchMove = 0;

    menu.addEventListener("touchstart", (e) => {
      touchStart = e.touches[0].clientY;
      document.querySelector(".task-add__input").blur();
    });

    menu.addEventListener("touchmove", (e) => {
      e.preventDefault();
      touchMove = -(touchStart - e.touches[0].clientY);
      if (touchMove > -menuHeight) {
        menu.style.transform = "translateY(" + (defaultPos + touchMove) + "px)";
      }
    });

    menu.addEventListener("touchend", () => {
      nowPos = touchMove < -menuHeight / 1.6 ? -menuHeight : defaultPos;
      menu.style.transform = "translateY(" + nowPos + "px)";
    });

    taskAddInput.onfocus = function () {
      menu.style.transform =
        "translateY(" +
        (-menuHeight +
          document.querySelector(".list-option").clientHeight +
          25) +
        "px)";
    };
  },
};
</script>

<style scoped>
.menu {
  width: 100%;
  position: fixed;
  top: 100%;
  left: 0;
  padding: 40px 20px 20px;
  background-color: var(--white);
  border-radius: 20px;
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  box-shadow: 0 7px 30px -10px rgba(125, 142, 150, 0.7);
  transform: translateY(-37px);
  transition-duration: 0.15s;
}

.menu::before {
  content: "";
  width: 35px;
  height: 4px;
  border-radius: 2px;
  position: absolute;
  top: 14px;
  left: 50%;
  transform: translateX(-50%);
  background-color: var(--grey);
}

.task-add {
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-rows: auto auto;
  /* margin-bottom: 40px; */
}

.task-add__input {
  padding: 9px 13px;
  border: 1px solid var(--grey);
  border-radius: 8px;
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
}

.task-add__button {
  grid-row: 1;
  grid-column: 2;
  display: flex;
  padding: 9px 13px;
  background-color: #007bff;
  border-radius: 8px;
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}

/* Task Priority */
.task-priority {
  grid-column: 1 / 3;
  grid-row: 2;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.task-priority__item {
  border: 1px solid var(--grey);
  border-right: none;
  display: flex;
  overflow: hidden;
}

.task-priority__item:first-child,
.task-priority__item:last-child {
  border-radius: 8px;
}

.task-priority__item:last-child {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  border-right: 1px solid var(--grey);
}

.task-priority__item:first-child {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.task-priority__item:checked {
  background-color: var(--blue);
}

.task-priority__label {
  padding: 7px 12px;
}

.list-option {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
