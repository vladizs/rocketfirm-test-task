<template>
  <div id="app">
    <FixedHeader :is-after-compact="false" />
    <Header :is-expanded="true" />
    <div class="container">
      <div class="layout-buttons">
        <div
          :class="`button--column-layout button ${!isGridLayout ? 'active' : ''}`"
          @click="isGridLayout = false"
        />
        <div
          :class="`button--grid-layout button ${isGridLayout ? 'active' : ''}`"
          @click="isGridLayout = true"
        />
      </div>
      <div class="images">
        <div
          v-if="isGridLayout"
          class="images--grid-layout"
        >
          <div class="column column1">
            <Photo
              v-for="photo in unsplashMain.filter(photoObj => unsplashMain.indexOf(photoObj) % 3 == 0)"
              :key="photo.id"
              :image-link="decodeURI(photo.urls.small)"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="photo.user.profile_image.medium"
              :profile-name="photo.user.name"
              :nickname="`@${photo.user.username}`"
              :photo-id="photo.id"
            />
          </div>
          <div class="column column2">
            <Photo
              v-for="photo in unsplashMain.filter(photoObj => unsplashMain.indexOf(photoObj) % 3 == 1)"
              :key="photo.id"
              :image-link="decodeURI(photo.urls.small)"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="photo.user.profile_image.medium"
              :profile-name="photo.user.name"
              :nickname="`@${photo.user.username}`"
              :photo-id="photo.id"
            />
          </div>
          <div class="column column3">
            <Photo
              v-for="photo in unsplashMain.filter(photoObj => unsplashMain.indexOf(photoObj) % 3 == 2)"
              :key="photo.id"
              :image-link="decodeURI(photo.urls.small)"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="photo.user.profile_image.medium"
              :profile-name="photo.user.name"
              :nickname="`@${photo.user.username}`"
              :photo-id="photo.id"
            />
          </div>
        </div>
        <div v-else class="images--column-layout">
          <div class="single-column">
            <Photo
              v-for="photo in unsplashMain"
              :key="photo.id"
              :image-link="decodeURI(photo.urls.regular)"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="photo.user.profile_image.medium"
              :profile-name="photo.user.name"
              :nickname="`@${photo.user.username}`"
              :photo-id="photo.id"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Header from '@/components/Header.vue'
import FixedHeader from '@/components/FixedHeader.vue'
import Photo from '@/components/Photo.vue'

export default Vue.extend({
  components: {
    Header,
    FixedHeader,
    Photo
  },
  async asyncData ({ $axios }) {
    const unsplashMain = await $axios.$get('https://api.unsplash.com/photos?client_id=VGOGR8Y1jxdBDnqLjE_I_2t_LN1mQY33M5v7_VRmeS8&per_page=18')
    return { unsplashMain }
  },
  data () {
    return {
      isGridLayout: true
    }
  }
})
</script>

<style scoped lang="scss">
.layout-buttons {
  display: flex;
  justify-content: center;
  margin-top: 80px;
}

.button--column-layout {
  background-image: url('~static/column-layout.svg');
  width: 24px;
  height: 24px;
  margin-right: 20px;

  &.active {
    background-image: url('~static/column-layout-active.svg');
  }

}

.button--grid-layout {
  background-image: url('~static/grid-layout.svg');
  width: 24px;
  height: 24px;

  &.active {
    background-image: url('~static/grid-layout-active.svg');
  }

}

.button {
  display: block;
  cursor: pointer;
}

.images {
  margin-top: 72px;
}

.images--grid-layout {
  display: flex;
}

.column {
  width: 32%;
  margin-left: 2%;

  &:first-child {
    margin-left: 0;
  }
}

.single-column {
  margin: 0 auto;
  width: 55%;
}

@media only screen and (max-width: 768px) {
  .single-column {
    margin: 0 auto;
    width: 80%;
  }

  .images--grid-layout {
    flex-direction: column;
  }

  .column {
    width: 100%;
    margin-left: 0;
    justify-content: center;
  }

  .layout-buttons {
    display: none;
  }
}

</style>
