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
                    v-on:orderedBurger="addToOrder($event)"


            />
          </div>
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
              <input v-model="fullName" type="text" id="firstname" name="fn" required="required" placeholder="First and last name">
            </p>
            <p>
              <label for="email_adress">Email adress</label><br>
              <input v-model="emailAdress" type="text" id="email_adress" name="em" required="required" placeholder="Email adress">
            </p>
          <!--  <p>
              <label for="street_name">Street</label><br>
              <input v-model="streetName" type="text" id="street_name" name="sname" required="required" placeholder="Street name">
            </p>
            <p>
              <label for="street_number">House</label><br>
              <input v-model="houseNumber" type="number" id="street_number" name="snumber" required="required" placeholder="House number">
            </p>--> <!--Dessa skulle man ta bort -->
          </form>

          <label for="recipient">Payment method</label>
          <select  id="recipient" name="Klarna" v-model="selectedPayment">
            <option>Swish</option>
            <option>VISA / Mastercard</option>
            <option>Klarna</option>
            <option>Doge coin</option>
          </select>

          <fieldset>
            <legend>Specify your gender</legend>

            <div class="genderButtons">
              <input type="radio" id="Male" name="drone" value="Male" v-model="pickedGender">

              <label for="Male">Male</label>
            </div>

            <div class="genderButtons">
              <input type="radio" id="Female" name="drone" value="Female" v-model="pickedGender">
              <label for="Female">Female</label>
            </div>

            <div class="genderButtons">
              <input type="radio" id="nonbinary" name="drone" value="nonbinary" v-model="pickedGender">
              <label for="nonbinary">Other</label>
            </div>

          </fieldset>


          <button @click="addOrder"  type="submit">
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
      <div id="dots" class="map" v-on:click="setLocation">
        <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" >
        </div>
      </div>
    </div>

  </html>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
const socket = io();


let customerOffset = {};
var personalInformationArray = []; //vi måste hitta nåt sätt att pusha
// denna in organiskt



export default {

  name: 'HomeView',
  components: {
    Burger,
  },
  data: function () {
    return {
      selectedPayment: "Swish",
      pickedGender:"" ,
      location: {x: 0, y: 0},
      burgerArray: [],
      fullName: '',
      emailAdress: '',
      streetName: '',
      houseNumber: '',
      burgers: menu,
      theOrderedBurger: {}
    }
  },
  methods: {
    setLocation: function(event) {
      customerOffset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      var exactPos = {x: event.clientX - 10 - customerOffset.x,
                      y: event.clientY - 10 - customerOffset.y}
      console.log(exactPos);
      this.location = exactPos;
    },
    addToOrder: function (event) {
      //this.orderedBurgers[event.name] = event.amount; den här var från labben men krångla för mycket
      console.log("nu trycks parent")
      this.theOrderedBurger[event.name] = event.amount;
      var toConsole = Object.entries(this.theOrderedBurger);
      console.log(toConsole);
      //this.burgerArray.push(event.name);
      //console.log(event.name);
      //console.log(event.amount);
      //console.log(this.burgerArray);
    },
    say: function(name,email){
      personalInformationArray.push(name);
      personalInformationArray.push(email)
      console.log(personalInformationArray);
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000); //ändrar man denna till "T" så får man bara en so
    },
    addOrder: function() { //min egna order add metod
      socket.emit("addOrder", {orderId: this.getOrderNumber(),
                                        details: {x: this.location.x,
                                                  y: this.location.y},
                                        orderItems: Object.entries(this.theOrderedBurger),
                                        deliveryInfo: [this.fullName,this.emailAdress, this.pickedGender,this.selectedPayment]
      }
      );
      console.log("du klickade på orderAdds");

    },
  }
}



</script>

<style>

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}

@media screen and (max-width: 800px) {
  h1 {
    font-size: 6vw;
  }
}
h1{
  font-size: 6vw;
}ad


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
.map {
    overflow: scroll;
    width: 1920px;
    height: 1078px;
    background: url("/Users/danielceoca/Desktop/Gränssnittprogrammering/Labb1/1md031-lab-2022/public/img/polacks.jpg")
  }
</style>