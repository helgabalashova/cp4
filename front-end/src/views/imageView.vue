<template>
<div class="admin">
    <div class="edit">
      <div class="image-cont" v-if="item">
        <h2>{{item.title}}</h2>
        <p></p>
        <img :src="item.path" />
        <p>Tags: {{item.tags}} </p>
      </div>
      
      <div>
          <h2>Add Comment</h2>
          <textarea v-model="addedComment" rows=6 cols=70></textarea><br>
          <button class="edit-button" @click="editItem()">Add Comment</button>
      </div>
      <div v-if="comments.length > 0">
          <h2>Comments</h2>
          <p v-for="comment in comments" :key="comment">- {{comment}}</p>
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

/* Uploaded images */
h2 {
    text-transform: capitalize;
    font-size: 20px;
}

.image-cont img{
    max-width: 80%;
}

.edit-button {
  padding: 8px 8px;
  background-color: #2a5d68;
  border: none;
  color: white;
  font-size: 15px;
  text-transform: uppercase;
  
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
  name: 'imageView',
  data() {
    return {
      addedComment: "",
      comments: [],
      item: null,
    }
  },
  created() {
      this.getComments();
  },
  watch: {
    // call again the method if the route changes
    '$route': 'getComments'
  },
  methods: {
    async editItem() {
      try {
        await axios.put("/api/items/" + this.$route.params.id, {
          title: this.item.title,
          tags: this.item.tags,
          comments: this.addedComment,
        });
        this.addedComment = "";
        this.getComments();
      } catch (error) {
        //console.log(error);
      }
    },
    async getComments() {
        try {
            let result = await axios.get("/api/items/" + this.$route.params.id);
            this.item = result.data;
            this.comments = result.data.comments;
        } catch (error) {
            //console.log(error);
        }
    },
  },
}
</script>
