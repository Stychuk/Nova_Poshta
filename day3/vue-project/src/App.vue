<template>
  <div>
    <h1>Нова пошта</h1>
    <h2>Оберіть населений пункт</h2>
    <select v-model="punkt" @click="viddil()">
      <option v-for="city in cities" v-bind:key="city.Ref" v-bind:value="city.Description">{{city.Description}}</option>
    </select>
    <br>
    <h2>Поштове відділення</h2>
    <select>
      <option v-for="punkt in departments" v-bind:key="punkt.CityID" v-bind:value="punkt.Description">{{punkt.Description}}</option>
    </select>
  </div>
</template>

<script>
  import axios from 'axios'
  
  export default {
    data() {
      return {
        cities:[],
        departments:[],
        punkt:"Абазівка (Полтавський р-н, Полтавська обл)"
      }},
      
      mounted: function(){
        axios.post("https://api.novaposhta.ua/v2.0/json/Address/getCities", {
          apiKey: "2d5f2229c3a4b3a0d142e9b7b27a1748",
          modelName: "Address",
          calledMethod: "getCities",
          methodProperties: {}
        })
        .then((response)=>{
          console.log(response.data);
          this.cities = response.data.data;
        })
      },

      methods:{
        viddil() {
          axios.post("https://api.novaposhta.ua/v2.0/json/Address/getWarehouses", {
            apiKey: "2d5f2229c3a4b3a0d142e9b7b27a1748",
            modelName: "Address",
            calledMethod: "getWarehouses",
            methodProperties: {
              CityName: this.punkt,
            }
          })
          .then((response)=>{
            console.log(response.data);
            this.departments = response.data.data;
          })
        }
      }
  }
</script>

<style scoped>

</style>
