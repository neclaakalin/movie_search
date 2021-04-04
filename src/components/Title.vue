<template>
  <v-card-title>
    {{ title }}
  </v-card-title>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
  },
};
</script>
