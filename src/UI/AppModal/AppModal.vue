<template>
  <div class="modal" v-if="modalIsOpen" @click="closeModal">
    <div class="modal__container" @click.stop>
      <div class="modal__header">
        <h2 class="modal__title">
          <slot name="header"></slot>
        </h2>
        <button class="modal__close" @click="closeModal">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" fill="none">
            <g>
              <path fill="#4A4A4A"
                    d="M25.335 8.88 23.455 7 16 14.453 8.548 7l-1.88 1.88 7.453 7.453-7.453 7.454 1.88 1.88 7.453-7.454 7.454 7.454 1.88-1.88-7.454-7.454 7.454-7.453Z"/>
            </g>
            <defs>
                <path fill="#fff" d="M0 0h32v32H0z"/>
            </defs>
          </svg>
        </button>
      </div>
      <div class="modal__content">
        <picture class="modal__picture">
          <img :src="setImage" :class="{'empty': setImage.includes('.svg')}" alt="">
        </picture>
        <div class="modal__info">
          <p class="modal__text">
            <slot name="text"></slot>
          </p>
          <label>
            <input class="modal__input" type="text" v-model="name" :placeholder="placeholder">
            <span class="dangerous hidden"
                  ref="dangerous">Ошибка! Название должно состоять минимум из 3-х символов!</span>
          </label>
          <div class="modal__file">
            <input type="file" @change="onFileChange">
            <svg class="modal__icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 23" fill="none">
              <path fill-rule="evenodd"
                    d="M11 9.77a2.647 2.647 0 0 0-2.636 2.659A2.647 2.647 0 0 0 11 15.087a2.647 2.647 0 0 0 2.636-2.658A2.647 2.647 0 0 0 11 9.77ZM7.375 12.43c0-2.011 1.62-3.648 3.625-3.648s3.625 1.636 3.625 3.648c0 2.01-1.62 3.647-3.625 3.647s-3.625-1.636-3.625-3.647Z"
                    clip-rule="evenodd"/>
              <path fill-rule="evenodd"
                    d="M7.007 7.617H3.393v10.145h15.214V7.617h-3.614l-.659-2.146H7.666l-.66 2.146ZM6.82 4.328h8.358l.659 2.146h3.912v12.43H2.25V6.475h3.912l.66-2.146Z"
                    clip-rule="evenodd"/>
              <path d="M4.64 8.427h2.286V9.57H4.641V8.427ZM16.464 8.427h1.143V9.57h-1.143V8.427Z"/>
            </svg>
            <p>{{ fileName !== null ? fileName : 'Загрузить обложку' }}</p>
          </div>
        </div>
      </div>
      <div class="modal__footer">
        <TheButton class="v-button--outline" @click="closeModal">Отменить</TheButton>
        <TheButton class="v-button--outline v-button--field" @click="createFunc">Создать</TheButton>
      </div>
    </div>
  </div>
</template>

<script>

import TheButton from "@/UI/TheButton/TheButton.vue";

import logo from '@/assets/icon-logo.svg'

