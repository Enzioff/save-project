<template>
  <div class="search" :class="{'search__input--typing': text.length}">
    <svg class="search__icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 19 18" fill="none">
      <ellipse cx="7.606" cy="7" stroke="#838383" stroke-linecap="round" stroke-linejoin="round" rx="4.927" ry="5"/>
      <path stroke="#838383" stroke-linecap="round" stroke-linejoin="round" d="m16.475 16-4.927-5"/>
    </svg>
    <input class="search__input"
           v-model="text"
           @input="searchedText"
           type="text"
           placeholder="Искать проект">
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: ''
    }
  },
  methods: {
    searchedText() {
      this.$emit('searchText', this.text)
    }
  }
}
</script>

<style lang="scss" scoped>
.search {
  position: relative;
  display: flex;
  border: 1px solid #838383;
  border-radius: 6px;
  overflow: hidden;

  &__icon {
    position: absolute;
    top: 50%;
    left: 19px;
    pointer-events: none;
    width: 19px;
    height: 18px;
    transform: translateY(-50%);
    transition: all .2s linear;
  }

  &__input--typing {
    .search__input {
      padding-left: 20px;
    }

    .search__icon {
      width: 0;
      opacity: 0;
    }
  }

  &__input {
    display: block;
    padding: 9px 20px 9px 57px;
    width: 100%;
    height: 100%;
    border: none;
    transition: all .2s linear;
  }
}
</style>