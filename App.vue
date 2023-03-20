<template>
  <div class="row eq-row-height">

    <div class="col">
      <!-- <button v-if="categories.length" class="btn btn-outline-primary" @click="fetchData()">
        All
      </button> -->
      <category class="category" :class="{ 'active': currentCat !== null }">
        <template v-slot:text @click="fetchData()"> <!-- v-if="categories.length" -->
          All
        </template>
      </category>
    </div>

    <div class="col" v-for="category in categories" :key="categories.cat" :class="{ 'avtive': currentCat && currentCat !== category }">
      <!-- {{ category }} -->
      <!-- <button class="btn btn-outline-primary" @click="fetchCategory(category)">
        {{ category }}
      </button> -->
      <category class="category">
        <template v-slot:text @click="fetchCategories()">
          {{ category.cat }}
        </template>
      </category>
    </div>
    <div class="w-100 mt-2"></div>
    <div class="col-12 col-md-3 mb-3" v-for="product in products">
      <card class="card">

        <template v-slot:header>
          <h4>{{ product.title }}</h4>
        </template>

        <template v-slot:content>
          <div :style="{ backgroundImage: 'url(' + product.image + ')' }" class="img-product">
          </div>
        </template>


        <template v-slot:fotter>
          <div> {{ product.description }} </div>
          <div class="category badge badge-info text-white p-1 pt-1.9 pb-2" @click="fetchCategory(product.category)"> {{
            product.category }}</div>
          <div class="text-success"> {{ product.price }}</div>
        </template>
      </card>
    </div>
  </div>
  <!-- <button @click="fetchData">load</button> -->
</template>

<script>
import Card from "./components/Card.vue";
import Category from "./components/Category.vue";
// const API = "https://fakestoreapi.com/";

export default {
  name: "App",
  components: {
    Card,
    Category
  },
  data() {
    return {
      products: [],
      categories: [
        { cat:  "electronics"},
        { cat:  "jewelery"},
        { cat:  "men's clothing"},
        { cat:  "women's clothing"},
      ],
      selectedCat: null,
    };
  },
  created() {
    this.fetchData()
    // this.fetchCategories()
  },
  methods: {
    async fetchData(url) {
      this.currentCat = null;
      let response = await fetch("https://fakestoreapi.com/products"); // or can write > fetch(input: '${API}/products')
      // .then(res=>res.json())
      // .then(json=>console.log(json))
      let data = await response.json();
      // console.log(data)
      this.products = data.map(product => (
        {
          // id: product.id,
          title: product.title.slice(0, 25),
          description: product.description.slice(0, 100),
          price: product.price + "$",
          category: product.category,
          image: product.image,
        }
      ));
    },

    async fetchCategory(category) {
      this.currentCat = category;
      switch (category) {
        case "electronics":
          let response = await fetch("https://fakestoreapi.com/products/category/electronics")
          let data = await response.json();
          this.products = data.map(product => (
            {
              title: product.title.slice(0, 25),
              description: product.description.slice(0, 100),
              price: product.price + "$",
              category: product.category,
              image: product.image,
            }
          ));
          console.log(category)
          break;

        case "jewelery":
          // let response = await fetch("https://fakestoreapi.com/products/category/jewelery")
          // let data = await response.json();
          await fetch("https://fakestoreapi.com/products/category/jewelery")
            // .then((response) => response.json())
            // .then((data) => console.log(data));
            .then(res => res.json())
            .then(json => console.log(json))
          // this.products = data.map(product => (
          //   {
          //     title: product.title.slice(0, 25),
          //     description: product.description.slice(0, 100),
          //     price: product.price + "$",
          //     category: product.category,
          //     image: product.image,
          //   }
          // ));
          console.log(category)

          break;

        case "women's clothing":
          console.log(category)

          break;

        case "men's clothing":
          console.log(category)

          break;

        default:
          break;
      }
      // let response = await fetch("https://fakestoreapi.com/products/category/men's clothing")
      // let data = await response.json();
      // this.products = data.map(product => (
      //   {
      //     title: product.title,
      //     description: product.description,
      //     price: product.price + "$",
      //     category: product.category,
      //     image: product.image,
      //   }
      // ));
    },

    // async fetchCategories() {
    //   this.categories = await fetch("https://fakestoreapi.com/products/categories")
    //     .then(res => res.json())
    //     .then(json => console.log(json))
    //   // let dataCat = await response.json();
    //   // this.categories = dataCat;
    //   // console.log(this.categories);
    // }
  },
  computed: {},
};
</script>

<style>
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.3.1/dist/css/bootstrap.min.css";

.img-product {
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  height: 250px;
}

.category:hover {
  cursor: pointer;
  opacity: 1;
}

.card {
  height: 100%;
}
</style>
