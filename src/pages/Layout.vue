<template>
  <div class="mb-10">
    <Header />
    <div
      v-if="loading"
      class="flex justify-center items-center h-screen text-2xl"
    >
      Fetching Products...
    </div>
    <div
      v-else-if="!loading && products.length < 1"
      class="flex justify-center items-center h-screen text-2xl"
    >
      Ops! Can't reach server.
    </div>
    <div v-else class="mt-16 p-5 grid grid-cols-1 md:grid-cols-2 gap-4">
      <ProductCard
        v-for="(product, idx) in products"
        :key="idx"
        :product="product"
      />
    </div>
    <Paginator
      :currentPage="paginatorData.currentPage"
      :perPage="paginatorData.perPage"
      :totalPages="paginatorData.totalPages"
      :totalItems="paginatorData.totalItems"
      @selectPage="loadPage($event)"
    />
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Paginator from "../components/Paginator.vue";
import ProductCard from "../components/ProductCard.vue";

export default {
  name: "Layout",
  components: {
    Header,
    Paginator,
    ProductCard,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      baseUrl: "http://localhost:3000/review",
      loading: true,
      products: [],
      paginatorData: {
        totalItems: 0,
        totalPages: 1,
        perPage: 5,
        currentPage: 1,
      },
    };
  },
  created() {
    this.loadProducts(0, this.paginatorData.perPage);
  },
  methods: {
    loadPage(page) {
      const start = this.paginatorData.perPage * (page - 1);
      this.loadProducts(start, this.paginatorData.perPage);
    },

    loadProducts(start, limit) {
      this.loading = true;
      this.products = [];
      this.axios
        .get(this.baseUrl + `?_start=${start}&_limit=${limit}`)
        .then((response) => {
          this.products = response.data;
          this.paginatorData.totalItems = parseInt(
            response.headers["x-total-count"]
          );
          this.paginatorData.totalPages = Math.floor(
            this.paginatorData.totalItems / this.paginatorData.perPage
          );
        })
        .finally(() => {
          this.loading = false;
          window.scrollTo(0, 0);
        });
    },
  },
};
</script>

<style scoped></style>
