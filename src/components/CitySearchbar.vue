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
       }
  },
} }
</script>

