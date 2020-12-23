<template>
  <div id="app">
    <FixedHeader :is-after-compact="true" />
    <Header :is-expanded="false" />
    <div class="image-block" :style="`background: url(${unsplashImage.urls.regular}) center/cover`">
      <div class="image-block--filter">
        <div class="container">
          <div class="image-block--top">
            <a class="profile" :href="`https://unsplash.com/@${unsplashImage.user.username}`" target="_blank">
              <img class="profile--avatar" :src="unsplashImage.user.profile_image.medium" alt="">
              <div class="profile--info">
                <div class="profile-name">
                  {{ unsplashImage.user.name }}
                </div>
                <div class="username">
                  @{{ unsplashImage.user.username }}
                </div>
              </div>
            </a>
            <div class="actions">
              <div
                class="action--favorite action"
                @click="favoritesAdd(unsplashImage.urls.regular, $route.params.id, unsplashImage.user.name, unsplashImage.user.username, unsplashImage.user.profile_image.medium)"
              >
                <svg width="25" height="25" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path id="icon/action/favorite_24px_2" d="M18.9121 28.7685C17.8354 29.746 16.1779 29.7461 15.1013 28.7544L14.9454 28.6127C7.50795 21.8836 2.64878 17.4777 2.83295 11.981C2.91795 9.57272 4.15045 7.26355 6.14795 5.90355C9.88795 3.35355 14.5063 4.54355 16.9996 7.46188C19.4929 4.54355 24.1113 3.33938 27.8513 5.90355C29.8488 7.26355 31.0813 9.57272 31.1663 11.981C31.3646 17.4777 26.4913 21.8835 19.0538 28.641L18.9121 28.7685Z" fill="#828282" />
                </svg>
              </div>
              <a
                class="action--download action"
                :href="unsplashImage.urls.regular"
                target="_blank"
              >
                <svg width="34" height="34" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path id="icon/file/download_24px_2" fill-rule="evenodd" clip-rule="evenodd" d="M21.25 13.4584H23.5025C24.7633 13.4584 25.3867 14.9884 24.4942 15.8809L17.9917 22.3834C17.4392 22.9359 16.5467 22.9359 15.9942 22.3834L9.49168 15.8809C8.59918 14.9884 9.23668 13.4584 10.4975 13.4584H12.75V6.37504C12.75 5.59587 13.3875 4.95837 14.1667 4.95837H19.8333C20.6125 4.95837 21.25 5.59587 21.25 6.37504V13.4584ZM8.50001 29.0417C7.72084 29.0417 7.08334 28.4042 7.08334 27.6251C7.08334 26.8459 7.72084 26.2084 8.50001 26.2084H25.5C26.2792 26.2084 26.9167 26.8459 26.9167 27.6251C26.9167 28.4042 26.2792 29.0417 25.5 29.0417H8.50001Z" fill="white" />
                </svg>
                <span class="action--label">Download</span>
              </a>
            </div>
          </div>
          <div class="main-photo" :style="`background: url(${unsplashImage.urls.regular}) center/cover`" />
        </div>
      </div>
    </div>
    <div class="container">
      <h1 class="title title--similar">
        Похожие изображения
      </h1>
      <div class="images">
        <div class="column column1">
          <Photo
            v-for="photo in unsplashSearch.results.filter(photoObj => unsplashSearch.results.indexOf(photoObj) % 3 == 0)"
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
            v-for="photo in unsplashSearch.results.filter(photoObj => unsplashSearch.results.indexOf(photoObj) % 3 == 1)"
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
            v-for="photo in unsplashSearch.results.filter(photoObj => unsplashSearch.results.indexOf(photoObj) % 3 == 2)"
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
  async asyncData ({ $axios, route }) {
    const unsplashImage = await $axios.$get(`https://api.unsplash.com/photos/${route.params.id}?client_id=VGOGR8Y1jxdBDnqLjE_I_2t_LN1mQY33M5v7_VRmeS8`)
    const unsplashSearch = await $axios.$get('https://api.unsplash.com/search/photos?client_id=VGOGR8Y1jxdBDnqLjE_I_2t_LN1mQY33M5v7_VRmeS8&per_page=18&query=' +
      unsplashImage.alt_description)
    return { unsplashSearch, unsplashImage }
  },
  data () {
    return {
      isGridLayout: true
    }
  },
  methods: {
    favoritesAdd (imageLink: string, photoId: string, profileName: string, nickname: string, profileAvatarLink: string) {
      const favoritesList: string = window.localStorage.favorites
      const data: string = imageLink.concat(':',
        photoId, ':',
        profileName, ':',
        nickname, ':',
        profileAvatarLink)
      const favoritesArray = favoritesList ? favoritesList.split(',') : []
      if (favoritesArray.includes(data)) {
        favoritesArray.splice(favoritesArray.indexOf(data), 1)
        window.localStorage.setItem('favorites', favoritesArray.toString())
        return
      }
      favoritesArray.push(data)
      window.localStorage.setItem('favorites', favoritesArray.toString())
    }
  }
})
</script>

<style lang="scss" scoped>

.image-block {
  display: block;
  height: 90vh;
  position: relative;
}

.image-block--filter {
  background-color: #00000080;
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}

.image-block--top {
  display: flex;
  margin: 40px auto;
  justify-content: space-between;

  .profile, .actions {
    display: flex;
    text-decoration: none;
  }

}

.profile--avatar {
  width: 55px;
  height: 55px;
  border: 1px solid #fff;
  border-radius: 8px;
  margin-right: 12px;
}

.profile--info {
  align-self: center;
  color: #fff;
}

.profile-name {
  font-size: 30px;
}

.username {
  font-size: 18px;
}

.actions {
  align-self: center;
}

.action {
  cursor: pointer;
  height: 50px;
}

.action--favorite {
  display: flex;
  width: 50px;
  background: #fff;
  justify-content: center;
  border-radius: 8px;
  margin-right: 20px;

  svg {
    align-self: center;
  }

}

.action--download {
  display: flex;
  height: 50px;
  background: #219653;
  justify-content: center;
  border-radius: 8px;
  text-decoration: none;

  svg {
    margin-left: 20px;
    margin-right: 24px;
    align-self: center;
  }

  .action--label {
    margin-right: 40px;
    color: #fff;
    align-self: center;
  }

}

.main-photo {
  display: block;
  border-radius: 8px;
  margin: 0 auto;
  height: 60vh;
  width: 100%;
  margin-bottom: 40px;
}

h1.title {
  margin-top: 80px;
  margin-bottom: 48px;
  font-size: 36px;
  font-weight: 600;
}

.images {
  display: flex;
}

.column {
  width: 32%;
  margin-left: 2%;

  &:first-child {
    margin-left: 0;
  }
}

@media only screen and (max-width: 768px) {

  .action {
    width: 40px;
    height: 40px;

    .action--label {
      display: none;
    }

    svg {
      margin: 0;
    }
  }

  .profile-name {
    font-size: 18px;
  }

  .username {
    font-size: 14px;
  }

  .profile--avatar {
    width: 48px;
    height: 48px;
  }

  .images {
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

  h1.title {
    margin-top: 60px;
    margin-bottom: 24px;
    font-size: 24px;
  }

}

</style>
