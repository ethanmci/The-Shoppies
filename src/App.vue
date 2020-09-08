<template>
  <div id="app" class="h-full w-full">
    <span>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="80"
        height="80"
        viewBox="0 0 24 24"
        class="mx-auto mt-8"
      >
        <path
          fill="#2F855A"
          d="M13.408 18c.498-3.947 5.592-7.197 5.592-17h-14c0 9.803 5.105 13.053 5.604 17h2.804zm-3.614-11.472l1.46-.202.643-1.326.643 1.326 1.46.202-1.063 1.021.26 1.451-1.3-.695-1.3.695.26-1.451-1.063-1.021zm-3.803 4.128c.286.638.585 1.231.882 1.783-4.065-1.348-6.501-5.334-6.873-9.439h4.077c.036.482.08.955.139 1.405h-2.689c.427 2.001 1.549 4.729 4.464 6.251zm10.009 10.963v1.381h-8v-1.381c1.941 0 2.369-1.433 2.571-2.619h2.866c.193 1.187.565 2.619 2.563 2.619zm8-18.619c-.372 4.105-2.808 8.091-6.873 9.438.297-.552.596-1.145.882-1.783 2.915-1.521 4.037-4.25 4.464-6.251h-2.688c.059-.45.103-.922.139-1.405h4.076z"
        />
      </svg>
      <h1 class="text-2xl font-black text-gray-800 text-center">THE SHOPPIES</h1>
    </span>
    <transition name="slide-full">
    <div v-if="nominations.length == 5" class="md:w-5/6 w-full m-auto shadow-lg mt-4 rounded-lg bg-green-500" style="padding: 0.5em">
      <p class="text-center font-bold text-white">Thank you for selecting 5 movies for nomination! <br>If you would like to change your list, use the remove buttons found in your list of nominated movies.</p>
    </div>
    </transition>
    <div class="md:w-5/6 w-full m-auto my-8 rounded-lg shadow-lg bg-gray-200">
      <div class="flex content-start flex-wrap bg-gray-100" style="min-height: 600px;">
        <!-- sidebar -->
        <div class="md:w-1/5 w-full py-8 px-4">
          <div
            class="lg:fixed lg:w-1/6 text-gray-700 rounded-lg shadow-lg bg-white p-8 border-solid border-2 border-gray-500"
            style="min-height:500px;"
          >
            <h2 class="text-left text-black font-bold">Your Nominations</h2>
            <transition name="slide">
            <p v-if="nominations.length <= 0" class="p-2">Movies you nominate will show up here!</p>
            </transition>
            <ul class="list-disc p-4 text-left">
              <transition-group name="list">
                <li
                  v-for="(nomination, i) in nominations"
                  :key="nomination.id"
                  style="min-height: 60px;"
                >
                  <div class="flex mb-4">
                    <div class="w-2/3">
                      <span class="align-bottom">{{ nomination.title }}</span>
                    </div>
                    <div class="w-1/3">
                      <button
                        v-on:click="removeNom(i)"
                        class="bg-red-700 hover:bg-red-800 text-gray-100 font-bold p-2 rounded inline-flex items-center"
                      >
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          width="16"
                          height="16"
                          viewBox="0 0 24 24"
                          style="-webkit-filter: invert(100%); filter: invert(100%);"
                        >
                          <path
                            d="M3 6v18h18v-18h-18zm5 14c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm5 0c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm5 0c0 .552-.448 1-1 1s-1-.448-1-1v-10c0-.552.448-1 1-1s1 .448 1 1v10zm4-18v2h-20v-2h5.711c.9 0 1.631-1.099 1.631-2h5.315c0 .901.73 2 1.631 2h5.712z"
                          />
                        </svg>
                        <span class="text-sm">Remove</span>
                      </button>
                    </div>
                  </div>
                </li>
              </transition-group>
            </ul>
          </div>
        </div>
        <!-- cards + search container -->
        <div class="md:w-4/5 w-full p-4">
          <div class="h-22 items-center justify-center mx-4 pt-4">
            <input
              v-model="searchVal"
              placeholder="Search for a movie..."
              style="outline: none"
              class="mb-auto mt-auto border-solid border-2 border-gray-500 rounded-lg w-full h-16 px-4 focus:border-green-600 focus:shadow-2xl"
            />
          </div>
          <Cards
            :search="searchVal"
            :page="page"
            :nomination-list="nominations"
            v-on:nominated="addNomination"
            v-on:last-page-check="lastPageCheck"
          ></Cards>
          <div class="flex my-5 ml-4" v-if="searchVal != ''">
            <div class="flex mx-auto">
              <button
                v-if="page > 1"
                class="bg-green-500 hover:bg-green-700 text-white block rounded-lg font-bold py-4 px-6 mr-2 flex items-center"
                v-on:click="[page != 1 ? page-- : null];"
              >
                <svg
                  class="h-5 w-5 mr-2 fill-current"
                  version="1.1"
                  id="Layer_1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                  x="0px"
                  y="0px"
                  viewBox="-49 141 512 512"
                  style="enable-background:new -49 141 512 512;"
                  xml:space="preserve"
                >
                  <path
                    id="XMLID_10_"
                    d="M438,372H36.355l72.822-72.822c9.763-9.763,9.763-25.592,0-35.355c-9.763-9.764-25.593-9.762-35.355,0 l-115.5,115.5C-46.366,384.01-49,390.369-49,397s2.634,12.989,7.322,17.678l115.5,115.5c9.763,9.762,25.593,9.763,35.355,0 c9.763-9.763,9.763-25.592,0-35.355L36.355,422H438c13.808,0,25-11.193,25-25S451.808,372,438,372z"
                  />
                </svg>
                Previous page
              </button>
              <button
                v-if="page > 1"
                class="border-solid border-2 border-green-500 hover:border-green-700 text-gray-800 block rounded-lg font-bold py-4 px-6 mx-2 flex items-center"
                v-on:click="page=1"
              >Return to First Page</button>
              <button
                v-if="lastPage != true"
                class="bg-green-500 hover:bg-green-700 text-white block rounded-lg font-bold py-4 px-6 ml-2 flex items-center"
                v-on:click="page++;"
              >
                Next page
                <svg
                  class="h-5 w-5 ml-2 fill-current"
                  clasversion="1.1"
                  id="Layer_1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                  x="0px"
                  y="0px"
                  viewBox="-49 141 512 512"
                  style="enable-background:new -49 141 512 512;"
                  xml:space="preserve"
                >
                  <path
                    id="XMLID_11_"
                    d="M-24,422h401.645l-72.822,72.822c-9.763,9.763-9.763,25.592,0,35.355c9.763,9.764,25.593,9.762,35.355,0
            l115.5-115.5C460.366,409.989,463,403.63,463,397s-2.634-12.989-7.322-17.678l-115.5-115.5c-9.763-9.762-25.593-9.763-35.355,0
            c-9.763,9.763-9.763,25.592,0,35.355l72.822,72.822H-24c-13.808,0-25,11.193-25,25S-37.808,422-24,422z"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Cards from "./components/Cards";

export default {
  name: "App",
  components: {
    Cards,
  },
  data() {
    return {
      searchVal: "",
      page: 1,
      nominations: [],
      lastPage: false,
    };
  },
  watch: {
    searchVal() {
      this.page = 1;
    },
    nominations(newNomin){
      localStorage.setItem("nominations", JSON.stringify(newNomin));
    },
  },
  methods: {
    addNomination: function (e) {
      //console.log("parent:" + e.title + e.id);
      if (this.nominations.length < 5) {
        this.nominations.push(e);
      }
    },
    removeNom(i) {
      this.nominations.splice(i, 1);
    },
    lastPageCheck: function(e){
      this.lastPage = e;

    }
  },
  mounted(){
    if(localStorage.nominations){
      this.nominations = JSON.parse(localStorage.getItem("nominations"));
    }
  }
};
</script>

<style>
</style>
