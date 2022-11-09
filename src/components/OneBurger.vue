<template>
  <div class="box fire"><!-- fire burger div -->
    <p id="burgerName">
      <strong> The fire burger </strong> </p>
    <p>
      <img src="https://i.ytimg.com/vi/i7qI52gVPlA/maxresdefault.jpg" style="width:180px;height:180px;">
    </p>
    <p>
      Ingredients:
    </p>
    <ul class="fatText">
      <li> Avocado</li>
      <li> Tomato</li>
    </ul>
  </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  
  </style>
  