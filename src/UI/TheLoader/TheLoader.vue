<template>
  <div class="loader"></div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.loader {
  display: block;
  margin: 50px auto 0;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  border: 5px solid #2E2E2E;
  border-top-color: transparent;
  animation: rotate 0.8s linear infinite;
}

@keyframes rotate {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>