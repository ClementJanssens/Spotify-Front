<template>
  <div class="container mx-auto mb-20">
    <artists @fetchArtists="fetchArtists"></artists>
  </div>
</template>

<script>
import Artists from "../components/Artists.vue";
export default {
  components: { Artists },
  methods: {
    fetchArtists: async function (name, callback) {
      await this.$axios
        .$get(`http://localhost:3030/spotify-service?artistName=${name}`)
        .then((response) => {
          callback(response.items);
        });
    },
  },
};
</script>