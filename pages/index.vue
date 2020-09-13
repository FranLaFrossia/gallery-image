<template>
<div class="app">
  <header>
    <div class="wrapper --main">
      <h1>{{ appTitle }}</h1>
      <h2>{{ appSubtitle }}</h2>
    </div>
  </header>
  <main>
    <div class="wrapper --main">
        <!-- v-for="(imgs, index) in apiImages" -->
        <!-- :key="index" -->
      <Gallery
        :galleryImages="apiImages"></Gallery>
      <button
        @click="loadMoreImages()"
        class="btn"
      >Load more images</button>
    </div>
  </main>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      appTitle: 'Image Gallery',
      appSubtitle: 'Just an image gallery for getting a job. Hopefully.',
      apiImages: [],
      fetchPage: 1,
    }
  },
  methods: {
    // Fetch 15 more images from Unsplash API
    async loadMoreImages() {
      this.fetchPage++;
      const moreImgs = await fetch(
      `https://api.unsplash.com/photos/?page=${this.fetchPage}&per_page=15&client_id=9ca4NFCNeR3ZWNBaDCruj-iF5uuSs3kOEirD7HRTySY`
      ).then(res => res.json());
      this.apiImages = this.apiImages.concat(moreImgs);
    }
  },
  async fetch() {
    // Fetch 15 images from Unsplash API
    this.apiImages = await fetch(
      'https://api.unsplash.com/photos/?page=1&per_page=15&client_id=9ca4NFCNeR3ZWNBaDCruj-iF5uuSs3kOEirD7HRTySY'
    ).then(res => res.json())
  }
}
</script>

<style>
header {
  padding: 2rem 0;
  color: #333;
}
  h2 {
    font-size: 1.25em;
    margin-top: .5rem;
    font-weight: 400;
  }
main {
  margin-bottom: 3rem;
}
.wrapper {
  margin: 0 auto;
}
.wrapper.--main {
  max-width: 1180px;
  padding: 0 20px;
}

.btn {
  background: rgba(25, 25, 25, .85);
  border: 0;
  border-radius: 4px;
  color: white;
  cursor: pointer;
  font-size: 1rem;
  line-height: 1.5;
  margin: 1.5rem auto;
  outline: none;
  padding: .5rem 1rem;
  /* text-transform: uppercase; */
}
  .btn:hover {
    -webkit-box-shadow: 0 1px 0 rgba(0, 0, 0, 0.15);
    box-shadow: 0 1px 0 rgba(0, 0, 0, 0.15); 
    background: rgba(25, 25, 25, .95);
  }
</style>
