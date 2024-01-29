<template>
  <div class="container">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="skeleton-img"></div>
        <div class="skeleton-body">
          <div class="skeleton-head"></div>
          <div class="skeleton-footer"></div>
        </div>
      </div>
    </div>
    <div
      v-else
      class="container"
      :class="{
        'bg-cyan': menCategory,
        'bg-grey-light': unavailableCategory,
        'bg-pink': womenCategory,
      }"
    >
      <div v-if="!unavailableCategory" class="overlay">
        <img src="../assets/bg-pattern.svg" alt="pattern" />
      </div>
      <div class="card">
        <div v-if="unavailableCategory" class="unavailable-container">
          <div class="overlay">
            <img
              src="../assets/unavailable-product.svg"
              alt="unavailable product"
            />
          </div>
          <div class="card-body">
            <p>This product is unavailable to show</p>
            <div class="button-sec">
              <button @click="nextProduct" class="btn-next">
                Next product
              </button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
          <div class="card-img">
            <img :src="product?.image" alt="product-image" />
          </div>
          <div class="card-body">
            <div class="card-body-header">
              <h2
                class="title"
                :class="menCategory ? 'text-navy' : 'text-purple'"
              >
                {{ product?.title }}
              </h2>
              <div class="card-body-header-sub">
                <p>{{ product?.category }}</p>
                <div class="rating">
                  <span>{{ product?.rating.rate }}</span>
                  <div class="circle-rate">
                    <span
                      class="circle"
                      :class="
                        product?.rating.rate > 0.5 || product?.rating.rate < 1.5
                          ? menCategory
                            ? 'bg-navy'
                            : 'bg-purple'
                          : menCategory
                          ? 'bg-navy-white'
                          : 'bg-purple-white'
                      "
                    ></span>
                    <span
                      class="circle"
                      :class="
                        product?.rating.rate >= 1.5
                          ? menCategory
                            ? 'bg-navy'
                            : 'bg-purple'
                          : menCategory
                          ? 'bg-navy-white'
                          : 'bg-purple-white'
                      "
                    ></span>
                    <span
                      class="circle"
                      :class="
                        product?.rating.rate > 2.5
                          ? menCategory
                            ? 'bg-navy'
                            : 'bg-purple'
                          : menCategory
                          ? 'bg-navy-white'
                          : 'bg-purple-white'
                      "
                    ></span>
                    <span
                      class="circle"
                      :class="
                        product?.rating.rate > 3.5
                          ? menCategory
                            ? 'bg-navy'
                            : 'bg-purple'
                          : menCategory
                          ? 'bg-navy-white'
                          : 'bg-purple-white'
                      "
                    ></span>
                    <span
                      class="circle"
                      :class="
                        product?.rating.rate > 4.5
                          ? menCategory
                            ? 'bg-navy'
                            : 'bg-purple'
                          : menCategory
                          ? 'bg-navy-white'
                          : 'bg-purple-white'
                      "
                    ></span>
                  </div>
                </div>
              </div>
            </div>
            <div class="desc">
              <p>{{ product?.description }}</p>
            </div>
            <div class="card-footer">
              <span
                class="price"
                :class="menCategory ? 'text-navy' : 'text-purple'"
                >$ {{ product?.price }}</span
              >
              <div class="button-sec">
                <button
                  class="btn-buy"
                  :class="menCategory ? 'bg-navy' : 'bg-purple'"
                >
                  Buy Now
                </button>
                <button
                  class="btn-next"
                  :class="
                    menCategory
                      ? 'border-navy text-navy'
                      : 'border-purple text-purple'
                  "
                  @click="nextProduct"
                >
                  Next product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

export default defineComponent({
  data() {
    return {
      productId: 1,
      product: null as any,
      isLoading: true,
      menCategory: false,
      womenCategory: false,
      unavailableCategory: false,
      maxProductId: null as any,
    };
  },
  async created() {
    const res = await axios.get(`https://fakestoreapi.com/products`);
    this.maxProductId = res.data.length;
  },
  async mounted() {
    await this.fetchProduct();
  },
  methods: {
    async fetchProduct() {
      try {
        this.isLoading = true;
        const res = await axios.get(
          `https://fakestoreapi.com/products/${this.productId}`
        );
        if (res.data.category === "men's clothing") {
          this.menCategory = true;
          this.womenCategory = false;
          this.unavailableCategory = false;
        } else if (res.data.category === "women's clothing") {
          this.menCategory = false;
          this.womenCategory = true;
          this.unavailableCategory = false;
        } else {
          this.menCategory = false;
          this.womenCategory = false;
          this.unavailableCategory = true;
        }
        console.log({
          id: this.productId,
          menCategory: this.menCategory,
          womenCategory: this.womenCategory,
          unavailableCategory: this.unavailableCategory,
        });
        this.product = res.data;
      } catch (error) {
        console.error(error);
      } finally {
        this.isLoading = false;
      }
    },
    nextProduct() {
      this.productId++;
      if (this.productId > this.maxProductId) {
        this.productId = 1;
      }
      this.fetchProduct();
    },
  },
});
</script>
