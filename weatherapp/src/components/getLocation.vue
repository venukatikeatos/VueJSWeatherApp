<template>
  <div>
    <p v-if="city">Your current city1: {{ city }}</p>
    <p v-else>Loading city...</p>
  </div>
</template>

<script>
export default {
    name:'veP',
  data() {
    return {
      city: null,
    };
  },
   mounted() {
    this.getCity();
  },
  methods: {
    async getCity() {
      if ('geolocation' in navigator) {
        navigator.geolocation.getCurrentPosition(
          async (position) => {
            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;

            try {
              const response = await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latitude}&lon=${longitude}`);
              const data = await response.json();
              this.city = data.address.city || 'City not found';
            } catch (error) {
              console.error('Error getting city:', error);
              this.city = 'City not found';
            }
          },
          (error) => {
            console.error('Error getting location:', error);
            this.city = 'Location not available';
          }
        );
      } else {
        console.error('Geolocation is not available in this browser.');
        this.city = 'Geolocation not supported';
      }
    },
  },
};
</script>
