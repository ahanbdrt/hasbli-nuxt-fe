<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <Navigation />
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH TRANSAKSI</h5>
          <hr />
          <b-form @submit="store">
            <b-form-group label="Nama Produk">
              <b-form-input
                type="text"
                v-model="product.nama_produk"
                :class="{ 'is-invalid': validation.nama_produk }"
                placeholder="Masukkan Nama Produk"
              >
              </b-form-input>
              <div v-if="validation.nama_produk" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.nama_produk[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Harga">
              <b-form-input
                type="text"
                v-model="product.harga"
                :class="{ 'is-invalid': validation.harga }"
                placeholder="Masukkan Harga Produk"
              >
              </b-form-input>
              <div v-if="validation.harga" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.harga[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="SKU">
              <b-form-input
                type="text"
                v-model="product.sku"
                :class="{ 'is-invalid': validation.sku }"
                placeholder="Masukkan SKU"
              >
              </b-form-input>
              <div v-if="validation.sku" class="mt-2">
                <b-alert show variant="danger">{{ validation.sku[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Deskripsi">
              <b-form-textarea
                id="textarea"
                v-model="product.deskripsi"
                :class="{ 'is-invalid': validation.deskripsi }"
                placeholder="Masukkan Deskripsi Barang"
                rows="5"
              >
              </b-form-textarea>
              <div v-if="validation.deskripsi" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.deskripsi[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">SIMPAN</b-button>
            <a href="/product/" class="btn btn-primary">Back</a>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      product: {
        nama_produk: "",
        harga: "",
        sku: "",
        deskripsi: "",
      },
      //state validation
      validation: [],
    };
  },

  methods: {
    //method "store"
    async store(e) {
      e.preventDefault();

      //send data ke Rest API
      await this.$axios
        .post("/api/product", {
          //data yang dikirim ke server
          nama_produk: this.product.nama_produk,
          harga: this.product.harga,
          sku: this.product.sku,
          deskripsi: this.product.deskripsi,
        })
        .then(() => {
          this.$router.push({
            name: "product",
          });
        })
        .catch((error) => {
          //assign validation
          this.validation = error.response.data;
        });
    },
  },
};
</script>

<style>
</style>