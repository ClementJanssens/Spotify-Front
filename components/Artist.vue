<template>
  <div class="artist shadow-2xl bg-white p-5 w-full rounded-xl flex">
    <img
      v-if="artist.images[0]"
      class="w-1/5 h-auto rounded-md self-start"
      v-bind:src="artist.images[0].url"
      v-bind:alt="artist.name"
    />
    <div class="infos ml-5">
      <span class="block text-xl font-bold mb-5">{{ artist.name }}</span>
      <span class="block mb-2 font-bold text-xs uppercase">Artists &amp; albums</span>
      <ul class="border-l-4 border-red-500 pl-5" v-for="album of artist.albums" :key="album.id">
        <li>
          {{ album.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    artist: Object,
  },
};
</script>