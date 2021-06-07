<template>
  <div class="container">
    <ul class="list">
      <transition-group name="slide-top-fade">
        <li v-for="list in lists_prop" :key="list.id" class="list__item">
          <button
            class="btn-blank list__button"
            :class="{'list__button--starred': list.isStarred}"
            @click="$emit('openListEvent', list)"
          >
            {{ list.name }}

            <img
              class="icon"
              src="../assets/image/icon/arrow-right.svg"
              alt="Arrow right"
            />
          </button>
        </li>
      </transition-group>
    </ul>
    <div class="add-list">
      <button
        class="btn-blank add-list__button"
        @click="$emit('addNewListEvent')"
      >
        <svg
          class="icon add-list__svg"
          viewBox="0 0 426 426"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="m405.332031 192h-170.664062v-170.667969c0-11.773437-9.558594-21.332031-21.335938-21.332031-11.773437 0-21.332031 9.558594-21.332031 21.332031v170.667969h-170.667969c-11.773437 0-21.332031 9.558594-21.332031 21.332031 0 11.777344 9.558594 21.335938 21.332031 21.335938h170.667969v170.664062c0 11.777344 9.558594 21.335938 21.332031 21.335938 11.777344 0 21.335938-9.558594 21.335938-21.335938v-170.664062h170.664062c11.777344 0 21.335938-9.558594 21.335938-21.335938 0-11.773437-9.558594-21.332031-21.335938-21.332031zm0 0"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["lists_prop"],
};
</script>

<style scoped>
.list {
  border: 1px solid var(--grey);
  border-radius: 8px;
  overflow: hidden;
}

.list__item {
  border-bottom: 1px solid var(--grey);
}

.list__item:last-child {
  border: none;
}

.list__button {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 8px 10px;
  font-size: 25px;
}

.list__button--starred {
  background-color: #FFDC4E;
}

.add-list__button {
  position: absolute;
  right: 0;
  bottom: 0;
  margin: 20px;
  background-color: var(--blue);
  border-radius: 50%;
  padding: 10px;
  padding-bottom: 8px;
}

.add-list__svg {
  fill: var(--white);
}
</style>
