<template>
  <header class="header">
    <h1 class="header__title">
      <input
        class="input-blank header__input"
        type="text"
        v-model="newListName"
        maxlength="12"
        @keyup.enter="headerInputBlur"
        @keyup="$emit('changeListNameEvent', newListName)"
      />
    </h1>
    <button
      class="btn-blank"
      @click="$emit('listStarred')"
    >
      <svg
        class="icon icon--starred"
        :class="{ 'icon--starred-active': listStarred_prop }"

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
</template>

<script>
export default {
  props: ["listName_prop", "listStarred_prop"],
  data() {
    return {
      newListName: this.listName_prop,
    };
  },
  methods: {
    headerInputBlur() {
      document.querySelector(".header__input").blur();
    },
  }
};
</script>

<style scoped>
.header {
  padding: 20px;
  padding-bottom: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header__title {
  padding-left: 10px;
}

.header__input {
  max-width: 240px;
  font-size: 35px;
  font-weight: 700;
}
</style>
