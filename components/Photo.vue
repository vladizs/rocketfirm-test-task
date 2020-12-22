<template>
  <div class="image-container" :title="altDescription">
    <img :src="imageLink" class="image" loading="lazy" :alt="altDescription">
    <div class="image-overlay">
      <div class="overlay--elements">
        <a :href="`https://unsplash.com/${nickname}`" class="profile-info" target="_blank">
          <img :src="profileAvatarLink" :alt="nickname" class="avatar">
          <div class="profile-name">
            {{ profileName }}
          </div>
          <div class="nickname">
            {{ nickname }}
          </div>
        </a>
        <div id="actions" class="actions">
          <div
            id="action--favorite"
            class="action--favorite action"
            @mouseenter="onActionHover($event)"
            @mouseleave="onActionOut($event)"
            @click="favoritesAdd()"
          >
            <svg width="34" height="34" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path id="icon/action/favorite_24px_2" d="M18.9121 28.7685C17.8354 29.746 16.1779 29.7461 15.1013 28.7544L14.9454 28.6127C7.50795 21.8836 2.64878 17.4777 2.83295 11.981C2.91795 9.57272 4.15045 7.26355 6.14795 5.90355C9.88795 3.35355 14.5063 4.54355 16.9996 7.46188C19.4929 4.54355 24.1113 3.33938 27.8513 5.90355C29.8488 7.26355 31.0813 9.57272 31.1663 11.981C31.3646 17.4777 26.4913 21.8835 19.0538 28.641L18.9121 28.7685Z" fill="white" />
            </svg>
          </div>
          <nuxt-link
            id="action--maximize"
            class="action--maximize action"
            :to="`/photo/${photoId}`"
            @mouseenter="onActionHover($event)"
            @mouseleave="onActionOut($event)"
          >
            <svg width="34" height="34" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M21.5833 4.5C21.5833 3.67157 22.2736 3 23.125 3H32.375C33.2264 3 33.9167 3.67157 33.9167 4.5V13.5C33.9167 14.3284 33.2264 15 32.375 15C31.5236 15 30.8333 14.3284 30.8333 13.5V6H23.125C22.2736 6 21.5833 5.32843 21.5833 4.5Z" fill="white" />
              <path fill-rule="evenodd" clip-rule="evenodd" d="M4.62499 21C5.47643 21 6.16666 21.6716 6.16666 22.5V30H13.875C14.7264 30 15.4167 30.6716 15.4167 31.5C15.4167 32.3284 14.7264 33 13.875 33H4.62499C3.77356 33 3.08333 32.3284 3.08333 31.5V22.5C3.08333 21.6716 3.77356 21 4.62499 21Z" fill="white" />
              <path fill-rule="evenodd" clip-rule="evenodd" d="M33.4651 3.43934C34.0672 4.02513 34.0672 4.97487 33.4651 5.56066L22.6735 16.0607C22.0714 16.6464 21.0953 16.6464 20.4932 16.0607C19.8912 15.4749 19.8912 14.5251 20.4932 13.9393L31.2849 3.43934C31.8869 2.85355 32.8631 2.85355 33.4651 3.43934Z" fill="white" />
              <path fill-rule="evenodd" clip-rule="evenodd" d="M16.5068 19.9393C17.1088 20.5251 17.1088 21.4749 16.5068 22.0607L5.71512 32.5607C5.11306 33.1464 4.13693 33.1464 3.53487 32.5607C2.93281 31.9749 2.93281 31.0251 3.53487 30.4393L14.3265 19.9393C14.9286 19.3536 15.9047 19.3536 16.5068 19.9393Z" fill="white" />
            </svg>
          </nuxt-link>
          <a
            id="action--download"
            class="action--download action"
            :href="imageLink"
            target="_blank"
            @mouseenter="onActionHover($event)"
            @mouseleave="onActionOut($event)"
          >
            <svg width="34" height="34" viewBox="0 0 34 34" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path id="icon/file/download_24px_2" fill-rule="evenodd" clip-rule="evenodd" d="M21.25 13.4584H23.5025C24.7633 13.4584 25.3867 14.9884 24.4942 15.8809L17.9917 22.3834C17.4392 22.9359 16.5467 22.9359 15.9942 22.3834L9.49168 15.8809C8.59918 14.9884 9.23668 13.4584 10.4975 13.4584H12.75V6.37504C12.75 5.59587 13.3875 4.95837 14.1667 4.95837H19.8333C20.6125 4.95837 21.25 5.59587 21.25 6.37504V13.4584ZM8.50001 29.0417C7.72084 29.0417 7.08334 28.4042 7.08334 27.6251C7.08334 26.8459 7.72084 26.2084 8.50001 26.2084H25.5C26.2792 26.2084 26.9167 26.8459 26.9167 27.6251C26.9167 28.4042 26.2792 29.0417 25.5 29.0417H8.50001Z" fill="white" />
            </svg>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    imageLink: {
      type: String,
      required: true
    },
    altDescription: {
      type: String,
      default: ''
    },
    profileAvatarLink: {
      type: String,
      required: true
    },
    profileName: {
      type: String,
      required: true
    },
    nickname: {
      type: String,
      required: true
    },
    photoId: {
      type: String,
      required: true
    }
  },
  methods: {
    onActionHover (event: any) {
      event.path[0].classList.add('active')
      event.path[1].classList.add('has-active')
    },
    onActionOut (event: any) {
      event.path[0].classList.remove('active')
      event.path[1].classList.remove('has-active')
    },
    favoritesAdd () {
      const favoritesList: string = window.localStorage.favorites
      const data: string = this.imageLink.concat(':', this.photoId, ':', this.profileName, ':', this.nickname, ':', this.profileAvatarLink)
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
img.image {
  width: 100%;
  border-radius: 8px;
  transition: filter .3s linear;
  margin: 0 auto;
}

.image-container {
  position: relative;
  border-radius: 8px;
  margin-bottom: 3%;

  &:hover .image {
    filter: blur(2px);
  }

  &:hover .image-overlay {
    visibility: visible;
    opacity: 1;
  }

}

.image-overlay {
  display: flex;
  opacity: 0;
  width: 100%;
  height: 100%;
  border-radius: 8px;
  justify-content: center;
  position: absolute;
  top: 0;
  left: 0;
  color: #fff;
  text-align: center;
  background: rgba(31, 31, 31, 0.4);
  transition: opacity .3s linear;
}

.overlay--elements {
  align-self: center;
}

.profile-info {
  color: #fff;
  text-decoration: none;
}

.avatar {
  width: 70px;
  height: 70px;
  box-sizing: border-box;
  border-radius: 8px;
  border: 1px solid #fff;
}

.profile-name {
  margin-top: 14px;
  font-size: 30px;
}

.nickname {
  font-size: 18px;
}

.actions {
  display: flex;
  margin-top: 40px;
  fill: #ff0000;
  justify-content: center;
}

.action {
  width: 32px;
  height: 32px;
  margin-left: 40px;
  transition: transform .2s linear;
  cursor: pointer;

  img {
    width: 100%;
  }

  &:first-child {
    margin-left: 0;
  }
}

path {
  transition: all .2s linear;
}

.has-active {

  path {
    fill: #bbb;
  }

  .active path{
    fill: #fff;
  }

  .active {
    transform: scale(1.2);
  }
}

</style>
