<template>
  <div class="food-detail">
    <Navbar />

    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <!-- detail -->
      <div class="row mt-3">
        <div class="col-md-6">
          <img
            :src="'../assets/images/' + product.gambar"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <h4>
            Harga : <strong>Rp {{ product.harga }}</strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan</label>
              <input
                type="number"
                v-model="pesan.jumlah_pesan"
                class="form-control"
                name=""
                id=""
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                class="form-control"
                placeholder="Jangan terlalu pedas.."
                name=""
                id=""
                v-model="pesan.keterangan"
              />
            </div>
            <button class="btn btn-success" type="submit" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: {},
      // untuk menampung data pesanan
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesan.jumlah_pesan) {
        // untuk ambil data produk dari PRODUCT dan dimasukin ke array PESAN
        // jadi nanti isi array nya = jumlah pesan, keterangan, sama array product
        this.pesan.product = this.product;

        axios
          // post("masukan alamat API", apa yang mauu dikirim)
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            // redirect ke halaman keranjang
            this.$router.push({ path: "/keranjang" });

            // notif sukses
            this.$toast.success(
              "Makanan berhasil dimasukkan ke dalam keranjang",
              {
                type: "success",
                position: "top-right",
                duration: 2000,
                dismissible: true,
              }
            );
          })
          .catch((error) => console.log(error));
      } else {
        // notif gagal
        this.$toast.error("Silahkan masukan jumlah pesananan", {
          type: "warning",
          position: "top-right",
          duration: 2000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      // untuk nambahin id pada link endpoint API
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>