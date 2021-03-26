<template>
  <div class="flex justify-center my-4">
    <div class="pt-4 flex">
      <div
        class="flex justify-center items-center cursor-pointer bg-white text-center text-sm h-8 w-8 mr-4 shadow-lg rounded-full"
        :class="[isFirstPage ? 'opacity-50 cursor-not-allowed' : '']"
        @click="prevPage()"
      >
        <svg
          class="h-5 w-5"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          aria-hidden="true"
        >
          <path
            fill-rule="evenodd"
            d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
            clip-rule="evenodd"
          />
        </svg>
      </div>
      <div
        v-if="firstIndex > 1"
        class="flex justify-center items-center cursor-pointer text-black text-center text-sm h-8 w-8 mr-2 rounded-full"
        @click="morePage('prev')"
      >
        ...
      </div>
      <div
        v-for="page in range"
        :key="page"
        class="flex justify-center items-center cursor-pointer text-center text-sm h-8 w-8 mr-2 rounded-full"
        :class="[activePage === page ? 'text-white bg-black' : 'text-black']"
        @click="selectPage(page)"
      >
        {{ page }}
      </div>
      <div
        v-if="lastIndex < totalPages"
        class="flex justify-center items-center cursor-pointer text-black text-center text-sm h-8 w-8 mr-2 rounded-full"
        @click="morePage('next')"
      >
        ...
      </div>
      <div
        class="flex justify-center items-center cursor-pointer bg-white text-center text-sm h-8 w-8 mr-4 shadow-lg rounded-full"
        :class="[isLastPage ? 'opacity-50 cursor-not-allowed' : '']"
        @click="nextPage()"
      >
        <svg
          class="h-5 w-5"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
          fill="currentColor"
          aria-hidden="true"
        >
          <path
            fill-rule="evenodd"
            d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
            clip-rule="evenodd"
          />
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Paginator",
  props: {
    currentPage: {
      type: Number,
      default: 1,
    },
    perPage: {
      type: Number,
      default: 5,
    },
    totalPages: {
      type: Number,
      default: 1,
    },
    totalItems: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      activePage: 1,
      firstIndex: 1,
      lastIndex: 3,
    };
  },
  watch: {
    totalPages() {
      this.lastIndex = this.totalPages > 3 ? 3 : this.totalPages;
    },
  },
  computed: {
    isFirstPage() {
      return this.activePage - 1 <= 0;
    },

    isLastPage() {
      return this.activePage + 1 > this.totalPages;
    },

    range() {
      return [...Array(this.lastIndex - this.firstIndex + 1).keys()].map(
        (i) => i + this.firstIndex
      );
    },
  },
  created() {
    this.activePage = this.currentPage;
    this.lastIndex = this.totalPages > 3 ? 3 : this.totalPages;
  },
  methods: {
    selectPage(page) {
      if (page === this.activePage) return;
      this.activePage = page;
      this.$emit("selectPage", this.activePage);
    },

    prevPage() {
      if (this.isFirstPage) return;
      if (this.activePage === this.firstIndex) {
        this.morePage("prev");
        return;
      }
      this.selectPage(this.activePage - 1);
    },

    nextPage() {
      if (this.isLastPage) return;
      if (this.activePage === this.lastIndex) {
        this.morePage("next");
        return;
      }
      this.selectPage(this.activePage + 1);
    },
    //load more page number
    morePage(direction) {
      const nextIndex = this.lastIndex + 1;
      switch (direction) {
        case "next":
          this.lastIndex =
            this.lastIndex + 3 > this.totalPages
              ? this.totalPages
              : this.lastIndex + 3;
          this.firstIndex = this.lastIndex - 2;
          this.selectPage(nextIndex);
          break;
        case "prev":
          this.firstIndex = this.firstIndex - 3 < 1 ? 1 : this.firstIndex - 3;
          this.lastIndex =
            this.firstIndex + 2 > this.totalPages
              ? this.totalPages
              : this.firstIndex + 2;
          this.selectPage(this.lastIndex);
          break;
        default:
          break;
      }
    },
  },
};
</script>

<style></style>
