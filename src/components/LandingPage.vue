<template>
  <v-container fluid class="pa-10">
    <v-row class="mb-6 ma-0" justify="center">
      <Title :title="locale.appTitle" />
    </v-row>
    <v-row justify="center" class="mb-6 ma-0">
      <v-responsive>
        <AutoComplete
          :items="items"
          :isLoading="isLoading"
          :label="locale.autoCompleteLabel"
          :placeholder="locale.autoCompletePlaceholder"
          @searchChanged="searchChanged"
          @itemSelected="itemSelected"
        />
      </v-responsive>
    </v-row>
    <v-row justify="center" v-if="model && !isImgLoadError">
      <PosterContainer
        :title="model.Title"
        :posterUrl="model.Poster"
        :year="model.Year"
        @imageLoadError="imageLoadError"
      />
    </v-row>
    <v-row justify="center" v-else>
      <ErrorMessage :message="errorMessage" />
    </v-row>
  </v-container>
</template>

<script>
import AutoComplete from "./AutoComplete";
import Title from "./Title";
import PosterContainer from "./PosterContainer";
import ErrorMessage from "./ErrorMessage";
import en from "../locale/en.json";

export default {
  components: {
    AutoComplete,
    Title,
    PosterContainer,
    ErrorMessage,
  },

  data: () => ({
    model: null,
    descriptionLimit: 60,
    isLoading: false,
    apiResponse: "",
    errorMessage: "",
    isImgLoadError: false,
    searchItems: [],
    locale: en,
  }),

  computed: {
    items() {
      return this.searchItems?.map((entry) => {
        const Title =
          entry.Title.length > this.descriptionLimit
            ? entry.Title.slice(0, this.descriptionLimit) + "..."
            : entry.Title;
        return Object.assign({}, entry, { Title });
      });
    },
    checkIfModelUpdated(model) {
      return model === this.model;
    },
  },

  methods: {
    searchChanged(searchText) {
      if (searchText) {
        this.isLoading = true;
        fetch(
          `http://www.omdbapi.com/?apikey=2d26cc0c&type=movie&y=2020&s=${searchText}`
        )
          .then((res) => res.json())
          .then((res) => {
            const { Response, Error, Search } = res;
            this.apiResponse = Response;
            this.errorMessage =
              this.model && this.isImgLoadError ? this.errorMessage : Error;
            this.searchItems = Search;
            console.log("-*-*-*");
          })
          .catch((err) => {
            this.errorMessage = err;
          })
          .finally(() => (this.isLoading = false));
      }
    },
    itemSelected(newModel) {
      this.model = newModel;
    },
    imageLoadError(err) {
      console.log(err);
      this.errorMessage = err;
      this.isImgLoadError = true;
    },
  },

  watch: {
    model() {
      console.log("---");
      this.isImgLoadError = false;
    },
  },
};
</script>

<style scoped>
* {
  caret-color: transparent;
}
</style>
