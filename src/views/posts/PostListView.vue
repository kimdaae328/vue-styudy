<template>
    <div>
        <h2>게시글 목록</h2>
        <hr class="my-4">
        <div class="row g-3">
            <div v-for="post in posts" :key="post.id" class="col-4">
                <PostItem :title="post.title" :content="post.content" :created-at="post.createdAt"></PostItem>
            </div>
        </div>
    </div>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import { getPosts } from '@/api/posts';
import {ref} from 'vue';
const posts = ref([]);

const fetchPosts = () => {
    posts.value = getPosts();
}
fetchPosts();
</script>

<style lang="scss" scoped>

</style>