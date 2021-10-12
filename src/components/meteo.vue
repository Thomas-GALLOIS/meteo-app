<template>
 
  <div class="meteo">
  <h3 class="date">{{dateTime}}</h3>
  <p class="description">{{description}}</p>
  <p>Température: <br/><br/>{{temp}}°c</p>
  <p>Températures prévues: <br/><br/>Min: {{tempMin}}°c <br/>Max: {{tempMax}}°c</p>
  <p>Vitesse du vent: {{speed}}km/h</p>
  <img :src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" />
  </div>

      
  
</template>

<script>
export default {
  
  props: {
    dateTime: String,
    temp: Number,
    tempMin: Number,
    tempMax: Number,
    speed: Number,
    description: String,
    icon: String,
  },

  
};



  

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