export default {
  components: {TheButton},
  props: {
    modalIsOpen: Boolean,
    modalId: Number,
    roomId: Number,
  },
  data() {
    return {
      name: '',
      imageUrl: null,
      fileName: null,
    }
  },
  methods: {
    closeModal() {
      this.$emit('update:modal-is-open', false)
      this.name = '';
    },
    createNewProject() {
      const newProject = {
        id: Date.now(),
        title: this.name,
        picture: this.setImage,
        rooms: [],
      }
      if (this.name.length >= 3) {
        this.$emit('createNewProject', newProject)
        this.name = '';
        this.imageUrl = null;
        this.fileName = null;
        this.closeModal();
      }
    },
    createNewRoom() {
      const newRoom = {
        roomId: this.roomId,
        id: Date.now(),
        roomTitle: this.name,
        picture: this.setImage,
        materials: [],
      }
      if (this.name.length >= 3) {
        this.$emit('createNewRoom', newRoom)
        this.name = '';
        this.imageUrl = null;
        this.fileName = null;
        this.closeModal();
      }
    },
    createFunc() {
      if (this.modalId === 1) {
        this.createNewProject();
        this.nameValidation();
      } else {
        this.createNewRoom();
        this.nameValidation();
      }
    },
    onFileChange(evt) {
      const file = evt.target.files[0]
      this.fileName = evt.target.files[0].name
      this.imageUrl = URL.createObjectURL(file)
    },
    nameValidation() {
      if (this.name.length >= 3) {
        this.$refs.dangerous.classList.add('hidden');
      } else {
        this.$refs.dangerous.classList.remove('hidden');
      }
    }
  },
  computed: {
    placeholder() {
      if (this.modalId === 1) {
        return 'Название проекта, например, «Квартира №1»'
      } else {
        return 'Название комнаты, например, «Детская»'
      }
    },
    setImage() {
      if (this.imageUrl !== null) {
        return this.imageUrl
      } else {
        return logo
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 103;

  &__container {
    position: relative;
    display: flex;
    flex-direction: column;
    row-gap: 30px;
    padding: 60px;
    background-color: #fff;
    width: 100%;
    max-width: 870px;
    border-radius: 6px;
  }

  &__content {
    display: flex;
    align-items: flex-start;
    column-gap: 30px;
  }

  &__input {
    display: flex;
    padding: 13px 20px;
    font-weight: 500;
    font-size: 16px;
    line-height: 20px;
    color: #000;
    border: 1px solid #838383;
    border-radius: 6px;

    &::placeholder {
      color: #838383;
    }
  }

  &__close {
    position: absolute;
    top: 30px;
    right: 30px;
    background: transparent;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    height: 30px;
    padding: 0;
    margin: 0;
    border: none;
    cursor: pointer;

    svg {
      width: 30px;
      height: 30px;
    }
  }

  &__info {
    display: flex;
    flex-direction: column;
    width: 100%;

    label {
      display: flex;
      flex-direction: column;
      row-gap: 10px;
      width: 100%;
      margin-bottom: 30px;
    }

  }

  &__text {
    margin-bottom: 20px;
    max-width: 335px;
    font-weight: 500;
    font-size: 16px;
    line-height: 20px;
    color: #000000;
  }

  &__picture {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-shrink: 0;
    width: 270px;
    height: 270px;
    background-color: #DAE8F0;
    border-radius: 6px;
    overflow: hidden;

    .empty {
      width: 132px;
      height: auto;
    }

    img {
      display: block;
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
    }
  }

  &__title {
    font-weight: 700;
    font-size: 24px;
    line-height: 29px;
    color: #333333;
  }

  &__footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__file {
    position: relative;
    display: flex;
    align-items: center;
    width: max-content;
    column-gap: 10px;
    padding: 12px 24px;
    border: 1px solid #6DA1BD;
    border-radius: 6px;
    transition: all .2s linear;

    &:hover {
      background-color: #6DA1BD;

      .modal__icon {
        fill: #fff;
      }

      p {
        color: #fff;
      }
    }

    .modal__icon {
      pointer-events: none;
      width: 22px;
      height: 23px;
      fill: #6DA1BD;
      transition: all .2s linear;
    }

    input {
      position: absolute;
      top: 0;
      left: 0;
      opacity: 0;
      display: flex;
      width: 100%;
      height: 100%;
      cursor: pointer;
    }

    p {
      font-weight: 600;
      font-size: 16px;
      line-height: 20px;
      color: #6DA1BD;
      transition: all .2s linear;
    }
  }
}

@media screen and (max-width: 1200px) {
  .modal {
    top: inherit;
    left: 0;
    right: inherit;
    bottom: 90px;
    width: 100%;
    height: calc(100% - 151px);
    background-color: transparent;

    &__container {
      padding: 30px 20px 20px;
      height: 100%;
      max-width: 100%;
    }

    &__header {
      padding-bottom: 10px;
      border-bottom: 1px solid #EFF0F2;
    }

    &__content {
      display: flex;
      flex-direction: column;
      row-gap: 20px;

      label {
        margin-bottom: 20px;
      }
    }

    &__title {
      font-size: 16px;
      line-height: 20px;
    }

    &__text {
      margin-bottom: 10px;
      max-width: 100%;
    }

    &__close {
      top: 30px;
      right: 20px;
      width: 20px;
      height: 20px;
      svg {
        width: 20px;
        height: 20px;
      }
    }
  }
}
</style>