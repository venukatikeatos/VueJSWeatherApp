<template>
  <div class="search">
            <input  type="text" placeholder="enter your city name" spellcheck="false" 
            @keyup.enter="checkWeather()">
            <button @click="checkWeather()"><img src="@/images/search.png"></button>
          
        </div>
</template>

<script>
export default {
    name:"SearchBar",
    props:['city', 'che:function']

    
}
</script>

<style>

</style>