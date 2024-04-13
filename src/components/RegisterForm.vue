<template>
    <div v-if="msg != null">
        <div v-if="msg['errors']" class="alert alert-danger" role="alert">
            <li v-for="err in msg['errors']"> {{ err }} </li>
        </div>
        <div v-else class="alert alert-success" role="alert">
            <p>{{ msg['message'] }}</p>
        </div>
    </div>
    <h1>Register</h1>
    <form @submit.prevent="registerUser" method="POST" id="registerForm">
        <div class="form-group mb-3">
            <label for="" class="form-label">First Name</label>
            <input type="text" class="form-control" placeholder="Please enter your First Name">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Last Name</label>
            <input type="text" class="form-control" placeholder="Please enter your Last Name">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Email Address</label>
            <input type="text" class="form-control" placeholder="Please enter your Email Address">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Username</label>
            <input type="text" class="form-control" placeholder="Please enter your Username">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Password</label>
            <input type="text" class="form-control" placeholder="Please enter your Password">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Location</label>
            <input type="text" class="form-control" placeholder="Please enter your Location">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Biography</label>
            <input type="text" class="form-control" placeholder="Please enter your Biography">
        </div>
        <div class="form-group mb-3">
            <label for="" class="form-label">Profile Photo</label>
            <input type="file" class="form-control">
        </div>
        <button class="btn btn-primary" type="submit">Register</button>
    </form>
</template>

<script setup>
    import { ref, onMounted } from "vue";

    onMounted(() =>{

    });

    const msg = ref(null);

    function registerUser(){
        let registerForm = document.getElementById('registerForm');
        let form_data = new FormData(registerForm);

        fetch('/api/v1/auth/register', {
            method: 'POST',
            body: form_data,
            headers: {
                'X-CSRFToken': csrf_token.value
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