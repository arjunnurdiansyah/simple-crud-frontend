<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>EDIT PRODUCT</h5>
          <hr />
          <b-form @submit="update">
            <b-form-group label="Product Name">
              <b-form-input
                type="text"
                v-model="post.name"
                :class="{ 'is-invalid': validation.name }"
                placeholder="Product Name"
              >
              </b-form-input>
              <div v-if="validation.name" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.name[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Product Price">
              <b-form-textarea
                id="textarea"
                v-model="post.price"
                :class="{ 'is-invalid': validation.price }"
                placeholder="Product Price"
                rows="5"
              >
              </b-form-textarea>
              <div v-if="validation.price" class="mt-2">
                <b-alert show variant="danger">{{
                  validation.price[0]
                }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">UPDATE</b-button>
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
      post: {
        name: "",
        price: "",
      },
      //state validation
      validation: [],
    };
  },

  mounted() {
    //get data post by ID
    this.$axios.get(`/product/${this.$route.params.id}`).then((response) => {
      (this.post.name = response.data.name),
        (this.post.price = response.data.price);
    });
  },

  methods: {
    async update(e) {
      e.preventDefault();

      //send data ke Rest API untuk update
      await this.$axios
        .patch(`/product/${this.$route.params.id}`, {
          //data yang dikirim
          name: this.post.name,
          price: Number(this.post.price),
        })
        .then(() => {
          //redirect ke route "post"
          this.$router.push({
            name: "post",
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

<style></style>
