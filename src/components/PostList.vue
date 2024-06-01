<template>
  <div class="background">
    <div class="container">
      <h1>MASUKKAN NAMA</h1>
      <form @submit.prevent="createPost" class="form">
        <input
          v-model="newPost.title"
          placeholder="Enter title"
          required
          class="input"
        />
        <button type="submit" class="button">TAMBAHKAN</button>
      </form>
      <ul class="post-list">
        <li v-for="post in posts" :key="post.id" class="post-item">
          <input v-model="post.title" @blur="updatePost(post)" class="input" />
          <button @click="deletePost(post.id)" class="button delete">
            HAPUS
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ref, onMounted } from "vue";

// Replace with your BIN ID and API Key
const BIN_ID = "6659588bad19ca34f871f823";
const API_KEY = "$2a$10$maAf3Q3twBEI4YqKypp3Deyl4tO2I1ne1JHDqM.6i0R0dwxjanEO.";
const BASE_URL = `https://api.jsonbin.io/v3/b/${BIN_ID}`;

export default {
  setup() {
    const posts = ref([]);
    const newPost = ref({
      title: "",
    });

    const fetchPosts = async () => {
      try {
        const response = await axios.get(`${BASE_URL}/latest`, {
          headers: {
            "X-Master-Key": API_KEY,
          },
        });
        posts.value = response.data.record.posts || [];
      } catch (error) {
        console.error("Error fetching posts:", error);
      }
    };

    const createPost = async () => {
      try {
        const post = { ...newPost.value, id: Date.now() };
        posts.value.push(post);
        await axios.put(
          BASE_URL,
          { posts: posts.value },
          {
            headers: {
              "Content-Type": "application/json",
              "X-Master-Key": API_KEY,
            },
          }
        );
        newPost.value.title = ""; // Clear the input after successfully adding a post
      } catch (error) {
        console.error("Error creating post:", error);
      }
    };

    const updatePost = async (post) => {
      try {
        await axios.put(
          BASE_URL,
          { posts: posts.value },
          {
            headers: {
              "Content-Type": "application/json",
              "X-Master-Key": API_KEY,
            },
          }
        );
      } catch (error) {
        console.error("Error updating post:", error);
      }
    };

    const deletePost = async (id) => {
      try {
        posts.value = posts.value.filter((post) => post.id !== id);
        await axios.put(
          BASE_URL,
          { posts: posts.value },
          {
            headers: {
              "Content-Type": "application/json",
              "X-Master-Key": API_KEY,
            },
          }
        );
      } catch (error) {
        console.error("Error deleting post:", error);
      }
    };

    onMounted(fetchPosts);

    return {
      posts,
      newPost,
      createPost,
      updatePost,
      deletePost,
    };
  },
};
</script>

<style scoped>
.background {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  overflow: hidden; /* Mencegah scrolling horizontal */
  background-image: url('https://example.com/path-to-your-image.jpg'); /* Ganti dengan link gambar Anda */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.container {
  max-width: 600px;
  width: 100%; /* Ubah dari 500% menjadi 100% */
  padding: 50px;
  text-align: center;
  font-family: Arial, sans-serif;
  background-color: rgba(255, 255, 255, 0.8); /* Add a slight background color for readability */
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Add shadow for depth effect */
  border-radius: 8px; /* Rounded corners for better aesthetics */
  margin: 0 auto; /* Ensure centering with auto margins */
  box-sizing: border-box; /* Include padding and border in the element's total width and height */
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

.form {
  display: flex;
  flex-direction: column; /* Arrange form elements vertically */
  gap: 10px;
  margin-bottom: 20px;
  align-items: center; /* Center form elements horizontally */
  width: 100%; /* Ensure form takes the full width of the container */
  box-sizing: border-box;
}

.input {
  width: 100%;
  max-width: 400px; /* Limit input width */
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box; /* Include padding and border in the input's total width */
}

.button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  transition: background-color 0.3s; /* Smooth transition */
}

.button.delete {
  background-color: #dc3545;
}

.button:hover {
  background-color: #0056b3; /* Darker color on hover */
}

.button.delete:hover {
  background-color: #c82333; /* Darker color on hover */
}

.post-list {
  list-style: none;
  padding: 0;
  width: 100%; /* Ensure list takes the full width of the container */
  box-sizing: border-box;
}

.post-item {
  display: flex;
  gap: 10px;
  align-items: center;
  margin-bottom: 10px;
  width: 100%; /* Ensure post item takes the full width of the container */
  box-sizing: border-box;
}

.post-item .input {
  flex: 1;
  box-sizing: border-box;
}
</style>
