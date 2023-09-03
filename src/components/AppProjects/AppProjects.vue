<template>
  <div class="projects" v-if="projects.length">
    <AppProject v-for="project in projects"
                :project="project"
                :key="project.id"
                :currentProduct="currentProduct"
                :productId="productId"
                @addNewMaterial="(material, roomId, id) => this.$emit('addNewMaterial', material, roomId, id)"
                @addNewRoom="addNewRoom"
                @openCreateModal="openCreateModal"/>
  </div>
  <p class="notification" v-else>Проекты не найдены!</p>
</template>

<script>
import AppProject from "@/components/AppProject/AppProject.vue";

export default {
  components: {AppProject},
  props: {
    projects: Object,
    currentProduct: String,
    productId: Number,
  },
  methods: {
    addNewRoom(id) {
      this.$emit('addNewRoom', id);
    },
    openCreateModal() {
      this.$emit('openCreateModal', true)
    },
  }
}
</script>

<style lang="scss" scoped>
.projects {
  display: flex;
  flex-direction: column;
  padding-bottom: 10px;
  row-gap: 40px;
}
</style>
