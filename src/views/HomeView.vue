<script setup>
import { ref } from "vue";

let message = ref("Share photos of your favourite moments with friends, family and the world.")

</script>

<template>
  <div class="container d-flex justify-content-center">
    <img src="/src/assets/home_image.jpg" alt="Home Page Image">
    <div id="welcome" class="border shadow p-3 mb-5 bg-white rounded ">
      <div class="text-center">
        <div>
          <img src="/src/assets/photogram_logo.png" alt="Phorogram Camera Logo">
          <h1>Photogram</h1>
        </div>
        <hr>
        <p>{{ message }}</p>
        <button @click="$router.push('/register')" type="button" class="btn btn-primary">Register</button>
        <button @click="$router.push('/login')" type="button" class="btn btn-secondary">Login</button>
      </div>
    </div>
  </div>

</template>

<style>
/* Add any component specific styles here */
  div.text-center img {
    height: 20px;
    width: 20px;
  }

  img {
    height: 40%;
    width: 40%;
  }

  div.container {
    height: 60%;
  }
</style>