<template>
  <header
    class="header fixed-header"
    :class="{
      'hidden': isHidden,
      'searchbar-active': hasSearchBar,
      'historybar-active': hasHistoryBar,
    }"
  >
    <div class="container">
      <div class="header--top">
        <nuxt-link to="/">
          <div class="logo">
            <img src="@/static/unsplash.svg" class="logo--img">
            <div class="logo--text">
              ImageStock
            </div>
          </div>
        </nuxt-link>
        <div class="actions">
          <div
            class="action--search action"
            @click="toggleSearchBar"
          >
            <img src="@/static/search.svg" class="action--icon icon-search">
            <div class="action--text actions--faves--text">
              Поиск
            </div>
          </div>
          <nuxt-link class="action--faves action" to="/favorites">
            <img src="@/static/faves.svg" class="action--icon icon-fave">
            <div class="action--text actions--faves--text">
              Избранное
            </div>
          </nuxt-link>
          <div
            class="action--search-history action"
            @click="toggleHistoryBar"
          >
            <img src="@/static/history.svg" class="action--icon icon-history">
            <div class="action--text actions--search-history--text">
              История поиска
            </div>
          </div>
        </div>
      </div>
      <div class="search">
        <input
          id="image-searchbar-fixed"
          v-model="searchInput"
          type="text"
          name="image-search"
          class="search--searchbar"
          placeholder="Поиск"
          autocomplete="off"
          @keydown="search($event)"
        >
        <div class="searchbar--border" />
        <div class="search--suggestions">
          <nuxt-link
            v-for="sugg in searchSuggestions"
            :key="sugg"
            :to="`/search/${encodeURIComponent(sugg)}`"
            class="suggestion"
          >
            {{ sugg }}
          </nuxt-link>
        </div>
      </div>
      <div class="history">
        <h1 class="history--heading">
          Ваши последние запросы
        </h1>
        <span v-if="!searchHistory.length" class="nohistory">
          Вы еще ничего не искали
        </span>
        <div v-else class="history--queries">
          <nuxt-link
            v-for="searchQuery in searchHistory"
            :key="searchQuery"
            :to="`/search/${encodeURIComponent(searchQuery)}`"
            class="history--query"
          >
            {{ searchQuery }}
          </nuxt-link>
        </div>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  props: {
    isAfterCompact: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      isHidden: true,
      hasSearchBar: false,
      hasHistoryBar: false,
      searchSuggestions: [
        'Wallpapers',
        'Textures & Patterns',
        'Nature',
        'Current Events',
        'Architecture',
        'Business & Work',
        'Film',
        'Animals',
        'Travel',
        'Fashion',
        'Food & Drink',
        'Spirituality',
        'Experimental',
        'People',
        'Health',
        'Arts & Culture'
      ],
      searchInput: '',
      searchHistory: []
    }
  },
  beforeMount () {
    window.addEventListener('scroll', this.scrollHandler)
  },
  beforeDestroy () {
    window.removeEventListener('scroll', this.scrollHandler)
  },
  mounted () {
    this.searchHistory = window.localStorage.searchHistory ? window.localStorage.searchHistory.split(',').reverse() : []
  },
  methods: {
    scrollHandler () {
      if (window.scrollY >= (this.isAfterCompact ? 80 : 350)) {
        this.isHidden = false
      } else {
        this.isHidden = true
        this.hasSearchBar = false
        this.hasHistoryBar = false
      }
    },
    toggleSearchBar () {
      if (this.hasHistoryBar) {
        this.hasHistoryBar = false
      }
      this.hasSearchBar = !this.hasSearchBar
    },
    toggleHistoryBar () {
      if (this.hasSearchBar) {
        this.hasSearchBar = false
      }
      this.hasHistoryBar = !this.hasHistoryBar
    },
    search (event: any) {
      if (event.key !== 'Enter') {
        return
      }
      const searchHistoryString = window.localStorage.searchHistory ? window.localStorage.searchHistory : ''
      const searchHistoryArray = searchHistoryString.length ? searchHistoryString.split(',').reverse() : []
      if (searchHistoryArray.length > 15) {
        searchHistoryArray.pop()
        searchHistoryArray.reverse().push(event.target.value)
        window.localStorage.setItem('searchHistory', searchHistoryArray)
        this.searchHistory = window.localStorage.searchHistory
      } else {
        searchHistoryArray.reverse().push(event.target.value)
        window.localStorage.setItem('searchHistory', searchHistoryArray)
        this.searchHistory = window.localStorage.searchHistory
      }
      window.open(`/search/${encodeURIComponent(event.target.value)}`, '_self')
    }
  }
})
</script>

<style lang="scss" scoped>

@media only screen and (max-width: 768px) {
  .header .header--top {
    .logo--text {
      display: none;
    }

    .action--text {
      display: none;
    }
  }
}

header.header {
  display: block;
  width: 100%;
  margin: 0;
  background: #000;
  color: #fff;
  min-height: 80px;
  padding-top: 25px;

  .header--top {
    display: flex;
    justify-content: space-between;
    padding: 0 20px;
  }
}

header.header.fixed-header {
  position: fixed;
  top: 0;
  z-index: 1000;
  transition: top .3s linear;
}

header.header.fixed-header.hidden {
  top: -100px;
}

a {
  color: #fff;
  text-decoration: none;
}

.logo {
  display: flex;
}

.logo--img {
  display: block;
  height: 28px;
  width: 28px;
}

.logo--text {
  font-weight: 600;
  font-size: 24px;
  margin-left: 24px;
  line-height: 28px;
}

.actions {
  display: flex;
}

.action {
  display: flex;
  margin-left: 24px;
  cursor: pointer;
}

.action--icon {
  width: 26px;
  height: 26px;
  margin-right: 10px;
}

.action--text {
  line-height: 26px;
}

.search--searchbar {
  display: block;
  margin: 48px auto 0;
  width: 80%;
  height: 100px;
  background: none;
  border: none;
  outline: none;
  font-size: 72px;
  text-align: center;
  caret-color: #fff;
  color: #fff;

  &::placeholder {
    color: #fff;
    transition: color 0.2s linear;
  }

  &:focus::placeholder {
    color: #777;
  }
}

.search {
  display: none;
}

header.header.fixed-header.searchbar-active .search{
  display: block;
}

.searchbar--border {
  height: 0.5px;
  width: 80%;
  background: linear-gradient(
    90deg,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.5) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  margin: 0 auto;
}

.search--suggestions {
  display: block;
  margin: 30px auto 0;
  height: 21px;
  text-align: center;
  overflow: hidden;
  background: linear-gradient(to right, #fff, #fff, #ffffff00);
  background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 90px;
}

.suggestion {
  margin-right: 24px;
  font-size: 18px;
  line-height: 21px;
}

.history {
  display: none;
}

header.header.fixed-header.historybar-active .history{
  display: block;
}

.history {
  text-align: center;
}

.history--heading {
  font-size: 36px;
  font-weight: 500;
  margin-bottom: 48px;
  margin-top: 60px;
}

.history--queries {
  margin-bottom: 130px;
}

.history--query {
  margin-right: 24px;
  font-size: 18px;
  line-height: 21px;

  &:last-child {
    margin-right: 0;
  }
}

.nohistory {
  display: block;
  margin-bottom: 40px;
}

</style>
