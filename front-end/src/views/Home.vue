<template>
<div class="home">
  <section>
    <form class="form">
      <input v-model="searchText" placeholder="Search for an image"/>
    </form>
  </section>
  <section class="image-gallery">
    <div class="image" v-for="item in pictures" :key="item.id">
      <!-- <img @click="showModal(item)" :src="item.path"/>
      <EditImage v-show="isModalVisible" @close="closeModal" v-bind:currentImage="currentImage"></EditImage>
      <router-link :to="{ name: 'imageView', params: { id: item._id } }">Open Image</router-link> -->
      <router-link :to="{ name: 'imageView', params: { id: item._id } }"><img :src="item.path"/></router-link>
      <div class="edit-button">
      <button @click="showModal(item)">Edit Image</button>
      </div>
      <EditImage v-show="isModalVisible" @close="closeModal" v-bind:currentImage="currentImage"></EditImage>
    </div>
  </section>
</div>
</template>

<script>

// @ is an alias to /src
import axios from 'axios';
import EditImage from './editImageModal.vue';
export default {
  name: 'Home',
  components: {
    EditImage,
  },
  data() {
    return {
      searchText: '',
      items: [],
      picture: [],
      isModalVisible: false,
      currentImage2: null,
    }
  },
  created() {
    return this.getItems();
  },
  
  
  methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        //console.log(error);
      }
    },
    showModal(item) {
      this.isModalVisible = true;
      this.currentImage2 = item;
    },
    closeModal() {
      this.isModalVisible = false;
    }
  },

  computed: {
    pictures() {
      let picture = this.items;
      if (picture.length > 0) {
        picture = picture.filter(pic => pic.tags.toLowerCase().search(this.searchText) >= 0);
      }
      return picture;
    },
    currentImage() {
      let result = this.currentImage2;
      return result;
    }
  },
}
</script>

<style scoped>
.image h2 {
  font-style: italic;
}

.edit-button {
  width: 100%;
}

.edit-button button {
  width: 100%;
  padding: 8px 0px;
  background-color: #2a5d68;
  border: none;
  color: white;
  font-size: 15px;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.form {
  text-align: center;
}

.form input{
  width: 500px;
  max-width: 100%;
  line-height: 25px;
  margin-top: 5px;
  margin-bottom: 20px;
}

/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}

.image-gallery {
  column-gap: 1.5em;
}

.image {
  margin: 0 0 1.5em;
  display: inline-block;
  width: 100%;
}

.image img {
  width: 100%;
}

.edit {
  display: none;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    column-count: 4;
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 3;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    column-count: 2;
  }
}
</style>
