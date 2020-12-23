<template>
  <div id="app">
    <Header />
    <FixedHeader />

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
      <div v-if="favorites && favorites.length" class="images">
        <div
          v-if="isGridLayout"
          class="images--grid-layout"
        >
          <div class="column column1">
            <Photo
              v-for="photo in favorites.filter(photoObj => favorites.indexOf(photoObj) % 3 == 0)"
              :key="photo[1]"
              :image-link="'https://' + photo[0]"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="'https://' + photo[4]"
              :profile-name="photo[2]"
              :nickname="photo[3]"
              :photo-id="photo[1]"
            />
          </div>
          <div class="column column2">
            <Photo
              v-for="photo in favorites.filter(photoObj => favorites.indexOf(photoObj) % 3 == 1)"
              :key="photo[1]"
              :image-link="'https://' + photo[0]"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="'https://' + photo[4]"
              :profile-name="photo[2]"
              :nickname="photo[3]"
              :photo-id="photo[1]"
            />
          </div>
          <div class="column column3">
            <Photo
              v-for="photo in favorites.filter(photoObj => favorites.indexOf(photoObj) % 3 == 2)"
              :key="photo[1]"
              :image-link="'https://' + photo[0]"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="'https://' + photo[4]"
              :profile-name="photo[2]"
              :nickname="photo[3]"
              :photo-id="photo[1]"
            />
          </div>
        </div>
        <div v-else class="images--column-layout">
          <div class="single-column">
            <Photo
              v-for="photo in favorites"
              :key="photo[1]"
              :image-link="'https://' + photo[0]"
              :alt-description="photo.alt_description ? photo.alt_description : ''"
              :profile-avatar-link="'https://' + photo[4]"
              :profile-name="photo[2]"
              :nickname="photo[3]"
              :photo-id="photo[1]"
            />
          </div>
        </div>
      </div>
      <div v-else class="noimages">
        Здесь совсем пусто :c
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
  data () {
    return {
      isGridLayout: true,
      favorites: [] as string[]
    }
  },
  mounted () {
    if (window.localStorage.favorites.length === 0) {
      this.favorites = []
      return
    }
    window.localStorage.favorites.replaceAll(/https:\/\//ig, '').split(',').forEach((arr: any) => {
      this.favorites.push(arr.split(':'))
    })
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

.noimages {
  margin-top: 80px;
  font-size: 36px;
  text-align: center;
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
    justify-content: center;
  }

  .layout-buttons {
    display: none;
  }
}

</style>
