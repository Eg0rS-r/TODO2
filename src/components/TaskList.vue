<template>
  <div class="wrapper-task">
    <header class="list-header">
      <h1 class="list-header__title">
        <input
          type="text"
          v-model="listName"
          maxlength="12"
          class="header-input"
          @keyup.enter="headerInputBlur"
        />
      </h1>
      <button
        class="btn-blank star-button"
        :class="{ 'star-button--active': isListStarred }"
        @click="isListStarred = !isListStarred"
      >
        <svg
          version="1.1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          viewBox="0 0 512 512"
        >
          <g>
            <g>
              <path
                d="M511.267,197.258c-1.764-5.431-6.457-9.389-12.107-10.209l-158.723-23.065L269.452,20.157c-2.526-5.12-7.741-8.361-13.45-8.361c-5.71,0-10.924,3.241-13.451,8.361l-70.988,143.827l-158.72,23.065c-5.649,0.82-10.344,4.778-12.108,10.208c-1.765,5.431-0.293,11.392,3.796,15.377l114.848,111.954L92.271,482.671c-0.966,5.628,1.348,11.314,5.967,14.671c2.613,1.898,5.708,2.864,8.818,2.864c2.388,0,4.784-0.569,6.978-1.723l141.967-74.638l141.961,74.637c5.055,2.657,11.178,2.215,15.797-1.141c4.619-3.356,6.934-9.044,5.969-14.672l-27.117-158.081l114.861-111.955C511.56,208.649,513.033,202.688,511.267,197.258z"
              />
            </g>
          </g>
        </svg>
      </button>
    </header>
    <ul class="task-list">
      <transition-group name="slide-top-fade">
        <li
          v-for="task in notDoneTaskList"
          :key="task.id"
          class="task-list__item"
        >
          <TaskButton
            @deleteTask="removeTask($event)"
            @newTaskName="renameTask($event)"
            @completeTask="checkTask($event)"
            :task_prop="task"
          >
            {{ task.name }}
          </TaskButton>
        </li>
      </transition-group>
    </ul>

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
          <ul v-if="isCompleteTaskOpen" class="task-list task-list--complete">
            <transition-group name="slide-bottom-fade">
              <li
                v-for="task in doneTaskList"
                :key="task.id"
                class="task-list__item"
              >
                <TaskButton
                  @deleteTask="removeTask($event)"
                  @newTaskName="renameTask($event)"
                  @completeTask="checkTask($event)"
                  :task_prop="task"
                >
                  {{ task.name }}
                </TaskButton>
              </li>
            </transition-group>
          </ul>
        </transition>
      </div>
    </transition>
    <MenuBottom @newTask="addNewTask($event)" />
  </div>
</template>

<script>
import MenuBottom from "./MenuBottom.vue";
import TaskButton from "./TaskButton.vue";

export default {
  name: "TaskList",
  components: {
    TaskButton,
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
          priority: 'Month',
          // dateCreate: new Date(2021, 4, 19),
        },
        {
          id: 1,
          name: "Example task 2",
          isCheck: false,
          priority: 'Day',
          // dateCreate: new Date(2021, 4, 18),
        },
      ],
    };
  },
  methods: {
    headerInputBlur() {
      document.querySelector('.header-input').blur()
    },
    addNewTask(task) {
      this.taskList.push({
        id: this.taskList.length,
        name: task.name,
        isCheck: false,
        priority: task.priority,
        // dateCreate: new Date(),
      });
    },
    checkTask(task) {
      task.isCheck = !task.isCheck;
    },
    removeTask(task) {
      const index = this.taskList.findIndex((item) => item.id === task.id);
      if (index !== -1) {
        this.taskList.splice(index, 1);
      }
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

.list-header {
  padding-top: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.list-header__title {
  padding-left: 10px;
}

.star-button > svg {
  width: 30px;
  opacity: 0.35;
  transition-duration: 0.4s;
}

.star-button--active > svg {
  opacity: 1;
  fill: var(--yellow);
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
