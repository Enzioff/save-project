<template>
  <div class="project">
    <h3 class="project__title">{{ project.title }}</h3>
    <AppProjectRooms :projectRooms="project.rooms"
                     :productId="productId"
                     @addNewMaterial="(material, roomId, id) => this.$emit('addNewMaterial', material, roomId, id)"
                     :currentProduct="currentProduct"/>
    <TheButton class="" @click="openCreateModal">Добавить комнату</TheButton>
  </div>
</template>

<script>
import TheButton from "@/UI/TheButton/TheButton.vue";
import AppProjectRooms from "@/components/AppProjectRooms/AppProjectRooms.vue";

export default {
  components: {AppProjectRooms, TheButton},
  props: {
    project: Object,
    currentProduct: String,
    productId: Number,
  },
  methods: {
    openCreateModal() {
      this.$emit('openCreateModal', true);
      this.addNewRoom();
    },
    addNewRoom() {
      this.$emit('addNewRoom', this.project.id)
    }
  }
}
</script>

<style lang="scss" scoped>
.project__title {
  margin-bottom: 10px;
  padding: 0 30px;
}
</style>
