<template>
  <div class="row" v-if="pageCount > 0">
    <div class="col-md-4 mb-2">Total: {{ totalItems }}</div>
    <div class="col-md-4 mb-2">
      <nav>
        <ul class="pagination justify-content-center">
          <!-- Previous page -->
          <li
            v-if="pageNumber != 1"
            @click="goToPage(pageNumber - 1)"
            class="page-item"
            title="Previous page"
            aria-label="Previous"
          >
            <a class="page-link" href="#">
              <span aria-hidden="true">&laquo;</span>
            </a>
          </li>
          <!-- Current page - 2 -->
          <li
            v-if="pageNumber - 2 > 0"
            class="page-item"
            @click="goToPage(pageNumber - 2)"
          >
            <a class="page-link" href="#">{{ pageNumber - 2 }}</a>
          </li>
          <!-- Current page - 1 -->
          <li
            v-if="pageNumber - 1 > 0"
            class="page-item"
            @click="goToPage(pageNumber - 1)"
          >
            <a class="page-link" href="#">{{ pageNumber - 1 }}</a>
          </li>
          <!-- Current page -->
          <li v-if="pageCount > 1" class="page-item active">
            <a class="page-link" href="#">{{ pageNumber }}</a>
          </li>
          <!-- Current page + 1 -->
          <li
            v-if="pageNumber + 1 <= pageCount"
            class="page-item"
            @click="goToPage(pageNumber + 1)"
          >
            <a class="page-link" href="#">{{ pageNumber + 1 }}</a>
          </li>
          <!-- Current page + 2 -->
          <li
            v-if="pageNumber + 2 <= pageCount"
            class="page-item"
            @click="goToPage(pageNumber + 2)"
          >
            <a class="page-link" href="#">{{ pageNumber + 2 }}</a>
          </li>
          <!-- Next page -->
          <li
            v-if="pageNumber < pageCount"
            @click="goToPage(pageNumber + 1)"
            class="page-item"
            title="Next page"
            aria-label="Next"
          >
            <a class="page-link" href="#"
              ><span aria-hidden="true">&raquo;</span></a
            >
          </li>
        </ul>
      </nav>
    </div>
    <div class="col-md-4 text-right">
      <span v-if="pageCount > 1">Page {{ pageNumber }} / {{ pageCount }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    totalItems: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      pageNumber: 1,
    };
  },
  watch: {
    /**
     * Whenever a new search takes place, reset pagination to first page
     */
    totalItems: function () {
      this.pageNumber = 1;
    },
  },
  computed: {
    /**
     * Computes the current page count, based on the total number of
     * results and the number of results per page.
     */
    pageCount: function () {
      let remainder = this.totalItems % this.perPage;
      if (remainder === 0) {
        return this.totalItems / this.perPage;
      } else {
        return Math.floor(this.totalItems / this.perPage) + 1;
      }
    },
    /**
     * Computes the current offset, based on the current page number
     * and the records per page.
     */
    offset: function () {
      if (this.pageNumber === 1) {
        return 0;
      } else {
        return this.pageNumber * this.perPage - this.perPage;
      }
    },
  },
  methods: {
    /**
     * Emits an event to change the page for the parent component.
     * The offset to use is passed as event.
     */
    goToPage: function (pageNum) {
      this.pageNumber = pageNum;
      this.$emit("page-changed", this.offset);
    },
  },
};
</script>
