<template>
  <div>
    <div v-if="items.length">
      <img class="logo-img" src="/logo-img.png" alt="logo-img">

      <div class="cards">
        <PreviewCard
          v-for="item in items"
          :key="item.id"
          :item="item"
        />
      </div>

    </div>

    <div v-else class="title">
      No data
    </div>

    <div ref="loading">
      <Loading v-show="loading" />
    </div>
  </div>
</template>

<script>
import PreviewCard from '~/components/PreviewCard'
import Loading from '~/components/Loading'

export default {
  components: { Loading, PreviewCard },
  async fetch() {
    await this.fetchItems()
  },

  async mounted() {
    if (process.client) {
      await this.$fetch()
      window.addEventListener('scroll', this.scrollEvent)

      this.isEndPage() && this.scrollEvent()
    }
  },

  destroyed() {
    window.removeEventListener('scroll', this.scrollEvent)
  },

  data() {
    return {
      loading: false,
      items: [],
      info: null,
    }
  },

  methods: {
    isEndPage() {
      const { top } = this.$refs?.loading?.getBoundingClientRect()
      const { clientHeight } = document.documentElement

      const stock = 50

      return top - stock <= clientHeight && top >= 0
    },

    async scrollEvent() {
      if (this.isEndPage() && !this.loading && this.info?.next) {
        await this.fetchItems()
        this.isEndPage() && await this.scrollEvent()
      }
    },

    async fetchItems() {
      try {
        this.loading = true

        const response = await fetch(this.info?.next || 'https://rickandmortyapi.com/api/character')

        const { info, results } = await response.json()

        this.info = info
        this.items.push(...results)
      } catch (e) {
        console.log(e)
      } finally {
        this.loading = false
      }
    },
  },

}
</script>

<style scoped>
.logo-img {
  display: flex;
  justify-content: center;
  margin: 0 auto 10px;
  width: 100%;
  max-width: 350px;
}

.title {
  text-align: center;
  font-size: 36px;
  margin-bottom: 15px;
}

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  grid-gap: 1em;
}

@media (max-width: 480px) {
  .cards {
    grid-template-columns: repeat(auto-fit, minmax(270px, 1fr));
  }
}
</style>
