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
                            <div @click="toggleLike(post['id'])" class="png-container" ref="hearts">
                                <img src="/src/assets/like.png" alt="Like heart picture">
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
    import { jwtDecode } from "jwt-decode";
    import { ref, onMounted, isProxy, toRaw } from "vue";

    const likes = ref([]);
    const hearts = ref([]);
    const user_id = ref(null)
    const token = localStorage.getItem("token")
    const posts = ref(null);

    onMounted(() => {
        getPosts();
        getUserId();
    });

    function toggleLike(like){
        // like = !like
        // console.log(like)
        // if (like){
        //     document.getElementById("like")
        //     .style
        //     .filter = "drop-shadow(0px 1000px 0 red)"

        //     document.getElementById("like")
        //     .style
        //     .transform = "translateY(-1000px)"
        // }else{
        //     document.getElementById("like")
        //     .style
        //     .filter = null

        //     document.getElementById("like")
        //     .style
        //     .transform = null
        // }
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
            // console.log(hearts.value)
            // const img = hearts.value
            // console.log(img)

            // for(let val of ){
            //     console.log(val)
            // }
            // for(let i = 0; i < likes.value.length; i++){
                
            //     console.log(document.getElementById('post'+i))
                
            //     if(likes.value[i] == false){

            //         heart.style.filter = "drop-shadow(0px 1000px 0 red)"

            //         heart.style.transform = "translateY(-1000px)"
            //     }
            // }
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