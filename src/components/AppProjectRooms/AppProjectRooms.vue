<template>
  <div class="project__rooms">
    <AppProjectRoom v-for="room in projectRooms"
                    :room="room"
                    :key="room.id"
                    @savedMaterial="(savedMaterial, roomId, id) => this.$emit('savedMaterial', savedMaterial, roomId, id)"
                    :currentProduct="currentProduct"/>
  </div>
</template>

<script>
import AppProjectRoom from "@/components/AppProjectRoom/AppProjectRoom.vue";

export default {
  components: {AppProjectRoom},
  props: {
    projectRooms: Object,
    currentProduct: String,
  }
}
</script>

<style lang="scss" scoped>
.project__rooms {
  display: flex;
  flex-direction: column;
  margin-bottom: 5px;
}
</style>