<template>
    <div v-if="posts != null">
        <div v-if="posts['error']" class="alert alert-danger" role="alert">
            <li> {{ posts['error'] }} </li>
        </div>
        <div v-else>
            <div v-for="post in posts" class="card-columns">
                <div class="cards">
                    <img :src="post['profilePhoto']" alt="Poster Profile Photo">
                    <p class="card-text">{{ post['poster'] }}</p>
                    <img :src="post['photo']" alt="Photo used in photo">
                    <p class="card-text">{{ post['caption'] }}</p>
                    <p>{{ post['likes'] }}</p>
                    <p>{{ post['created_on'] }}</p>
                </div>
            </div>
        </div>
    </div>
    <button @click="$router.push('/posts/new')" type="button" class="btn btn-primary">New Post</button>
</template>

<script setup>
    import { ref, onMounted } from "vue";

    onMounted(() => {
        getPosts();
    });

    const posts = ref(null);

    function getPosts(){
        fetch('/api/v1/posts', {
            method: 'GET',
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            posts.value = data;
        })
        .then(function (error) {
            console.log(error)
        });
    }
</script>