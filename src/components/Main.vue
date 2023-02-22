<template>
  <Loader v-if="loader"/>
  <div v-else class="main-wrapper">
    <div v-if="error" v-text="error"></div>
    <div class="main-wrapper__item" v-for="item in items" :key="item.id">
      <a :href="item.url">
        <img :src="item.thumbnailUrl" alt="image">
      </a>
      <p v-text="item.title"/>
    </div>
  </div>
</template>

<script>
import Loader from "@/components/Loader";

export default {
  name: "Main",
  components: {Loader},
  data() {
    return {
      items: [],
      loader: false,
      error: '',
      apiUrl: 'https://jsonplaceholder.typicode.com/albums/1/photos',
      imagesToPreload: []
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    async getList() {
      this.loader = true;
      await this.axios.get(this.apiUrl).then((response) => {
        response.data.forEach(el => {
          if (el.thumbnailUrl) {
            this.imagesToPreload.push(el.thumbnailUrl)
          }
        })
        this.items = response.data
      }).then(() => {
        const images = this.imagesToPreload.map(imageSrc => {
          return new Promise((resolve, reject) => {
            const img = new Image();
            img.src = imageSrc;
            img.onload = resolve;
            img.onerror = reject;
          });
        });
        Promise.all(images).then(() => {
          this.loader = false;
        }).catch(err => {
          this.loader = false;
          throw err
        });
      }).catch(err => {
        this.error = 'Something went wrong'
        this.loader = false;
        throw err
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
  width: 150px;
  height: 150px;
  display: flex;
}
</style>
