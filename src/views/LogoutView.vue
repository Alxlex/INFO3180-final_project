<template>
    <h1>Logout Successful</h1>
</template>

<script setup>
    localStorage.removeItem('token')
</script>