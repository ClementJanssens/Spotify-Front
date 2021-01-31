<template>
  <div class="container mx-auto mb-20">
    <artists v-if="token" @fetchArtists="fetchArtists"></artists>

    <div v-else class="artist shadow-2xl bg-white p-5 w-full rounded-xl mt-10">
      <span class="block text-xl font-bold text-center m-auto"
        >Vous n'êtes pas connecté à Spotify</span
      >
      <a
        href="https://accounts.spotify.com/authorize?client_id=92ecc7e9b5c54ca1b21b337ea997d8dc&response_type=token&redirect_uri=http://localhost:3000/spotify-auth-2&scope=user-read-private%20user-read-email&state=34fFs29kd09"
        ><button
          class="block py-2 px-10 mt-5 text-center m-auto bg-green-500 text-white font-bold rounded-full"
        >
          Se connecter
        </button></a
      >
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      token: null,
      search: "",
      awaitingSearch: false,
      artists: [],
    };
  },
  created: function () {
    const UrlHash = this.$route.hash;
    const token = UrlHash.split("#access_token=")[1];

    if (token) this.token = token;
  },
  methods: {
    fetchArtists: async function (name, callback) {
      const loadedArtists = await this.$axios
        .$get(
          `https://api.spotify.com/v1/search?q=${name}&type=artist&limit=10`,
          {
            headers: { Authorization: `Bearer ${this.token}` },
          }
        )
        .then((response) => {
          return response.artists;
        });

      for (let artist of loadedArtists.items) {
        const albums = await this.$axios
          .$get(
            `https://api.spotify.com/v1/artists/${artist.id}/albums?include_groups=single,album`,
            {
              headers: { Authorization: `Bearer ${this.token}` },
            }
          )
          .then((response) => {
            return response.items;
          });

        artist.albums = albums;
      }

      callback(loadedArtists.items);
    },
  },
};
</script>