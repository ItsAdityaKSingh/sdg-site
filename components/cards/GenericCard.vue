<template>
  <div class="card">
    <div class="card-image">
      <component
        :is="link ? 'nuxt-link' : 'span'"
        :to="{ path: link, query: { subBlogs: subBlogs } }"
      >
        <figure :class="`image is-16x9`">
          <opti-image
            v-if="image"
            class="image_flex"
            :src="`${image}`"
            :srcset="responsiveImage.srcSet"
            :width="imageRatio[0]"
            :height="imageRatio[1]"
            :sizes="`(min-width: 768px) ${100 / 4}vw`"
          />
        </figure>
      </component>
    </div>
    <div class="card-content">
      <nuxt-link :to="link">
        <h2 :class="`${title ? '' : 'empty-content-placeholder'}`">
          {{ title }}
        </h2>
        <h4
          :class="{
            subtitle: true,
            'is-6': true,
            'empty-content-placeholder': !$slots.default,
          }"
        >
          <slot></slot>
        </h4>
      </nuxt-link>
    </div>
  </div>
</template>
<script>
import { OptiImage } from 'opti-image'
import { defineComponent } from '@nuxtjs/composition-api'
export default defineComponent({
  components: {
    OptiImage
  },
  props: {
    title: { type: String, default: '' },
    image: { type: String, default: '' },
    link: { type: String, default: '' },
    imageDimensions: { type: String, default: '16x9' }
  },
  data () {
    return { screenWidth: 2000 }
  },
  computed: {
    subBlogs () {
      return []
    },
    imageRatio () {
      return [this.screenWidth, (9 / 16) * this.screenWidth]
    },
    responsiveImage () {
      return { src: this.image, srcSet: '' }
    }
  },
  mounted () {
    this.screenWidth = (window.screen.width) * 0.90
  }
})
</script>
<style scoped lang="scss">
.image_flex {
  @media only screen and (max-width: 640px) {
    max-height: 30vh !important;
  }
}

.subtitle {
  opacity: 0.5;
  font-size: 0.8rem;
}
.empty-content-placeholder {
  background: transparent;
  color: transparent;
  position: relative;
  height: 1em;
  &:before {
    transform-origin: left;
    content: '';
    background: #eee;
    width: 100%;
    position: absolute;
    top: 0;
    bottom: 0;
    animation: fillHorizontal 2s linear infinite;
  }
  &.subtitle {
    width: 70%;
  }
}
.card-content {
  padding: 1rem;
}
</style>
<style lang="scss">
.opti-image-loaded + .spinner-wrapper {
  display: none;
}
.card img {
  transition: 0.8s ease-in-out all;
  &:hover {
    transform: scale(1.02);
  }
}
.card-content {
  color: white;
}

.opti-image-before-load {
  display: none;
}
</style>
