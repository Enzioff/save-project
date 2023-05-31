<template>
  <div class="room">
    <picture class="room__picture" @click="showInfo">
      <img :src="room.picture" :alt="room.roomTitle">
    </picture>
    <div class="room__info">
      <h3 class="room__title">{{ room.roomTitle }}</h3>
      <TheButton class="v-button--save" @click="saveToRoom">{{ isSaved ? 'Добавлено' : 'Сохранить' }}</TheButton>
    </div>
  </div>
</template>

<script>


import TheButton from "@/UI/TheButton/TheButton.vue";

export default {
  components: {TheButton},
  data() {
    return {
      isSaved: false,
    }
  },
  props: {
    room: Object,
    currentProduct: String,
  },
  methods: {
    showInfo() {
      console.log(this.room)
    },
    saveToRoom() {
      this.isSaved = true;
      this.$emit('savedMaterial', this.currentProduct, this.room.roomId, this.room.id)
      setTimeout(() => {
        this.isSaved = false;
      }, 1000)
    }
  }
}
</script>

<style lang="scss" scoped>
.room {
  display: flex;
  align-items: center;
  padding: 5px 30px;
  column-gap: 11px;
  transition: background-color .3s linear;

  &:hover {
    background-color: #EFF0F2;

    .room__title {
      max-width: 97px;
      text-overflow: ellipsis;
      white-space: nowrap;
      overflow: hidden;
    }

    .v-button {
      opacity: 1;
      visibility: visible;
      width: 110px;
    }
  }

  &__picture {
    display: flex;
    width: 50px;
    height: 50px;
    flex-shrink: 0;
    background-color: #D9D9D9;
    border-radius: 4px;
    overflow: hidden;

    img {
      display: block;
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
    }
  }

  &__title {
    font-weight: 600;
    font-size: 14px;
    line-height: 17px;
    color: #090909;
  }

  &__info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    column-gap: 7px;
    width: 100%;
    overflow: hidden;
  }
}
</style>