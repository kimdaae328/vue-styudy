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
        <AppAlert :items="alerts" />
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { useRoute, useRouter } from 'vue-router';
    import { getPostById, updatePost } from '@/api/posts';
    import PostForm from '@/components/posts/PostForm.vue';
    import AppAlert from '@/components/app/AppAlert.vue';

    const route = useRoute();
    const router = useRouter();
    const id = route.params.id;

    // 'form' 객체를 정의하고 초기화
    const form = ref({
        title: '',
        content: ''
    });

    const post = ref({});

    // 게시글 데이터를 가져오는 함수
    const fetchPost = async () => {
    const { data } = await getPostById(id);
        post.value = { ...data };
    };

    // 게시글 데이터를 가져옴
    fetchPost();

    // 게시글을 수정하는 함수
    const edit = async () => {
        try {
            await updatePost(id, { ...form.value });
            // router.push({ name: 'PostDetail', params: { id } });
            vAlert('수정이 완료되었습니다!!!', 'success');
        } catch (error){
            console.error(error);
            vAlert(error.message, 'error');
        }
    };

    // 취소 버튼 클릭 시 상세 페이지로 이동하는 함수
    const goDetailPage = () => router.push({ name: 'PostDetail', params: { id } });

    // 알림 메시지를 관리하는 'alerts' 객체
    const alerts = ref([]);

    // 알림을 추가하는 함수
    const vAlert = (message, type = 'success') => {
        alerts.value.push({ type, message });
        setTimeout(() => alerts.value.shift(), 3000);
    }
</script>

<style lang="scss" scoped></style>
