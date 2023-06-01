<template>
  <div class="projects">
    <AppProject v-for="project in projects"
                :project="project"
                :key="project.id"
                :currentProduct="currentProduct"
                @addNewMaterial="(material, roomId, id) => this.$emit('addNewMaterial', material, roomId, id)"
                @addNewRoom="addNewRoom"
                @openCreateModal="openCreateModal"/>
  </div>
</template>

<script>
import AppProject from "@/components/AppProject/AppProject.vue";

export default {
  components: {AppProject},
  props: {
    projects: Object,
    currentProduct: String,
  },
  methods: {
    addNewRoom(id) {
      this.$emit('addNewRoom', id);
    },
    openCreateModal() {
      this.$emit('openCreateModal', true)
    },
    createNewRoom(room) {
      console.log(room)
    }
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