<template>
    <button @click="$router.push('/posts/new')" type="button" class="btn btn-primary">New Post</button>
    <div v-if="posts != null">
        <div v-if="posts['error']" class="alert alert-danger" role="alert">
            <li> {{ posts['error'] }} </li>
        </div>
        <div v-else>
            <div class="cards">
                <div v-for="post in posts['posts']" class="card-columns">
                    <div class="card" style="width: 50rem;">
                        <div class="card-header">
                            <p>{{ post['username'] }}</p>
                            <img :src="post['profilePhoto']" alt="Poster Profile Photo">
                        </div>
                        <div class="card-body">
                            <img :src="post['photo']" alt="Photo used in photo">
                            <p class="card-text">{{ post['caption'] }}</p>
                        </div>
                        <div class="card-footer clearfix">
                            <div @click="toggleLike(post['id'])" class="png-container">
                                <img src="/src/assets/like.png" alt="Like heart picture" id="like">
                            </div>
                            <p class="">{{ post['likes'] }}</p>
                            <p class="">{{ post['created_on'] }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    import { ref, onMounted } from "vue";

    var likes = ref(null);

    onMounted(() => {
        getPosts();
    });

    const posts = ref(null);

    function toggleLike(like){
        like = !like
        console.log(like)
        if (like == true){
            document.getElementById("like")
            .style
            .filter = "drop-shadow(0px 1000px 0 red)"

            document.getElementById("like")
            .style
            .transform = "translateY(-1000px)"
        }else{
            document.getElementById("like")
            .style
            .filter = null

            document.getElementById("like")
            .style
            .transform = null
        }
    }

    function getPosts(){
        fetch('/api/v1/posts', {
            method: 'GET',
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            // likes = Array(data["posts"].length).fill([, false])
            posts.value = data;
        })
        .then(function (error) {
            console.log(error)
        });
    }
</script>

<style>
    .png-container {
        overflow: hidden;
    }
</style>