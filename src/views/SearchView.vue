<template>
  <div class="wrapper">
    <div class="search">
      <input
        type="text"
        name="search"
        v-model="searchValue"
        @input="handleInput"
      />
    </div>
    <ul>
      <li v-for="item in results" :key="item.data[0].nasa_id">
        <p>{{ item.data[0].title }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import debounce from "lodash.debounce";

const api = "https://images-api.nasa.gov/search?q=";

export default {
  name: "SearchView",
  data() {
    return {
      searchValue: "",
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios
        .get(`${api}${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          console.log(this.results);
        })
        .catch((err) => {
          console.log(err);
        });
    }, 500),
  },
  components: {},
};
</script>

<style lang="scss" scoped>
.wrapper {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0;
  padding: 30px;
}
</style>
