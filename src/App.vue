<template>
  <div class="container">
    <form @submit.prevent="save">
      <input type="text" v-model="form.title" placeholder="Title" /><br />
      <textarea v-model="form.content" placeholder="Content"></textarea><br />
      <button type="submit">Save</button>
    </form>
    <ul>
      <li v-for="article in articles" :key="article.id">
        <h3>{{ article.title }}</h3>
        <p>{{ article.content }}</p>
        <button class="edit" @click="edit(article)">Edit</button>
      </li>
    </ul>
    <button @click="load">Load</button>
  </div>
  <footer>
    <p>&copy; 2024 Integrasi Dengan API. All rights reserved.</p>
  </footer>
</template>

<script setup>
import { reactive, ref, onMounted } from 'vue';
import axios from 'axios';

const form = reactive({
  id: null,
  title: '',
  content: '',
});

const articles = ref([]);

async function load() {
  try {
    const response = await axios.get('http://localhost:3000/articles');
    articles.value = response.data;
  } catch (error) {
    console.error('Error loading articles', error);
  }
}

async function save() {
  try {
    if (form.id) {
      await axios.put(`http://localhost:3000/articles/${form.id}`, form);
    } else {
      await axios.post('http://localhost:3000/articles', form);
    }
    form.id = null;
    form.title = '';
    form.content = '';
    load();
  } catch (error) {
    console.error('Error saving article', error);
  }
}

function edit(article) {
  form.id = article.id;
  form.title = article.title;
  form.content = article.content;
}

onMounted(load);
</script>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  background: url('https://img.freepik.com/free-photo/landscape-scenery-with-magenta-nature_23-2150693849.jpg?t=st=1717437045~exp=1717440645~hmac=42e81fc856ad9312f1f64b1651cc31a9d95733c7aec6f4a8e2fb99dcaf77e412&w=360') no-repeat center center;
  background-size: cover;
  color: white;
}

h1 {
  background: url('https://img.freepik.com/free-photo/beautiful-sunset-sea_23-2148210231.jpg?w=1380&t=st=1717440011~exp=1717443611~hmac=7da7c8508a0a435df4ff3f50e3bcde0e7ab39db8b2b0b63b987b14ab53b11784') no-repeat center center;
  background-size: cover;
  color: white;
  padding: 20px;
  border-radius: 10px;
}

input, textarea {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  box-sizing: border-box;
  background: rgba(255, 255, 255, 0.8);
  border: none;
}

button {
  padding: 10px 15px;
  margin: 10px 0;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
}

button.edit {
  background-color: #FFC107;
}

button:hover {
  opacity: 0.8;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  border-bottom: 1px solid #ccc;
  padding: 10px 0;
}

h3 {
  margin: 0;
}

footer {
  text-align: center;
  margin-top: 20px;
  font-size: 0.9em;
  color: #777;
}
</style>
