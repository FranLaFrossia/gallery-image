<template>
  <div class="gallery-modal">
    <div
      class="background"
      v-if="visible"
      @click="hide"
    >
      <div
        class="cross"
        @click.stop="hide"
      >&times;</div>
      <div class="modal-wrapper">
        <div
          class="arrow --left"
          @click.stop="prev"
          :class="{'invisible': ! hasPrev()}"
        >
          <svg
            fill="#fff"
            height="48"
            viewBox="0 0 24 24"
            width="48"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path d="M15.41 16.09l-4.58-4.59 4.58-4.59L14 5.5l-6 6 6 6z" />
            <path d="M0-.5h24v24H0z" fill="none" />
          </svg>
        </div>
        <div class="image-wrapper" @click.stop>
          <img :src="images[index].urls.regular" />
          <p v-if="images[index].description" class="image-title">{{ images[index].description }}</p>
          <p v-else class="image-title">No description</p>
        </div>
        <div
          class="arrow --right"
          @click.stop="next"
          :class="{'invisible': ! hasNext()}"
        >
          <svg
            fill="#fff"
            height="48"
            viewBox="0 0 24 24"
            width="48"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path d="M8.59 16.34l4.58-4.59-4.58-4.59L10 5.75l6 6-6 6z" />
            <path d="M0-.25h24v24H0z" fill="none" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GalleryModal',
  props: {
    images: {
      type: Array,
      default: () => [],
    }
  },
  data() {
    return {
      visible: false,
      index: 0,
    };
  },
  methods: {
    // Show modal
    show() {
      this.visible = true;
    },
    // Hied modal
    hide() {
      this.visible = false;
    },
    // Evaluates if there's more images in the array
    hasNext() {
      return this.index + 1 < this.images.length;
    },
    hasPrev() {
      return this.index - 1 >= 0;
    },
    // Show previous image on array
    prev() {
      if (this.hasPrev()) {
        this.index -= 1;
      }
    },
    // Show next image on array
    next() {
      if (this.hasNext()) {
        this.index += 1;
      }
    },
    // Functionality to show, change or hide modal with keyboards
    onKeydown(e) {
      if (this.visible) {
        switch (e.key) {
          case 'ArrowRight':
            this.next();
            break;
          case 'ArrowLeft':
            this.prev();
            break;
          case 'ArrowDown':
          case 'ArrowUp':
          case ' ':
            e.preventDefault();
            break;
          case 'Escape':
            this.hide();
            break;
        }
      }
    },
    setIndex(i) {
      this.index = i;
    }
  },
  mounted() {
    window.addEventListener('keydown', this.onKeydown)
  },
  destroyed() {
    window.removeEventListener('keydown', this.onKeydown)
  },
}
</script>

<style scoped>
.background {
  background: rgba(0, 0, 0, 0.8);
  position: fixed;
  top: 0;
  left: 0;
  z-index: 3;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}

.cross {
  position: fixed;
  top: 0;
  right: 0;
  color: white;
  cursor: pointer;
  font-size: 1.5rem;
  padding: 1rem;
  margin-right: 2rem;
}
.modal-wrapper {
  display: flex;
}

.arrow {
  cursor: pointer;
  align-self: center;
  padding: 2rem; 
}
  .arrow svg {
    pointer-events: none;
  }

.image-wrapper {
  text-align: center;
}

  .image-wrapper img {
    width: auto;
    height: auto;
    max-width: 100%;
    max-height: calc(90vh - 90px);
  }

  .image-title {
    color: #fff;
    text-align: center;
    margin-top: 2rem;
    font-size: 1.5rem;  
  }

.invisible {
  visibility: hidden;
}
</style>