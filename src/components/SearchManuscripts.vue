<template>
  <div class="wrapper" id="single-wrapper">
    <div class="container">
      <div class="row">
        <div class="col-md-12 content-area">
          <main class="site-main">
            <article>
              <header class="entry-header">
                <div class="entry-top-thumbnail">
                  <img src="middle-ages.jpg" alt="" class="src" />
                </div>
                <h1 class="entry-title">Search manuscripts</h1>
              </header>
              <div class="entry-content">
                <form class="input-group mx-auto mt-3 mb-3">
                  <input
                    id="search-input"
                    type="text"
                    class="form-control"
                    placeholder="Enter a search keyword"
                    aria-label="Recipient's username"
                    aria-describedby="search-button"
                    v-model.lazy="searchKeyword"
                  />
                  <div class="input-group-append">
                    <button
                      id="search-button"
                      type="button"
                      class="btn btn-primary"
                      v-on:click="getData"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="16"
                        height="16"
                        fill="currentColor"
                        class="bi bi-search"
                        viewBox="0 0 16 16"
                      >
                        <path
                          d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"
                        />
                      </svg>
                    </button>
                  </div>
                </form>

                <p v-if="error" class="alert alert-danger" style="color: red">
                  {{ error }}
                </p>
                <p v-if="status" class="alert alert-info" role="alert">
                  {{ status }}
                </p>
                <table v-if="results.length > 0" class="table table-striped">
                  <thead>
                    <tr>
                      <th scope="col">Title</th>
                      <!-- <th scope="col">#</th>
                      <th scope="col">#</th> -->
                    </tr>
                  </thead>
                  <tr v-for="res in results" :key="res.title">
                    <td>
                      <a target="_blank" :href="res.url">{{ res.title }}</a>
                      <div>
                        <span
                          class="badge badge-pill badge-info m-1"
                          v-for="k in res.keywords"
                          :key="k"
                          >{{ k }}</span
                        >
                      </div>
                    </td>
                    <!-- <td></td>
                    <td></td> -->
                  </tr>
                </table>
              </div>
            </article>
          </main>
        </div>
      </div>
    </div>
  </div>
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
