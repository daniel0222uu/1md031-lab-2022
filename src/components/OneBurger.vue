<template>

      <div class="box">  <!--fire burger div -->
        <p>
          <strong> {{"The "}}{{burger.name }} {{"burger"}} </strong> </p>
        <p>
          <img v-bind:src="burger.url"    style="width:180px;height:180px;">
        </p>
        <p>
          Contains gluten:
        </p>
        <ul v-if="burger.glutenlac == true" class="fatText">
          <li> Yes </li>
        </ul>
        <div>

            <button @click="addBurger" id="plus">
              <img src="https://thumbs.dreamstime.com/b/plus-button-icon-plus-button-icon-vector-illustration-105545139.jpg" style="width:20px;height:20px;">
            </button>     <p>{{amountOrdered}} </p>
            <button @click="amountOrdered--" id="minus">
              <img src="https://thumbs.dreamstime.com/b/round-red-minus-sign-icon-button-flat-remove-negative-symbol-isolated-white-background-eps-vector-file-172345750.jpg" style="width:20px;height:20px;">
            </button>


        </div>
      </div>


</template>
  <script>


  export default {
    data() {
      return {
        amountOrdered: 0,
      }
    },
    name: 'OneBurger',
    props:
        {
      burger: Object,
    },
    methods: {
      addBurger: function(){
        this.amountOrdered += 1;
        this.$emit('orderedBurger', {name: this.burger.name,
                                      amount: this.amountOrdered});
      },
      say(){
        console.log("du klickade på plus")
      },
    },
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#plus{
  height: 20px;
  length: 20px;
}

</style>
  