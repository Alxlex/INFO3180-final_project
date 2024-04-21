<template>
    <div v-if="msg != null">
        <div v-if="msg['errors']" class="alert alert-danger" role="alert">
            <li v-for="err in msg['errors']"> {{ err }} </li>
        </div>
        <div v-else class="alert alert-success" role="alert">
            <p>{{ msg['message'] }}</p>
        </div>
    </div>
    <h1>New Post</h1>
    <div>
        <form @submit.prevent="submit" method="POST" id="newPostForm" enctype="multipart/form-data">
            <div class="form-group mb-3">
                <label for="photo" class="form-label">Photo</label>
                <input type="file" name="photo" class="form-control">
            </div>
            <div class="form-group mb-3">
                <label for="caption" class="form-label">Caption</label>
                <textarea type="text" class="form-control" name="caption" placeholder="Write a caption..."></textarea>
            </div>
            <button class="btn btn-primary" type="submit">Submit</button>
        </form>
    </div>
</template>

<script setup>
    import { jwtDecode } from "jwt-decode";
    import { ref, onMounted } from "vue";    

    let csrf_token = ref(null)
    let user_id;
    const msg = ref(null)
    const token = localStorage.getItem("token")

    onMounted(() =>{
        getCsrf();
        getUserId();
        console.log(user_id);
    });

    function getCsrf(){
        fetch('/api/v1/csrf-token')
        .then((response) => response.json())
        .then((data) => {
            console.log(data)
            csrf_token.value = data.csrf_token;
        })
    }

    function getUserId(){
        user_id = jwtDecode(token).user_id;
    }

    function submit(){
        let newPostForm = document.getElementById('newPostForm');
        let form_data = new FormData(newPostForm);
        form_data.append("user_id", user_id)

        console.log(form_data)

        fetch(`/api/v1/users/${user_id}/posts`, {
            method: 'POST',
            body: form_data,
            headers: {
                'X-CSRFToken': csrf_token.value,
                'Authorization': `Bearer ${token}`
            }
        })
        .then(function (response) {
            return response.json();
        })
        .then(function (data){
            console.log(data)
            msg.value = data;
        })
        .catch(function (error) {
            console.log(error)
        });
    }
</script>