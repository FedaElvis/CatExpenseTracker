<script setup>
import { ref } from 'vue'

const data = ref([])
const breedId = ref('') // To store the user input for the breed ID

const fetchData = () => {
    const apiUrl = `https://api.thecatapi.com/v1/images/search?limit=8&breed_ids=${breedId.value}&api_key=live_4krY9AjzkscXd9nsBWhGILtFv3GgwDss3Ks1ZUDzhw79TfjwjKqxznpGnr0oarQ9`
    
    // Get request
    fetch(apiUrl)
        .then((response) => {
            // Check if the response status is ok
            if (!response.ok) {
                throw new Error('HTTP error, status: ' + response.status)
            }
            // Now parse the actual response
            return response.json()
        })
        .then((responseData) => {
            data.value = responseData
        })
        .catch((error) => {
            console.error('error: ' + error)
        })
}
</script>

<template>
    <h1>Search for your cat breeds</h1>
    <input v-model="breedId" placeholder="Enter Breed ID" />
    <button @click="fetchData">Search</button>
    <div class="grid-container">
        <div v-for="(item, index) in data" :key="index" class="grid-item">
            <img :src="item.url" alt="Cat Image" />
            <p>Breed: {{ item.breeds[0]?.name || 'Unknown' }}</p>
            <p>Description: {{ item.breeds[0]?.description || 'No description available' }}</p>
        </div>
    </div>
</template>

<style scoped>
.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    padding: 20px;
}

.grid-item {
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 10px;
    text-align: center;
}

.grid-item img {
    width: 100%;
    height: auto;
    border-radius: 10px;
}

input{
    border: 1px solid #dedede;
  border-radius: 2px;
  display: block;
  font-size: 16px;
  padding: 20px;
  width: 100%;
}

button{
    padding: 10px;
    border: none;
    border-radius: 6px;
    color: white;
    background: green;
    margin-top: 5px;
}
</style>
