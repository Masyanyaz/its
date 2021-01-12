<template>
  <nuxt-link class="card" :to="`/${item.id}`">
    <article class="card-content">
      <div class="card-image">
        <img :src="item.image" :alt="item.name" />
      </div>
      <div class="card-title">
        <h2>{{ item.name }}</h2>
      </div>
    </article>
  </nuxt-link>
</template>

<script>
export default {
  name: "PreviewCard",
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style scoped>
.card {
  display: block;
  background-color: rgba(0, 0, 0, .7);
  border-radius: 7px;
  overflow: hidden;
  transition: .1s all ease-in;
  text-decoration: none;
  color: #fff;
}

.card:hover {
  box-shadow: 2px 2px 9px rgba(0, 0, 0, .3);
}

.card-content {
  display: flex;
}

.card-image {
  width: 100%;
  flex: 1 1 0;
}

.card-image img {
  width: 100%;
  height: 100%;
}

.card-title {
  flex: 3 1 0;
  position: relative;
  padding: 0.75rem;
  display: flex;
  flex-direction: column;
}

.card-title h2 {
  font-size: 20px;
}

@media (max-width: 480px) {
  .card-content {
    flex-direction: column;
  }

  .card-title {
    padding-top: 5px;
  }
}
</style>
