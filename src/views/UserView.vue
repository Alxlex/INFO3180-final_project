<template>
    <div class="profile-top">
        <img :src="user['profile_photo']" alt="User Profile Photo" class="card-img-left">
        <p class="card-text">{{ user['firstName'] user['lastName']}}</p>
        <p class="card-text">{{ user['location'] }}</p>
        <p class="card-text">{{ user['created_on'] }}</p>
        <p class="card-text">{{ user['biography'] }}</p>
        <p class="card-text">{{ user['posts'] }}</p>
        <p class="card-text">{{ user['followers'] }}</p>
        <button class="btn btn-primary">Follow</button>
    </div>
    <div class="card-deck">
        <div v-for="post in posts" class="card">
            <img :src="post['photo']" alt="Post Image" class="card-img-top">
        </div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from "vue";

    const user = ref(null)

    onMounted(() => {
        getUser();
    });

    function getUser() {
        
    }
</script>