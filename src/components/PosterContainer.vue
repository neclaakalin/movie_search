<template>
  <v-img
    :src="`${posterUrl}`"
    :lazy-src="`${posterUrl}`"
    :eager="true"
    class="white--text align-end"
    gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
    height="auto"
    width="auto"
    max-width="500px"
    max-height="600px"
    contain
    @error="imageLoadError"
  >
    <template v-slot:placeholder>
      <PlaceholderSpin />
    </template>
    <v-card-title v-text="title"></v-card-title>
    <v-card-subtitle v-text="year"> </v-card-subtitle>
  </v-img>
</template>

<script>
import PlaceholderSpin from "./PlaceholderSpin";
import en from "../locale/en.json";

export default {
  components: {
    PlaceholderSpin,
  },

  props: {
    title: {
      type: String,
    },
    posterUrl: {
      type: String,
    },
    year: {
      type: String,
    },
  },

  methods: {
    imageLoadError() {
      this.$emit("imageLoadError", en.imageLoadError);
    },
  },
};
</script>
