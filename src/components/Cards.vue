<template>
  <div class="container px-4">
    <div v-if="movies != null" class="flex flex-wrap -mx-1 lg:-mx-4">
      <div v-if="movies.data.Response == 'False' && movies.data.Error" class="p-5">
        <h3 v-if="search != ''" class="text-center">{{ movies.data.Error }}</h3>
      </div>
      <!-- Column -->
      <div
        v-for="movie in movies.data.Search"
        :key="movie+movie.imdbID"
        class="my-1 px-1 w-full lg:my-4 lg:px-4 lg:w-1/5"
      >
        <!-- Article -->
        <article class="overflow-hidden rounded-lg shadow-lg border-solid border-2 border-gray-500">
          <img
            alt="Placeholder"
            class="block w-full object-cover"
            style="height:350px"
            :src="[movie.Poster != 'N/A' ? movie.Poster : 'https://i.imgur.com/t21HboC.jpg']"
          />
          <!-- [movie.Poster == 'N/A' ? movie.Poster : '@/assets/images/missing.jpg'] -->
          <header class="flex items-center justify-between leading-tight p-4 bg-gray-100 h-48">
            <h1 class="text-xl text-left mr-1">
              <a
                class="no-underline hover:underline text-black font-bold"
                href="#"
              >{{ movie.Title }}</a>
            </h1>
            <p class="text-grey-darker text-sm">{{ movie.Year }}</p>
          </header>

          <footer class="flex items-center justify-between leading-none p-4 pt-1 bg-gray-100">
            <button
              v-on:click="nominate(movie)"
              :class="[listCheck(movie) ? 'opacity-50 cursor-not-allowed' : '']"
              :disabled="listCheck(movie)"
              class="bg-green-500 w-full hover:bg-green-600 text-gray-900 font-bold py-2 px-4 rounded h-12"
            >
              <span v-if="!listCheck(movie)">Nominate!</span>
              <span v-else>Already nominated!</span>
            </button>
            <!--
             <p class="text-left">A group of teachers must defend themselves from a gang of murderous kids when their school comes under siege after hours.</p>
            -->
          </footer>
        </article>
        <!-- END Article -->
      </div>
      <!-- END Column -->
    </div>
    <div v-else class="p-5">
      <h3 class="text-center">Type in the search box to find a movie to nominate!</h3>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Cards",
  props: {
    search: String,
    page: Number,
    nominationList: Array,
  },
  data() {
    return {
      searchUrl: "",
      movies: null,
      placeholder: "@/assets/images/missing.jpg",
    };
  },
  watch: {
    search(newSearch) {
      //console.log("debug: changed search value");
      axios
        .get(
          "https://www.omdbapi.com/?s=" +
            newSearch +
            "&page=" +
            this.page +
            "&type=movie&r=json&apikey=10de5c87"
        )
        .then((response) => (this.movies = response));
    },
    page() {
      //console.log("debug: changed page number!");
      if (this.search != "") {
        axios
          .get(
            "https://www.omdbapi.com/?s=" +
              this.search +
              "&page=" +
              this.page +
              "&type=movie&r=json&apikey=10de5c87"
          )
          .then((response) => (this.movies = response));
      }
    },
    movies(newMovies) {
      if (newMovies.data.Response == "True") {
        if (newMovies.data.Search.length < 10) {
          this.$emit("last-page-check", true);
        } else {
          this.$emit("last-page-check", false);
        }
      } else {
        this.$emit("last-page-check", true);
      }
    },
  },
  methods: {
    nominate(movie) {
      //console.log([movie.Title, movie.imdbID]);
      this.$emit("nominated", { title: movie.Title, id: movie.imdbID });
    },
    //checks if movie has already been added to users nominated list
    listCheck(movie) {
      for (var i = 0; i < this.nominationList.length; i++) {
        if (movie.imdbID == this.nominationList[i].id) {
          //console.log("already nominated!");
          return true;
        }
      }
      return false;
    },
  },
};
</script>