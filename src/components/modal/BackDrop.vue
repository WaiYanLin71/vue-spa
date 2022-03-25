<template>
  <div class="back-drop row justify-content-center">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: ["toggleModal"]
};
</script>
<style scoped>
.back-drop {
  position: fixed;
  top: 0;
  width: 100%;
  height: 100vh;
  background-color: #000000b3;
}
</style>