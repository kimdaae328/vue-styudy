<template>
    <div>
        <h2>게시글 수정</h2>
        <hr class="my-4" />
        <PostForm v-model:title="form.title" v-model:content="form.content" @submit.prevent="edit">
            <template #actions>
                <button type="button" class="btn btn-outline-danger" @click="goDetailPage">취소</button>
                <button type="button" class="btn btn-primary" @click="edit">수정</button>
            </template>
        </PostForm>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { useRoute, useRouter } from 'vue-router';
    import { getPostById, updatePost } from '@/api/posts';
    import PostForm from '@/components/posts/PostForm.vue';

    const route = useRoute();
    const router = useRouter();
    const id = route.params.id;

    const form = ref({
        title: null,
        content: null,
    });
    const fetchPost = async () => {
        const { data } = await getPostById(id);
        setForm(data);
    };
    const setForm = ({ title, content }) => {
        form.value.title = title;
        form.value.content = content;
    };
    fetchPost();
    const edit = async () => {
        try {
            await updatePost(id, { ...form.value });
            router.push({ name: 'PostDetail', params: { id } });
        } catch {
            console.error(error);
        }
    };

    const goDetailPage = () => router.push({ name: 'PostDetail', params: { id } });
</script>

<style lang="scss" scoped></style>
