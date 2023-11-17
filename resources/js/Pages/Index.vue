<script setup>
import Layout from "../Layouts/Layout.vue";

// Importamos axios 
import axios from 'axios';
import { ref } from 'vue';


const ACCESS_KEY = '8020772d16-92413dc418-s45of3';
const URL = 'https://api.fastforex.io/currencies?api_key=' + ACCESS_KEY;


const totalResults = ref([]);
const count = ref(0);
const postData = ref({ amount: '', from: '', to: '' });
 
const getData = async () => {
 try {
   
    const response = await  axios.get(URL);
    totalResults.value = response.data;
    console.log(totalResults);
 } catch (error) {
    alert(error);
 } finally {
    
 }
};

const convert = () => {
 
  if(count.value >= 5) {
    window.location.href = "/information";
  }

  const options = {
    method: 'GET',
    url: 'https://api.fastforex.io/convert',
    params: {from: postData.value.from, to: postData.value.
      to, amount: postData.value.amount, api_key: '8020772d16-92413dc418-s45of3'},
    headers: {accept: 'application/json'}
  };

  axios
    .request(options)
    .then(function (response) {
      let to = postData.value.to;
      
      document.getElementById("result").innerHTML = '<h3 class="font-bold text-3xl">'+response.data.amount + " " + response.data.base + " = " + Object.values(response.data.result)[0] + " " + Object.keys(response.data.result)[0] + '</h3>';
      count.value = parseInt(count.value) + parseInt(1);
    })
    .catch(function (error) {
      console.error(error);
    });
};

getData(); 
</script>

<template>
  <Layout>
  
    <form>
    <div class="mt-10 space-y-12 pb-12">
  
      <div class="">

        <div class="mt-10 grid grid-cols-1 gap-x-6 gap-y-8 sm:grid-cols-8">

          <div class="sm:col-span-2 sm:col-start-2">
            <label for="amount" class="block text-sm font-medium leading-6 text-gray-900">Importe</label>
            <div class="mt-2">
              <input type="number" v-model="postData.amount" name="amount" id="amount" autocomplete="address-level2" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
            </div>
          </div>

          <div class="sm:col-span-2">
            <label for="from" class="block text-sm font-medium leading-6 text-gray-900">De:</label>
            <div class="mt-2">
              <select  id="from" v-model="postData.from" name="from" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                <option v-for="(item, key) in totalResults.currencies" :key="key" :value=key>{{ key }}</option>
              </select>
            </div>
          </div>

          <div class="sm:col-span-2">
            <label for="to" class="block text-sm font-medium leading-6 text-gray-900">a:</label>
            <div class="mt-2">
              <select id="to" name="to" v-model="postData.to" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
                <option v-for="(item, key) in totalResults.currencies" :key="key" :value=key>{{ key }}</option>
              </select>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-6 flex items-center justify-center gap-x-6">
        <button type="button" @click="convert" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Convertir</button>
      </div>
    </div>
    <div id="result">
    </div>
  </form>

</Layout>
 </template>