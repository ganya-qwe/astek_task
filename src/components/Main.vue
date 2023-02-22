<template>
  <div class="main-wrapper">
    <div class="main-wrapper__item" v-for="item in items" :key="item.id">
      <a :href="item.url">
        <img :src="item.thumbnailUrl" alt="image">
      </a>
      <p v-text="item.title" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data () {
    return {
      items: [],
      apiUrl: 'https://jsonplaceholder.typicode.com/albums/1/photos'
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    getList() {
      this.axios.get(this.apiUrl).then((response) => {
        this.items = response.data
      })
    }
  }
}
</script>

<style scoped>
.main-wrapper {
  margin: 0 auto;
  max-width: 1200px;
  display: flex;
  flex-wrap: wrap;
}
.main-wrapper__item {
  padding: 5px;
  width: 150px;
  transition: all 0.5s ease;
}
.main-wrapper__item:hover {
  transform: scale(1.2);
}
.main-wrapper__item a {
  display: flex;
}
</style>
