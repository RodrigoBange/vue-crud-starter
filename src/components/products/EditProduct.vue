<template>
  <section>
    <div class="container">
      <form ref="form">
        <h2 class="mt-3 mt-lg-5">Edit a product</h2>
        <h5 class="mb-4"></h5>

        <div class="input-group mb-3">
          <span class="input-group-text">Name</span>
          <input type="text" class="form-control" v-model="product.name" />
        </div>

        <div class="input-group mb-3">
          <span class="input-group-text">Price</span>
          <input type="number" class="form-control" v-model="product.price" />
        </div>

        <div class="input-group mb-3">
          <span class="input-group-text">Description</span>
          <textarea
            class="form-control"
            v-model="product.description"
          ></textarea>
        </div>

        <div class="input-group mb-3">
          <span class="input-group-text">Image URL</span>
          <input type="text" class="form-control" v-model="product.image" />
        </div>

        <div class="input-group mb-3">
          <span class="input-group-text">Category</span>
          <select class="form-select">
            <option v-for="category in categories" :value="category.id">{{ category.name }}</option>
          </select>
        </div>

        <div class="input-group mt-4">
          <button
              type="button"
              class="btn btn-primary"
              @click="updateProduct">
            Save changes
          </button>
          <button
            type="button"
            class="btn btn-danger"
            @click="this.$router.push('/products')"
          >
            Cancel
          </button>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: "EditProduct",
  props: {
    id: Number,
  },
  data() {
    return {
      product: {
        id: 0,
        name: "",
        price: 0.0,
        description: "",
        image: "",
        category_id: 0,
      },
      categories: [],
    };
  },
  mounted() {
    axios.get('http://localhost/categories')
        .then(
            result => {
              this.categories = result.data
              console.log(result)
            }
        )
        .catch(
            error => console.log(error)
        )

    axios.get("http://localhost/products/" + this.id)
        .then(
            result => this.product = result.data
        )
        .catch(
            error => console.log(error)
        )
  },
  methods: {
    updateProduct() {
      axios.put("http://localhost/products/" + this.id, this.product)
          .then(
              response => {
                this.$router.push('/products')
              }
          )
          .catch(
              error => console.log(error)
          )
    }
  }
};
</script>

<style>
</style>
