<template>
  <!-- <div class="keranjang"> -->
  <div class="container">
    <!-- pake PROPS untuk lempar data antar komponen -->
    <Navbar :updateKeranjang="keranjangs" />
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
              Keranjang
            </li>
          </ol>
        </nav>
      </div>
    </div>

    <div class="row">
      <div class="col">
        <h2>Keranjang <strong>Saya</strong></h2>
      </div>
    </div>

    <div class="row mt-4">
      <div class="col">
        <table class="table">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Foto</th>
              <th scope="col">Makanan</th>
              <th scope="col">Keterangan</th>
              <th scope="col">Jumlah</th>
              <th scope="col">Harga</th>
              <th scope="col">Total Harga</th>
              <th scope="col">Hapus</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
              <th scope="row">{{ index + 1 }}</th>
              <td>
                <img
                  :src="'../assets/images/' + keranjang.product.gambar"
                  class="img-fluid shadow"
                  width="250"
                />
              </td>
              <td>
                <strong>
                  {{ keranjang.product.nama }}
                </strong>
              </td>
              <td>
                {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
              </td>
              <td>{{ keranjang.jumlah_pesan }}</td>
              <td align="right">Rp {{ keranjang.product.harga }}</td>
              <td align="right">
                Rp {{ keranjang.jumlah_pesan * keranjang.product.harga }}
              </td>
              <td align="center" class="text-danger">
                <b-icon-trash
                  @click="hapusKeranjang(keranjang.id)"
                ></b-icon-trash>
              </td>
            </tr>

            <tr>
              <td colspan="6" align="right"><strong>Total Harga :</strong></td>
              <td align="right">
                <strong>Rp {{ totalHarga }}</strong>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Form Checkout -->
    <div class="row justify-content-end">
      <div class="col-md-4">
        <form class="mt-4" v-on:submit.prevent>
          <div class="form-group">
            <label for="nama">Nama Pemesan</label>
            <input
              type="text"
              v-model="pesan.nama"
              class="form-control"
              name=""
              id=""
            />
          </div>
          <div class="form-group">
            <label for="nomor_meja">Nomor Meja</label>
            <input
              type="number"
              v-model="pesan.nomor_meja"
              class="form-control"
              name=""
              id=""
            />
          </div>
          <button
            class="btn btn-success float-right"
            type="submit"
            @click="checkout"
          >
            <b-icon-cart></b-icon-cart> Pesan
          </button>
        </form>
      </div>
    </div>
  </div>
  <!-- </div> -->
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        // untuk nambahin id pada link endpoint API
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          // notif hapus
          this.$toast.error("Makanan berhasil dihapus dari keranjang", {
            type: "warning",
            position: "top-right",
            duration: 2000,
            dismissible: true,
          });
          axios
            // untuk ambil data lagi sesudah berhasil hapus
            .get("http://localhost:3000/keranjangs/")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    checkout() {
      if (this.pesan.nama && this.pesan.nomor_meja) {
        // masukan data keranjangs ke dalam array pesanans utk checkout
        this.pesan.keranjangs = this.keranjangs;

        axios
          // post("masukan alamat API", apa yang mauu dikirim)
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            // untuk delete isi keranjang saat berhasil dilakukan checkout
            // map itu kek fungsi perulangan foreach
            this.keranjangs.map(function (item) {
              return (
                axios
                  // untuk nambahin id pada link endpoint API
                  .delete("http://localhost:3000/keranjangs/" + item.id)

                  .catch((error) => console.log(error))
              );
            });

            // redirect ke halaman pesanan-sukses
            this.$router.push({ path: "/pesanan-sukses" });

            // notif sukses
            this.$toast.success("Sukses dipesan", {
              type: "success",
              position: "top-right",
              duration: 2000,
              dismissible: true,
            });
          })
          .catch((error) => console.log(error));
      } else {
        // notif error karena ga diisi
        this.$toast.error("Nama pemesan dan nomor meja wajib diisi !", {
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
      .get("http://localhost:3000/keranjangs/")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.jumlah_pesan * data.product.harga;
      }, 0);
    },
  },
};
</script>

<style>
</style>