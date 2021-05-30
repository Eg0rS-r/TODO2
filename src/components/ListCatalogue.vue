<template>
  <div class="container">
    <ul class="list">
      <transition-group name="slide-top-fade">
        <li v-for="list in lists_prop" :key="list.id" class="list__item">
          <button class="btn-blank list__button" @click='$emit("openListEvent", list)'>
            {{ list.name }}
            <img class="icon" src="../assets/image/icon/arrow-right.svg" alt="Arrow right">
          </button>
        </li>
      </transition-group>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['lists_prop'],
};
</script>

<style scoped>
.list {
  border: 1px solid var(--grey);
  border-radius: 8px;
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
</style>
