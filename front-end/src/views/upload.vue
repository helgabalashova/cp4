<template>
    <div class="admin">
        <div class="add">
            <div class="form">
                <input v-model="title" placeholder="Title"><br>
                <p></p>
                <textarea v-model="tags" placeholder="Enter tags" rows="4" cols="50"></textarea>
                <p></p>
                <input type="file" name="photo" @change="fileChanged">
                <button class="button-upload" @click="upload">Upload</button>
            </div>
        </div>
    </div>
</template>
<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.button-upload {
    padding: 8px 8px;
    background-color: #2a5d68;
    border: none;
    color: white;
    font-size: 15px;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}
</style>
<script>
import axios from 'axios';
export default {
  name: 'upload',
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  data() {
    return {
      title: "",
      tags: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          tags: this.tags,
          path: r1.data.path,
          comments: [],
        });
        this.addItem = r2.data;
        this.getItems();
        location.reload();
      } catch (error) {
        //console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        //console.log(error);
      }
    },
  },
}
</script>
