<template>
  <button class="v-button">
    <span class="v-button__icon">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 18 18" fill="none">
        <path fill="#5A7384" fill-rule="evenodd" d="M16 10H2V8h14v2Z" clip-rule="evenodd"/>
        <path fill="#5A7384" fill-rule="evenodd" d="M8 16V2h2v14H8Z" clip-rule="evenodd"/>
      </svg>
    </span>
    <slot/>
  </button>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.v-button {
  display: flex;
  align-items: center;
  padding: 0;
  margin: 0 30px;
  cursor: pointer;
  column-gap: 10px;
  background-color: transparent;
  border: none;
  font-weight: 600;
  font-size: 16px;
  line-height: 20px;
  color: #6DA1BD;
  transition: all .3s linear;

  &--save {
    display: flex;
    justify-content: center;
    width: 0;
    margin: 0;
    padding: 9.5px 15px;
    background-color: #EB5A1E;
    border-radius: 6px;
    font-weight: 600;
    font-size: 14px;
    line-height: 17px;
    color: #FFFFFF;
    transition: background-color .2s linear;
    text-align: center;
    opacity: 0;
    visibility: hidden;
    &:hover {
      background-color: darken(#EB5A1E, 10%);
    }
    .v-button__icon {
      display: none;
    }
  }

  &--outline {
    margin: 0;
    padding: 13px 24px;
    font-weight: 600;
    font-size: 16px;
    line-height: 20px;
    color: #6DA1BD;
    border: 1px solid #6DA1BD;
    border-radius: 6px;
    background-color: #fff;

    &:hover {
      background-color: #6DA1BD;
      color: #fff;
    }

    .v-button__icon {
      display: none;
    }
  }

  &--field {
    color: #fff;
    background-color: #6DA1BD;
    &:hover {
      background-color: darken(#6DA1BD, 10%);
    }
  }

  &__icon {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 14px;
    border: 1px solid #6DA1BD;
    border-radius: 6px;

    svg {
      width: 18px;
      height: 18px;
    }
  }
}
</style>