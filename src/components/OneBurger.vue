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
      </div>



</template>
  <script>
  export default {
    name: 'OneBurger',
    props:
        {
      burger: Object,
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


</style>
  