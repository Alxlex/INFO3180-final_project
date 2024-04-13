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
        <form @prevent.submit="submit" method="POST" id="newPostForm">
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
    import { ref, onMounted } from "vue";

    let csrf_token = null
    const msg = ref(null)

    onMounted(() =>{
        // getUserId();
    });

    // function getUserId(){
        
    // }

    function submit(){
        let newPostForm = document.getElementById('newPostForm');
        let form_data = new FormData(newPostForm);

        fetch('/api/v1/users//posts', {
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