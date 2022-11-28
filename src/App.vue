<script>
  export default {
    name: 'App',
    data(){
      return {
        joke: "Waiting for a joke...",
        chuckNorrisUrl: "",
        categories: [
          "(none)",
          "animal",
          "career",
          "celebrity",
          "dev",
          "explicit",
          "fashion",
          "food",
          "history",
          "money",
          "movie",
          "music",
          "political",
          "religion",
          "science",
          "sport",
          "travel"
      ],
      isCategorySelected: false,
      }
    },
    props:{

    },
    methods: {
      getChuckNorrisJoke() {
        // Add a spinner while waiting for the joke to load
        this.joke = "Loading...";
        this.chuckNorrisUrl = this.isCategorySelected ? `https://api.chucknorris.io/jokes/random?category=${this.categorySelected}` : "https://api.chucknorris.io/jokes/random";
        fetch(this.chuckNorrisUrl)
          .then(response => response.json())
          .then(data => this.joke = data.value)
      },
      getChuckNorrisCategories() {
        fetch('https://api.chucknorris.io/jokes/categories')
          .then(response => response.json())
          .then(data => this.categories = data)
      },
      setIsCategorySelected() {
        if (this.categorySelected === "(none)") {
          this.isCategorySelected = false;
        } else {
          this.isCategorySelected = true;
        }
      },
      setCategory(e){
        let categorySelected = e.target.value;
        console.log(categorySelected);
        this.categorySelected = categorySelected;
        this.setIsCategorySelected();
      }
    },
    beforeMount() {
      // this.getChuckNorrisCategories();
    }
  }
</script>

<template>
    <h1>Chuck Norris Jokes</h1>
    <p>{{ joke }}</p>
    <button v-on:click="getChuckNorrisJoke">Press this button to get a new joke</button>
  <div class="radio-group categories">
    <label v-for="category in categories" :key="category">
      <input class="category" name="category" type="radio" :value="category" @change="setCategory">
      {{ category }}
    </label>
  </div>
</template>

<style>
  .categories {
    display: flex;
    flex-direction: column;
  }
</style>
