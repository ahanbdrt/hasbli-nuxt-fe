<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <Navigation />
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT PRODUCT</h5>
          <hr />
          <b-form @submit="update">
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
                placeholder="Masukkan SKU Produk"
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
                placeholder="Masukkan Deskripsi"
                rows="5"
              >
              </b-form-textarea>
              <div v-if="validation.deskripsi" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.deskripsi[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">UPDATE</b-button>
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
      //state post
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

  mounted() {
    //get data post by ID
    this.$axios
      .get(`/api/product/${this.$route.params.id}`)
      .then((response) => {
        (this.product.nama_produk = response.data.data.nama_produk),
          (this.product.harga = response.data.data.harga),
          (this.product.sku = response.data.data.sku),
          (this.product.deskripsi = response.data.data.deskripsi);
      });
  },

  methods: {
    async update(e) {
      e.preventDefault();

      //send data ke Rest API untuk update
      await this.$axios
        .put(`/api/product/${this.$route.params.id}`, {
          //data yang dikirim
          nama_produk: this.product.nama_produk,
          harga: this.product.harga,
          sku: this.product.sku,
          deskripsi: this.product.deskripsi,
        })
        .then(() => {
          //redirect ke route "post"
          this.$router.push({
            name: "product",
          });
        })
        .catch((error) => {
          //assign error validasi
          this.validation = error.response.data;
        });
    },
  },
};
</script>

<style>
</style>