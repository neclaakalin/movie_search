<template>
  <div v-if="message">
    <v-alert
      color="red"
      dense
      dismissible
      elevation="4"
      type="error"
      max-width="500px"
      >{{ message }}</v-alert
    >
  </div>
</template>

<script>
export default {
  props: {
    message: {
      type: String,
    },
  },
};
</script>

<style scoped>
div {
  width: 100%;
  box-sizing: border-box;
  position: absolute;
  bottom: 0;
  left: 50%;
  -ms-transform: translateX(-50%);
  transform: translateX(-50%);
  align-items: center;
  align-self: center;
}
</style>
