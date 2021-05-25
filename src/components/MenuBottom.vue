<template>
  <div class="wrapper-task-add">

    <div class="task-add">
      <input
        placeholder="Add new task"
        type="text"
        v-model="newTask.name"
        class="task-add__input"
        @keyup.enter="taskApply"
      />

      <ul class="task-add__priority">
        <li class="task-add__priority-item">
          <input
            type="radio"
            id="Month"
            name="Priority-group"
            v-bind:value="'Month'"
            v-model="newTask.priority"
          />
          <label for="Month">Month</label>
        </li>
        <li class="task-add__priority-item">
          <input
            type="radio"
            id="Week"
            name="Priority-group"
            v-bind:value="'Week'"
            v-model="newTask.priority"
          />
          <label for="Week">Week</label>
        </li>
        <li class="task-add__priority-item">
          <input
            type="radio"
            id="Day"
            name="Priority-group"
            v-bind:value="'Day'"
            v-model="newTask.priority"
          />
          <label for="Day">Day</label>
        </li>
      </ul>

      <button class="btn-blank task-add__button" @click="taskApply">
        <svg
          class="task-add__icon"
          viewBox="0 0 426 426"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="m405.332031 192h-170.664062v-170.667969c0-11.773437-9.558594-21.332031-21.335938-21.332031-11.773437 0-21.332031 9.558594-21.332031 21.332031v170.667969h-170.667969c-11.773437 0-21.332031 9.558594-21.332031 21.332031 0 11.777344 9.558594 21.335938 21.332031 21.335938h170.667969v170.664062c0 11.777344 9.558594 21.335938 21.332031 21.335938 11.777344 0 21.335938-9.558594 21.335938-21.335938v-170.664062h170.664062c11.777344 0 21.335938-9.558594 21.335938-21.335938 0-11.773437-9.558594-21.332031-21.335938-21.332031zm0 0"
          />
        </svg>
      </button>
    </div>
    
    <NavBar />
  </div>
</template>

<script>
import NavBar from "./NavBar.vue";

export default {
  components: {
    NavBar,
  },
  data() {
    return {
      newTask: {
        name: "",
        priority: "",
      },
    };
  },
  methods: {
    taskApply() {
      if (this.newTask != "") {
        const menu = document.querySelector(".wrapper-task-add");
        document.querySelector(".task-add__input").blur();
        this.$emit("newTask", this.newTask);
        this.newTask.name = "";

        menu.style.transform = "translateY(" + -menu.clientHeight + "px)";
      }
    },
  },
  mounted() {
    const menu = document.querySelector(".wrapper-task-add");
    const taskAddInput = document.querySelector(".task-add__input");

    var defaultPos = -37,
      menuHeight = menu.clientHeight,
      nowPos = defaultPos;

    var touchStart = 0,
      touchMove = 0;

    menu.style.transform = "translateY(" + defaultPos + "px)";

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
      if (touchMove < -menuHeight / 1.6) {
        nowPos = -menuHeight;
      } else {
        nowPos = defaultPos;
      }
      menu.style.transform = "translateY(" + nowPos + "px)";
    });

    taskAddInput.onfocus = function () {
      menu.style.transform =
        "translateY(" +
        (-menuHeight + document.querySelector(".nav-block").clientHeight + 25) +
        "px)";
    };
  },
};
</script>

<style scoped>
.wrapper-task-add {
  width: 100%;
  position: fixed;
  padding: 40px 20px 20px;
  top: 100%;
  left: 0;
  background-color: var(--white);
  border-radius: 20px;
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
  box-shadow: 0 7px 30px -10px rgba(125, 142, 150, 0.7);
  transform: translateY(-37px);
  transition-duration: 0.15s;
}

.wrapper-task-add::before {
  content: "";
  position: absolute;
  top: 14px;
  left: 50%;
  width: 35px;
  height: 4px;
  border-radius: 2px;
  background-color: var(--grey);
  transform: translateX(-50%);
}

.task-add {
  display: grid;
  grid-template-columns: 1fr auto;
  grid-template-rows: auto auto;
  margin-bottom: 40px;
}

.task-add__input {
  flex: 1;
  border: none;
  outline: none;
  padding: 9px 13px;
  padding-right: 0;
  margin-left: 2px;
  border: 1px solid var(--grey);
  border-radius: 8px;
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
  background-color: var(--white);
}

.task-add__button {
  grid-row: 1;
  grid-column: 2;
  display: flex;
  padding: 9px 13px;
  background-color: #007bff;
  border-bottom-right-radius: 7px;
  border-top-right-radius: 7px;
}

.task-add__priority {
  grid-column: 1 / 3;
  grid-row: 2;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.task-add__priority-item {
  border: 1px solid var(--grey);
  border-right: none;
  display: flex;
  overflow: hidden;
}

.task-add__priority-item:checked {
  background-color: var(--blue);
}

.task-add__priority-item:first-child,
.task-add__priority-item:last-child {
  border-radius: 8px;
}

.task-add__priority-item > label {
  padding: 7px 12px;
}

.task-add__priority-item:last-child {
  border-top-left-radius: 0;
  border-bottom-left-radius: 0;
  border-right: 1px solid var(--grey);
}

.task-add__priority-item:first-child {
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.task-add__icon {
  width: 22px;
  height: 22px;
  fill: #fff;
}
</style>
