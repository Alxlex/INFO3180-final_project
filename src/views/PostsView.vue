<template>
    <button @click="$router.push('/posts/new')" type="button" class="btn btn-primary">New Post</button>
    <h1 v-if="msg != null" class="alert alert-success" role="alert">
        {{ msg }}
    </h1>
    <div v-if="posts != null">
        <div v-if="posts['error']" class="alert alert-danger" role="alert">
            <li> {{ posts['error'] }} </li>
        </div>
        <div v-else>
            <div class="cards">
                <div v-for="post in posts['posts']" class="card-columns">
                    <div class="card" style="width: 50rem;">
                        <a :href="/users/ + post['user_id'] ">
                            <div class="card-header">
                                <p>{{ post['username'] }}</p>
                                <img :src="post['profilePhoto']" alt="Poster Profile Photo">
                            </div>
                        </a>
                        <div class="card-body">
                            <img :src="post['photo']" alt="Photo used in photo">
                            <p class="card-text">{{ post['caption'] }}</p>
                        </div>
                        <div class="card-footer clearfix">
                            {{ likes[post['id']-1] }}
                            <div v-if="likes[post['id']-1][0]" @click="toggleLike(post['id'])" class="png-container">
                                <img src="/src/assets/like.png" alt="Like heart picture" class="red">
                            </div>
                            <div v-else @click="toggleLike(post['id'])" class="png-container">
                                <img src="/src/assets/like.png" alt="Like heart picture">
                            </div>
                            {{ post['likes'] }}
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
    import { jwtDecode } from "jwt-decode";
    import { ref, onMounted, isProxy, toRaw } from "vue";

    const likes = ref([]);
    const user_id = ref(null)
    const token = localStorage.getItem("token")
    const posts = ref(null);
    const msg = ref(null)

    onMounted(() => {
        getPosts();
        getUserId();
    });

    function toggleLike(id){
        console.log(likes.value[id-1][0])
        fetch(`/api/v1/posts/${id}/like`, {
            // method: 'POST',
            headers: {
                'Authorization': `Bearer ${token}`
            }
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            msg.value = data.message
            likes.value[id-1][0] = data.liked
            posts.value['posts'][id-1]['likes'] = data.likes
            console.log(posts.value['posts'][id-1])
        })
        .then(function (error) {
            console.log(error)
        });
    }

    function getUserId(){
        user_id.value = jwtDecode(token).user_id;
    }

    function getPosts(){
        fetch('/api/v1/posts', {
            method: 'GET',
            headers: {
                'Authorization': `Bearer ${token}`
            }
        })
        .then(function (response) {
            return response.json()
        })
        .then(function (data) {
            console.log(data)
            for(let arr in data['posts']){
                likes.value.push([
                    data['posts'][arr]['liked'],
                    data['posts'][arr]['id']])
            }
            posts.value = data;
            console.log()
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
    .red{
        filter: drop-shadow(0px 1000px 0 red);
        transform: translateY(-1000px);
    }
</style>