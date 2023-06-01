<template>
  <div class="save-project"
       v-if="windowIsOpen"
       ref="draggable"
       :style="{'transform': `translate(${object.x}px, ${object.y}px)`}"
       @mousedown="startDrag"
       @mousemove="drag"
       @mouseup="stopDrag">
    <div class="save-project__wrapper">
      <div class="save-project__header">
        <h2 class="save-project__title">Сохранить в «Мои проекты»</h2>
        <AppSearch @searchText="searchedData"/>
      </div>
      <div class="save-project__content">
        <AppProjects :projects="filteredProjects"
                     @addNewRoom="(id) => roomId = id"
                     v-if="projects.length"
                     :currentProduct="currentProduct"
                     @addNewMaterial="addSavedMaterial"
                     @openCreateModal="openCreateRoomModal"/>
        <TheLoader v-else/>
      </div>
      <div class="save-project__footer">
        <TheButton @click="openCreateProjectModal">Создать проект</TheButton>
      </div>
    </div>
    <button class="save-project__close" @click="windowIsOpen = false">
      <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
        <g clip-path="url(#clip0_878_26964)">
          <path
              d="M25.3346 8.88L23.4546 7L16.0013 14.4533L8.54797 7L6.66797 8.88L14.1213 16.3333L6.66797 23.7867L8.54797 25.6667L16.0013 18.2133L23.4546 25.6667L25.3346 23.7867L17.8813 16.3333L25.3346 8.88Z"
              fill="#4A4A4A"/>
        </g>
        <defs>
          <clipPath id="clip0_878_26964">
            <rect width="32" height="32" fill="white"/>
          </clipPath>
        </defs>
      </svg>

    </button>
  </div>
  <AppModal v-model:modal-is-open="modalProjectIsOpen" @createNewProject="createNewProject" :modalId="1">
    <template v-slot:header>Создание проекта</template>
    <template v-slot:text>Создайте общий проект и наполните его объектами или комнатами</template>
  </AppModal>
  <AppModal v-model:modal-is-open="modalRoomIsOpen" @createNewRoom="addNewRoom" :modalId="2" :roomId="roomId">
    <template v-slot:header>Создание комнаты</template>
    <template v-slot:text>Добавляйте комнаты к готовому проекту</template>
  </AppModal>
</template>

<script>
import AppSearch from "@/components/AppSearch/AppSearch.vue";
import TheButton from "@/UI/TheButton/TheButton.vue";
import AppProjects from "@/components/AppProjects/AppProjects.vue";
import AppModal from "@/UI/AppModal/AppModal.vue";
import axios from "axios";
import TheLoader from "@/UI/TheLoader/TheLoader.vue";

export default {
  components: {TheLoader, AppModal, AppProjects, TheButton, AppSearch},
  data() {
    return {
      projects: [],
      searchedText: '',
      modalProjectIsOpen: false,
      modalRoomIsOpen: false,
      roomId: null,
      windowIsOpen: false,
      currentProduct: '',
      url: 'http://localhost:3001/projects/',
      object: {
        isDragging: false,
        x: 0,
        y: 0,
      }
    }
  },
  methods: {
    createNewProject(project) {
      this.projects.push(project)
      axios.post(this.url, project)
          .then(response => console.log(response))
          .catch(e => console.error(e))
    },
    addNewRoom(room) {
      this.projects.map(el => {
        if (el.id === room.roomId) {
          el.rooms.push(room)
          console.log(el.id)
          axios.put(this.url + el.id, el)
              .then(response => {
                console.log('Данные успешно обновлены:', response.data);
              })
              .catch(error => {
                console.error('Ошибка при обновлении данных:', error);
              });
        }
      })
    },
    openCreateProjectModal() {
      this.modalProjectIsOpen = !this.modalProjectIsOpen
    },
    openCreateRoomModal() {
      this.modalRoomIsOpen = !this.modalRoomIsOpen
    },
    searchedData(text) {
      this.searchedText = text;
    },
    addSavedMaterial(material, roomId, id) {
      console.log(material)
      this.projects.map(project => {
        project.rooms.map(room => {
          if (room.id === id) {
            console.log('Материал: ' + material + ' добавлен!')
            room.materials.push(material)
            axios.put(this.url + project.id, project)
                .then(response => {
                  console.log('Данные успешно обновлены:', response.data);
                })
                .catch(error => {
                  console.error('Ошибка при обновлении данных:', error);
                });
          }
        })
      })
    },
    startDrag(event) {
      this.object.isDragging = true;
      this.object.prevX = event.clientX;
      this.object.prevY = event.clientY;
    },
    drag(event) {
      if (this.object.isDragging) {
        const deltaX = event.clientX - this.object.prevX;
        const deltaY = event.clientY - this.object.prevY;

        this.object.x += deltaX;
        this.object.y += deltaY;

        this.object.prevX = event.clientX;
        this.object.prevY = event.clientY;
      }
    },
    stopDrag() {
      this.object.isDragging = false;
    },
    fetchingData() {
      axios.get('http://localhost:3001/projects')
          .then(response => {
            setTimeout(() => {
              this.projects.push(...response.data)
            }, 2000)
          })
          .catch(e => console.error(e))
    }
  },
  computed: {
    filteredProjects() {
      return [...this.projects].filter(el => el.title.toLowerCase().includes(this.searchedText.toLowerCase()))
    }
  },
  mounted() {
    console.log('Vue скрипт загружен !')
    const products = document.querySelectorAll('[data--catalog-product]');
    products.forEach(el => {
      const addToProjectBtn = el.querySelector('[data-button-open]');
      addToProjectBtn.addEventListener('click', () => {
        this.windowIsOpen = true;
        this.currentProduct = el.querySelector('.article-product__title').textContent.replace(/\s /g, '')
      })
    })
    this.fetchingData();
    window.addEventListener('keydown', evt => {
      evt.keyCode === 27 ? this.windowIsOpen = false : this.windowIsOpen = true;
    })
  }
}
</script>

<style lang="scss" scoped>
.save-project {
  position: fixed;
  display: flex;
  flex-direction: column;
  padding: 20px 0 30px;
  width: 330px;
  height: 518px;
  box-shadow: 0 0 7px -4px #2E2E2E;
  background: #FFFFFF;
  border-radius: 6px;
  z-index: 102;

  &__wrapper {
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  &__notification {
    padding: 0 30px;
    font-size: 18px;
    color: #000;
  }

  &__header {
    padding: 0 30px;
    margin-bottom: 20px;
  }

  &__title {
    padding-bottom: 10px;
    margin-bottom: 10px;
    font-weight: 600;
    font-size: 16px;
    line-height: 20px;
    color: #090909;
    border-bottom: 1px solid #EFF0F2;
  }

  &__content {
    display: flex;
    flex-direction: column;
    row-gap: 10px;
    height: 100%;
    overflow-y: auto;

    &::-webkit-scrollbar-thumb {
      background: #838383;
      border-radius: 50px;
    }

    &::-webkit-scrollbar {
      width: 5px;
      height: 8px;
      background-color: #EFF0F2;
      border-radius: 50px;
    }
  }

  &__close {
    position: absolute;
    top: 10px;
    right: 10px;
    display: flex;
    padding: 0;
    margin: 0;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }

  &__footer {
    padding-top: 10px;
    margin-top: auto;
    border-top: 1px solid #EFF0F2;
  }
}
</style>