<template>
  <div class="sm:flex shadow-md p-2 md:p-5">
    <img
      class="md:w-40 w-full h-60 md:h-40 object-cover rounded-md mr-4"
      :src="product.image"
      alt=""
    />
    <div class="flex flex-col justify-between">
      <div>
        <h2 class="font-bold text-lg">{{ product.name }}</h2>
        <p class="text-gray-500 text-sm">
          <span><strong>Seller:</strong> {{ product.seller_name }}</span>
          <span class="mx-2">•</span>
          <span><strong>Qty:</strong> {{ product.available_count }}</span>
        </p>
        <p class="font-bold text-lg">{{ product.price }}</p>
        <p class="description">{{ product.description }}</p>
      </div>
      <div>
        <div class="flex items-center text-gray-500 mt-2">
          <div class="relative h-4 w-24">
            <div class="absolute flex" style="width: 90px">
              <i
                v-for="i in 5"
                :key="i"
                class="far fa-star text-yellow-400"
              ></i>
            </div>
            <div
              class="absolute flex overflow-hidden"
              :style="{ width: rating }"
            >
              <i
                v-for="i in 5"
                :key="i"
                class="fas fa-star text-yellow-400"
              ></i>
            </div>
          </div>
          <span>({{ product.star_rating }})</span>
          <span class="mx-3">•</span>
          <span>{{ product.review_count }} Reviews</span>
        </div>
        <p class="text-sm text-gray-500">
          <span>{{ product.location }}</span>
          <span class="mx-2">|</span>
          <span><strong>Updated:</strong> {{ product.last_updated }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    product: {
      type: Object,
      default: () => ({
        id: "",
        name: "",
        description: "",
        last_updated: "",
        available_count: 0,
        price: "",
        location: "",
        image: "",
        seller_name: "",
        star_rating: 0,
        review_count: "",
      }),
    },
  },
  computed: {
    rating() {
      const percentage = (this.product.star_rating / 5) * 100 || 10;
      return `${percentage - 6}px`;
    },
  },
};
</script>

<style scoped>
p.description {
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 4;
  display: -webkit-box;
  -webkit-box-orient: vertical;
}
</style>
