<template>
  <div class="home">
    <h1>Movies</h1>
    <p>The Complete list of your favourite movies</p>
    <input
      class="searchBox"
      type="text"
      v-model="searchInput"
      placeholder="search..."
    />
    <div class="movieContainer" v-if="filteredList.length">
      <li class="moviesList" v-for="(each, index) in filteredList" :key="index">
        <p class="movienameClass">
          Movie Name : <span>{{ each.name }}</span>
        </p>
        <p class="directornameClass">
          Directed By : <span>{{ each.director }}</span>
        </p>
      </li>
    </div>
    <h2 v-else>No results found...</h2>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
@Component({
  components: {},
})
export default class HomeView extends Vue {
  moviesList: any[] = [];
  directorsList: any[] = [];
  fullList: any[] = [];
  searchInput: any = "";

  async mounted() {
    let directorsList = await fetch(
      "https://mocki.io/v1/98864832-79a3-4775-b184-c477b3a24c6a"
    );
    let directorsData = await directorsList.json();

    let moviesList = await fetch(
      "https://mocki.io/v1/5210535c-0673-42a0-8608-85b04158e5d2"
    );
    let movieData = await moviesList.json();
    this.moviesList = movieData;
    this.directorsList = directorsData;
    this.moviesList.forEach((movies) => {
      this.directorsList.forEach((directors) => {
        if (movies.director == directors.id) {
          this.fullList.push({ name: movies.title, director: directors.name });
        }
      });
    });
  }

  get filteredList() {
    let filteredList = this.fullList.filter(
      (each) =>
        each.name.toLowerCase().includes(this.searchInput.toLowerCase()) ||
        each.director.toLowerCase().includes(this.searchInput.toLowerCase())
    );
    return filteredList;
  }
}
</script>

<style>
.movieContainer {
  background-color: beige;
  color: black;
  height: 100%;
  width: 50%;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

.home {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.searchBox {
  width: 300px;
  height: 30px;
  padding-left: 10px;
  margin-bottom: 20px;
}
.moviesList {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  list-style-type: none;
  width: 40%;
  background-color: brown;
  margin: 20px;
  border-radius: 5px;
}

.movienameClass,
.directornameClass {
  color: white;
  font-weight: 600;
}
.movienameClass {
  margin-bottom: 0px;
}
.movienameClass span,
.directornameClass span {
  color: rgb(236, 238, 106);
}
</style>
