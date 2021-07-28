<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h1>Daftar <strong>Makanan</strong></h1>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col">
          <div class="input-group">
            <div class="input-group mb-2">
              <input
                v-model="search"
                type="text"
                class="form-control"
                id="inlineFormInputGroup"
                placeholder="Cari makanan kesukaan Anda..."
                @keyup="searchFood"
              />
              <div class="input-group-prepend">
                <div class="input-group-text">
                  <b-icon-search></b-icon-search>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row mb-3">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchFood() {
      axios
        // jadi ambil keyword dari form onkeyup trus masuk ke data search trus panggil fungsi untuk masukin ke akhir endpoint API
        .get("http://localhost:3000/products?q=" + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>
