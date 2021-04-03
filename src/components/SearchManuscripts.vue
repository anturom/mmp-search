<template>
  <section>
    <form>
      <input
        v-model.lazy="searchKeyword"
        placeholder="Enter a search keyword"
        id="searchInput"
        type="text"
      />
      <button v-on:click="getData" id="searchButton" type="button">
        Search
      </button>
    </form>
    <p style="color: red">{{ error }}</p>
    <p style="color: blue">{{ status }}</p>
    <table>
      <tr v-for="res in results" :key="res.title">
        <td>
          <a target="_blank" :href="res.url">{{ res.title }}</a>
          <div>
            <span style="margin: 0 10px" v-for="k in res.keywords" :key="k">{{
              k
            }}</span>
          </div>
        </td>
      </tr>
    </table>
  </section>
</template>

<script>
export default {
  data() {
    return {
      isLoading: false,
      searchKeyword: "",
      results: [],
      perPage: 20,
      error: "",
      status: "",
    };
  },
  methods: {
    /**
     * Fetches data from the remote API.
     */
    getData() {
      this.isLoading = true;
      this.status = this.isLoading === true ? "Loading..." : "";
      this.error = null;
      let url = `https://mmp.acdh-dev.oeaw.ac.at/api/stelle/?zitat=${this.searchKeyword}&limit=${this.perPage}`;
      fetch(url)
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          console.log(data);
          this.isLoading = false;
          // Update the status message
          this.status = data.count
            ? `The search returned ${data.count} result(s).`
            : "The search returned no results.";
          this.processData(data);
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false;
          this.error = "Failed to fetch data. Please try again later.";
        });
    },
    /**
     * Converts the raw API data into a new array structure (for easier rendering).
     */
    processData(data) {
      let results = [];
      data.results.forEach((pt) => {
        results.push({
          title: pt.display_label,
          keywords: this.extractKeywords(pt.key_word),
          url: this.getItemUrl(pt.url),
        });
      });
      this.results = results;
      console.log(this.results);
    },
    /**
     * Extracts keyword info from the 'key_word' multi-dimensional array,
     * into a simple 1-dimensional array.
     */
    extractKeywords(arr) {
      let keywords = [];
      arr.forEach(function (item) {
        if (item.stichwort) {
          keywords.push(item.stichwort);
        }
      });
      return keywords;
    },
    /**
     * Returns the item's URL in the base application, derived from the 'url' property, e.g.
     * from https://mmp.acdh-dev.oeaw.ac.at/api/stelle/27/ to https://mmp.acdh-dev.oeaw.ac.at/archiv/stelle/detail/27",
     */
    getItemUrl(url) {
      let retVal = url.replace(
        "https://mmp.acdh-dev.oeaw.ac.at/api/stelle/",
        "https://mmp.acdh-dev.oeaw.ac.at/archiv/stelle/detail/"
      );
      // strip the trailing / character
      return retVal.slice(0, retVal.length - 1);
    },
  },
};
</script>

<style scoped>
</style>
