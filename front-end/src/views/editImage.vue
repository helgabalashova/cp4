<template>
<div class="admin">
    <div class="edit">
      <div class="upload" v-if="findItem">
        <input v-model="findItem.title">
        <p></p>
        <textarea v-model="findItem.tags" rows=3 cols=50></textarea>
        <p></p>
        <img :src="findItem.path" />
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Save</button>
      </div>
    </div>
</div>
</template>
<style scoped>
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}


/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.actions button{
  padding: 8px 8px;
  background-color: #2a5d68;
  border: none;
  color: white;
  font-size: 15px;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-right: 10px;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 350px;
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
  name: 'Edit',
  props: ['picture'],
  computed: {
    findItem() {
        let result = this.picture;
        return result;
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
    }
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
      } catch (error) {
        //console.log(error);
      }
      location.reload();
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          tags: this.findItem.tags,
        });
        return true;
      } catch (error) {
        //console.log(error);
      }
    },
  },
}
</script>
