<script setup>
import { ref, reactive } from 'vue';

// Get a joke from third party API

const joke = reactive({
  value:"Do you want to get more new new jokes? Click on the 'get a new joke' button"
})

function getAJoke() {
  fetch('https://api.chucknorris.io/jokes/random')
  .then(r=>r.json())
  .then(data => {
    joke.value = data.value  
  })
}

// Get All Country & Capital list from Third Party API

const countries = reactive([
  {name: "Bangladesh", capital: "Dhaka"}
])

function getAllCountry(Clist = 'All') {
  countries.length = 0
  fetch('https://restcountries.com/v3.1/all?fields=name,capital')
  .then(r=>r.json())
    .then(data => {
      if('All' == Clist){
      data.forEach(country => {
        countries.push({
          name: country.name.common,
          capital:country.capital[0]
        })
      })
      } else {
        data.filter(c=>c.name.common.startsWith(Clist)).forEach(country => {
          countries.push({
            name: country.name.common,
            capital: country.capital[0]
          })
        })
      }
  })
}





</script>

<template>
  <section class="flex flex-col items-center w-[500px]">
    <p class="text-yellow-500">
      {{ joke.value }}
    </p>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-48 mt-5 cursor-pointer" @click="getAJoke()">
      Get a new joke
    </button>

  </section>

  <section>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-48 mt-5 cursor-pointer" @click="getAllCountry()">
      Country List
    </button>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-10 ml-2 mt-5 cursor-pointer" @click="getAllCountry('A')">
      A
    </button>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-10 ml-2 mt-5 cursor-pointer" @click="getAllCountry('B')">
      B
    </button>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded w-10 ml-2 mt-5 cursor-pointer" @click="getAllCountry('C')">
      C
    </button>
    <ul class="mt-10">
      <li v-for="country in countries" :key="country.name">
        {{ country.name }} = {{ country.capital }}
      </li>
    </ul>
  </section>
</template>

<style scoped>
</style>
