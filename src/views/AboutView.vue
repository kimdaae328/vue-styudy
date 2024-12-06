<template>
    <div>About View</div>
    <p>{{ $route.path }}</p>
    <p>{{ $route.name }}</p>
    <button class="btn btn-primary" @click="$router.push('/')">Home으로 이동</button>
</template>

<script setup>
    import { useRoute } from 'vue-router';

    const routs = useRoute();
    console.log('route.path', routs.path);
</script>

<style lang="scss" scoped></style>
