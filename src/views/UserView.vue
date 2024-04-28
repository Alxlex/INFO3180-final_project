<template>
    <h1 v-if="error != null" class="alert alert-danger" role="alert">{{ error }}</h1>
    <div v-if="user != null" class="container">
        <div class="profile-top">
            <img :src="user['profile_photo']" alt="profile Profile Photo" class="card-img-left">
            <p class="card-text">{{ user['firstname'] +" "+ user['lastname']}}</p>
            <p class="card-text">{{ user['location'] }}</p>
            <p class="card-text">{{ user['joined_on'] }}</p>
            <p class="card-text">{{ user['biography'] }}</p>
            <p class="card-text">{{ user['postcount'] }} Posts</p>
            <p class="card-text">{{ followers }} Followers</p>
            <button v-if="followed" @click="toggleFollow" class="btn btn-success">Following</button>
            <button v-else @click="toggleFollow" class="btn btn-primary">Follow</button>
        </div>
        <div v-if="posts != null" class="card-deck">
            <div v-for="post in posts" class="card">
                <img :src="post['photo']" alt="Post Image" class="card-img-top">
            </div>
        </div>
        <h1 v-else class="alert alert-danger" role="alert">{{ message }}</h1>
    </div>
</template>

<script setup>
    import { ref, onMounted } from "vue";
    import { useRoute } from "vue-router";

    const route = useRoute()
    const followers = ref(null)
    const followed = ref(null)
    const token = ref(null)
    const posts = ref(null)
    const error = ref(null)
    const user = ref(null)
    const message = ref(null)

    onMounted(() => {
        token.value = localStorage.getItem("token")
        getUser();
    });

    function toggleFollow(){
        fetch(`/api/v1/users/${route.params.user_id}/follow`, {
            // method: 'POST',
            headers: {
                'Authorization': `Bearer ${token.value}`
            }
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            followed.value = data.followed
            followers.value = data.followers
        })
        .then(function (error) {
            console.log(error)
        });
    }

    function getUser() {
        fetch(`/api/v1/users/${route.params.user_id}/posts`, {
            method: 'GET',
            headers: {
                'Authorization': `Bearer ${token.value}`
            }
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            // console.log()
            posts.value = data.posts
            user.value  = data.user
            followers.value = data.followers
            followed.value = data.followed
            error.value = data.error
            message.value = data.message

            console.log(message.value,posts.value)
        })
        .then(function (error) {
            console.log(error)
        });
    }
</script>