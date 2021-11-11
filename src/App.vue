<template>
    <div class="content">
        <select v-model="AreaRef" v-on:change="getCities">
            <option value="" selected disabled>Оберiть область</option>
            <option v-for="(area, index) in Areas" :key="index" :value="area.Ref"> {{area.Description}}</option>
        </select>
        <select v-model="CityRef" v-on:change="getWarehouses">
            <option value="" selected disabled>Оберiть населений пункт</option>
            <option v-for="(city, index) in Cities" :key="index" :value="city.Ref"> {{city.Description}}</option>
        </select>
        <select>
            <option value="null" selected disabled>Оберiть відділення</option>
            <option v-for="(warehouse, index) in Warehouses" :key="index"> {{warehouse.Description}}</option>
        </select>
    </div>
</template>

<script>

import axios from 'axios'


export default {
    
    data: function() {
        return {
           API_KEY: 'd358e01a2ab49e9fa7e0627b1bd6a6c8',
           url: 'https://api.novaposhta.ua/v2.0/json/',
           Areas: [],
          AreaRef: '',
          Cities: [],
          CityRef: '',
          Warehouses: [],
        }
    },
    mounted: function(){
        axios.post(this.url, {
          "apiKey": this.API_KEY,
          "modelName": "Address",
          "calledMethod": "getAreas",
          "methodProperties": {}
          })
        .then (res => {
            this.Areas = res.data.data;
        })
    },
    methods: {        
        getCities: function(){
           axios.post(this.url, {
                "modelName": "Address",
                "calledMethod": "getCities",
                "methodProperties": {
                    "AreaRef": this.AreaRef,
                },
                "apiKey": this.API_KEY,
            })
            .then (res => {
              console.log(res.data.data)
                this.Cities = res.data.data
            
            })
        },
        getWarehouses: function(){
          console.log(this.CityRef)
           axios.post(this.url, {
                  "modelName": "AddressGeneral",
                  "calledMethod": "getWarehouses",
                  "methodProperties": {
                      "CityRef": this.CityRef
                  },
                  "apiKey": this.API_KEY
              })
            .then (res => {
                
                this.Warehouses = res.data.data
                console.log(this.Warehouses)
            })
        },

    }
}
</script>

<style scoped>
  select{
    width: 800px;
    height: 40px;
    font-size: 24px;
    margin-bottom: 20px;
  }
  option{
    overflow:scroll;
  }
  .content{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }
</style>