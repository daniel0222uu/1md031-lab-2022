<template>
  <div>
    <div>
      Burgers
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
const socket = io();




class MenuItem{
  constructor(name,url,kcal,glutenlac) {
    this.name = name;
    this.url  = url;
    this.kcal = kcal;
    this.gluten_and_lactose = glutenlac;
  }
}

let burger = new MenuItem("fire","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551",400,true)

const burgerArray = [new MenuItem("fire","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 400,true),
  new MenuItem("dank","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 700,false),
  new MenuItem("chicken","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 250,false)];





export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}



</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>