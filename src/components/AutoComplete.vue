<template>
  <v-autocomplete
    v-model="model"
    :items="items"
    :loading="isLoading"
    :search-input.sync="searchText"
    :label="label"
    :placeholder="placeholder"
    auto-select-first
    clearable
    dense
    solo
    hide-no-data
    item-text="Title"
    solo-inverted
    return-object
  ></v-autocomplete>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
    },
    isLoading: {
      type: Boolean,
      required: true,
    },
    label: {
      type: String,
    },
    placeholder: {
      type: String,
      required: true,
    },
  },

  data: () => ({
    model: null,
    searchText: null,
  }),

  watch: {
    searchText() {
      this.$emit("searchChanged", this.searchText);
    },
    model() {
      this.$emit("itemSelected", this.model);
    },
  },
};
</script>
