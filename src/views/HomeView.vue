<template>
  <!DOCTYPE HTML>
  <html lang="en">
  <header class>

    <title>This is the page title</title>
    <meta charset="utf-8"/>
  </header>

  <div id="wholediv">




    <div id="longImagediv" >
      <img src="https://as1.ftcdn.net/v2/jpg/03/02/41/48/1000_F_302414819_juA2QCzXIICStqJOUHQicECan4WKZvN9.jpg" id="longImage">
      <div class="centerLongImageText">
        <h1 class="ourHeader">Welcome to Nonexistant Burgers</h1>
      </div>



    </div>




    <div class="welcomeArea separateArea welcomingText"> <link rel="stylesheet">




      <div>
        <header>

          <p>
            <strong> Select burger</strong>
          </p>
          <p>This is where you execute your burger selection
          </p>
        </header>
      </div>




      <div class="burgerMenu welcomeArea welcomingText wrapper"> <!--our entire burger menu -->

        <div class="box">
          <div>
            <Burger v-for="burger in burgers"
                    v-bind:burger="burger"
                    v-bind:key="burger.name"
                    v-bind:style="burger.gridPosition"
            />
          </div>
        </div>



      </div>






    </div>





    <main>

      <div class="separateArea" id="customerDiv"> <!-- customer div -->

        <p>
          <strong>
            Customer information
          </strong>
        </p>
        <p>
          This is where you provide necessary information
        </p>


        <section id="contact">
          <form>
            <p>
              <label for="firstname">Full name</label><br>
              <input type="text" id="firstname" name="fn" required="required" placeholder="First and last name">
            </p>
            <p>
              <label for="email_adress">Email adress</label><br>
              <input type="text" id="email_adress" name="em" required="required" placeholder="Email adress">
            </p>
            <p>
              <label for="street_name">Street</label><br>
              <input type="text" id="street_name" name="sname" required="required" placeholder="Street name">
            </p>
            <p>
              <label for="street_number">House</label><br>
              <input type="number" id="street_number" name="snumber" required="required" placeholder="House number">
            </p>
          </form>

          <label for="recipient">Payment method</label>
          <select id="recipient" name="Klarna">
            <option>Swish</option>
            <option>VISA / Mastercard</option>
            <option>Klarna</option>
          </select>

          <fieldset>
            <legend>Specify your gender</legend>

            <div class="genderButtons">
              <input type="radio" id="Male" name="drone" value="Male"
              >
              <label for="Male">Male</label>
            </div>

            <div class="genderButtons">
              <input type="radio" id="Female" name="drone" value="Female">
              <label for="Female">Female</label>
            </div>

            <div class="genderButtons">
              <input type="radio" id="nonbinary" name="drone" value="nonbinary" >
              <label for="nonbinary">Other</label>
            </div>

          </fieldset>

          <button type="submit">
            <img src="https://www.pngall.com/wp-content/uploads/12/Order-Now-Button-PNG-Photo.png" style="width:300px;height:100px;" >
          </button>


        </section>

      </div>
    </main>


    <hr>
    <footer>
      <p> ® Nonexistant burgers AB</p>
    </footer>
    <div>
      <div id="map" v-on:click="addOrder">
        click here
      </div>
    </div>
  </div>


  </html>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import OneBurger from "@/components/OneBurger";
const socket = io();





class MenuItem{
  constructor(name,url,kcal,glutenlac,position) {
    this.name = name;
    this.url  = url;
    this.kcal = kcal;
    this.gluten_and_lactose = glutenlac;
    this.gridPosition = position;
  }
}


const burgerArray = [new MenuItem("fire","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 400,false,1),
  new MenuItem("dank","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 250,true,2),
  new MenuItem("chicken","https://bildix.mmcloud.se/bildix/api/images/4c7ccd16-4dda-40db-b683-7a6bc14d82ff.jpeg?fit=crop&w=980&h=551", 700,true,3 )]




export default {
  name: 'HomeView',
  components: {
    Burger,
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

@media screen and (max-width: 800px) {
  h1 {
    font-size: 6vw;
  }
}
h1{
  font-size: 6vw;
}

<!--här var det importer förut men inte längre-->


body {
  font-family: 'Island Moments', cursive;
  font-family: 'Mukta', sans-serif;
}

.welcomingText{
  font-family: Helvetica;
  color: white;
}

.welcomeArea{
  background-color:black;
}

button:hover{
  background-color:springgreen;
  outline-style: auto;
  box-shadow: 0 0 0 2px #5a01a7;
  transition: 0.2s;

}

.genderButtons{
  margin-top: 20px;
}

.burgerMenu{
  margin-left: 300px;
}

.separateArea{
  border: 2px solid orange;
  margin: 10px;
  padding-left: 40px;

}


#customerDiv{
  background-color: white;
  color: black    ;
}

#wholediv{
}


#longImage{
  width: 100%;
  opacity: 40%;
  height: 300px;
}

#longImagediv{
  position: relative;
  text-align: center;
  padding-left: 10px;
  padding-right: 10px;
}


.wrapper{
  display: grid;
  grid-gap: 10px;
  grid-gap: 80px;
}

.box{
  padding: 20px;
  float: left;
}


.centerLongImageText{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}

.fatText{
  font-weight: bold;
}
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>