<template>
  <div>
    <nuxt-link to="/" class="back-button">
      <div class="arrow" />
      Back
    </nuxt-link>

    <div class="container" v-if="!loading && !error && item">
      <div class="image">
        <img :src="item.image" :alt="item.name" />
      </div>
      <div class="description">
        <h1 class="title">{{ item.name }}</h1>
        <div class="description-item">
          <h3>Status:</h3> {{ item.status }}
        </div>
        <div class="description-item">
          <h3>Gender:</h3> {{ item.gender }}
        </div>
        <div class="description-item">
          <h3>Location:</h3> {{ item.location.name }}
        </div>
      </div>
    </div>

    <div v-if="!loading && error" class="title">
      {{ error }}
    </div>

    <Loading v-if="loading" />
  </div>
</template>

<script>
import Loading from '~/components/Loading'

export default {
  name: "DetailPage",
  components: { Loading },
  async fetch() {
    try {
      this.loading = true

      const response = await fetch('https://rickandmortyapi.com/api/character/' + this.$route.params.id)

      const data = await response.json()

      if (response.status === 200) {
        this.item = data
      } else {
        this.error = data.error
      }
    } catch (e) {
      console.log(e)
    } finally {
      this.loading = false
    }
  },
  data() {
    return {
      loading: false,
      item: null,
      error: null,
    }
  },
}
</script>

<style scoped>
.container {
  display: flex;
}

.image {
  margin-right: 20px;
  max-width: 300px;
  max-height: 300px;
  width: 100%;
  height: 100%;
}

.image img {
  width: 100%;
  height: 100%;
}

.description-item {
  font-size: 20px;
  margin-bottom: 5px;
}

.description-item h3 {
  display: inline;
}

.title {
  font-size: 36px;
  margin-bottom: 10px;
  line-height: 30px;
}

.back-button {
  display: inline-block;
  text-decoration: none;
  margin: 15px 0;
  position: relative;
  color: #000000;
  font-size: 18px;
}

.back-button:hover {
  text-decoration: underline;
}

.arrow {
  position: absolute;
  top: calc(50% - 3px);
  width: 7px;
  height: 7px;
  border-top: 2px solid;
  border-right: 2px solid;
  left: -5px;
  transform: rotate(-135deg);
  animation: arrow-move .5s infinite alternate-reverse linear;
}

@keyframes arrow-move {
  0% {
    left: -5px;
  }

  100% {
    left: -8px;
  }
}

@media (max-width: 780px) {
  .container {
    flex-direction: column;
    align-items: center;
  }

  .image {
    margin-right: 0;
    margin-bottom: 20px;
  }
}

@media (max-width: 480px) {
  .description-item {
    font-size: 16px;
  }

  .title {
    font-size: 28px;
    -ms-scroll-limit-x-max: 22px;
  }

  .image {
    margin-bottom: 10px;
  }
}
</style>
