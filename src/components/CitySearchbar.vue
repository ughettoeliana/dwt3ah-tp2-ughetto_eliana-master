<template>
    <div class="search-box">
      <input type="text" class="search-bar" placeholder="Search the city..." v-model="value"
        v-on:keypress="onKeyPress" />
    </div>

</template>

<script>
export default {
  name: 'CitySearchbar',
  props: ['onSearch'],
  data: () => {
    return {
      value: '',
    }
  },

  methods: {
    onKeyPress(e) {
      if (e.key == "Enter" && this.value) {
        this.onSearch(this.value);
        this.value = '';
       }// else {
      //   return `<div id='errorText' class="errorText">
      //   <p>We didn't find the city {{ value }}, try again with another one</p>
      //   </div>`
      // }
    },

    // hasWeather() {
    //   return this.cityHistory.length > 0;
    // },

    // weatherClass() {
    //   return this.value > null ? 'errorText' : '';
    // },
  }
}
</script>

