<template>
  <div class="hero">
    <!-- untuk tampilan desktop -->
    <div class="d-md-block d-none">
      <div class="row">
        <div class="col-md-5">
          <div class="d-flex h-100">
            <div class="justify-content-center align-self-center">
              <h2>
                <strong>Delicious Food Menu</strong><br />
                in Your Gadget
              </h2>
              <p>Ayo segera pilih dan pesan makanan favorit Anda !</p>
              <router-link class="btn btn-success btn-lg" to="/foods">
                <b-icon-arrow-right></b-icon-arrow-right> Pesan
              </router-link>
            </div>
          </div>
        </div>
        <div class="col-md-7">
          <img src="../assets/images/hero.png" width="100%" />
        </div>
      </div>
    </div>

    <!-- untuk tampilan mobile -->
    <div class="d-sm-block d-md-none">
      <div class="row">
        <div class="col-md-7 mb-3">
          <img src="../assets/images/hero.png" width="100%" />
        </div>
        <div class="col-md-5">
          <div class="d-flex h-100">
            <div class="justify-content-center align-self-center">
              <h2>
                <strong>Delicious Food Menu</strong><br />
                in Your Gadget
              </h2>
              <p>Ayo segera pilih dan pesan makanan favorit Anda !</p>
              <button class="btn btn-success btn-lg">
                <b-icon-arrow-right></b-icon-arrow-right> Pesan
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style>
</style>