<template>
  <div class="container mx-auto mb-20">
    <input
      v-model="search"
      placeholder="Nom de l'artiste"
      class="p-2 border-2 focus:border-red-500 w-full rounded-lg bg-white mb-10 mt-10 outline-none transition-all duration-500"
    />

    <div
      v-if="awaitingSearch"
      class="artist shadow-2xl bg-white p-5 w-full rounded-xl flex"
    >
      <span class="block text-xl font-bold text-center m-auto"
        >Chargement...</span
      >
    </div>
    <div
      v-else-if="!artists || !artists.length"
      class="artist shadow-2xl bg-white p-5 w-full rounded-xl flex"
    >
      <span class="block text-xl font-bold text-center m-auto"
        >Aucun résultat</span
      >
    </div>
    <div v-else class="space-y-6">
      <div v-for="artist in artists" :key="artist.id">
        <artist v-bind:artist="artist"></artist>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      search: "",
      awaitingSearch: false,
      artists: [],
    };
  },
  watch: {
    search: function () {
      if (!this.awaitingSearch) {
        this.artists = [];
        setTimeout(async () => {
          if (this.search) {
            await this.$emit("fetchArtists", this.search, (artists) => {
              this.artists = artists;
              this.awaitingSearch = false;
            });
          } else {
            this.awaitingSearch = false;
          }
        }, 1000);
      }
      this.awaitingSearch = true;
    },
  },
};
</script>