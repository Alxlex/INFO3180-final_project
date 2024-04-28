<template>
  <header>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">
      <div class="container-fluid">
        <a class="navbar-brand" href="/"><img src="/src/assets/photogram_logo.png" alt="Photogram Camera Logo">Photogram</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto">
            <li class="nav-item">
              <RouterLink to="/" class="nav-link active">Home</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/explore">Explore</RouterLink>
            </li>
            <li v-if="token == null" class="nav-item">
              <RouterLink class="nav-link" to="/login">Login</RouterLink>
            </li>
            <li v-else class="nav-item">
              <RouterLink class="nav-link" to="/logout" @click="logout">Logout</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/register">Register</RouterLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup>
  import { onMounted, ref } from "vue";
  import { RouterLink } from "vue-router";

  const token = ref(null)
  const csrf_token = ref(null)

  onMounted(() =>{
    getCsrf()
    token.value = localStorage.getItem('token')
  })

  function logout(){
    fetch('/api/v1/auth/logout', {
        method: 'POST',
        headers: {
          'X-CSRFToken': csrf_token.value
        }
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            if(data != null){
              localStorage.removeItem('token')
              token.value = localStorage.getItem('token')
            }
        })
        .then(function (error) {
            console.log(error)
    });
  }

  function getCsrf(){
        fetch('/api/v1/csrf-token')
        .then((response) => response.json())
        .then((data) => {
            console.log(data)
            csrf_token.value = data.csrf_token;
        })
    }
</script>

<style>
/* Add any component specific styles here */
  div#navbarSupportedContent {
    flex-grow: unset;
  }

  a.navbar-brand img {
    height: 20px;
    width: 20px;
  }
</style>