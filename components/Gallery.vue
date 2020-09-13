<template>
<div class="gallery-wrapper">
  <div class="gallery">
    <figure
      v-for="(img, index) in galleryImages"
      :key="index"
      class="gallery-item"
      :class="`gallery-item-${index + 1}`"
      @click="showModal(index)">
      <img :src="img.urls.regular" class="gallery-img" :alt="img.alt_description">
      <div class="gallery-title">
        {{ img.user.first_name }}
      </div>
    </figure>
  </div>
  <GalleryModal :images="galleryImages" ref="modal"></GalleryModal>
</div>
</template>

<script>
export default {
  name: 'Galleryv1',
  props: {
    galleryImages: {
      type: Array,
      default: () => [],
    }
  },
  data() {
    return {
      modalActive: false
    }
  },
  methods: {
    // Triggers functions from GalleryModal child component 
    showModal(index) {
      this.$refs.modal.show();
      this.$refs.modal.setIndex(index);
    }
  }
}
</script>

<style scoped>
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(270px, 1fr));
  grid-auto-rows: 25vh;
  grid-gap: 15px;
  grid-auto-flow: dense;
}
.gallery-item {
  position: relative;
  cursor: pointer;
}
  .gallery-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
  }

  .gallery-title {
    align-items: center;
    background: rgba(200,200,200,.75);
    display: none;
    height: 100%;
    justify-content: center;
    left: 0;
    position: absolute;
    top: 0;
    width: 100%;
    font-weight: 600;
  }
    .gallery-item:hover .gallery-title {
      display: flex;
    }

.gallery-item:nth-child(15n+2),
.gallery-item:nth-child(15n+3),
.gallery-item:nth-child(15n+5),
.gallery-item:nth-child(15n+7),
.gallery-item:nth-child(15n+12) {
  grid-row: span 2;
}

.gallery-item:nth-child(15n+4),
.gallery-item:nth-child(15n+9) {
  grid-column: span 2;
  grid-row: span 2;
}

@media (max-width: 900px) {
  .gallery {    
    grid-auto-rows: 35vh;
  }
}

@media (max-width: 550px) {
  .gallery-item:nth-child(15n+4),
  .gallery-item:nth-child(15n+9) {
    grid-column: auto;
  }
}
</style>