<template>
  <div id="components-demo">
    <div class="cityHistory-div" v-for="(item, index) in cityHistory.slice().reverse()" v-bind:key="index"
>
      <div class="cityHistory-box">
        <h2 class="cityHistory-title">{{ item.name }}</h2>
        <h3 class="cityHistory-temp">{{ item.temp }}°c</h3>
        <p class="cityHistory-weather">{{ item.weather }}</p>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'CityCard',
  props: ['cityHistory', 'name',
    'temp',
    'weather'
  ],
}

</script>

