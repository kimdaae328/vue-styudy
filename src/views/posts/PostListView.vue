<template>
    <div>
        <h2>게시글 목록</h2>
        <hr class="my-4" />
        <PostFilter v-model:title="params.title_like" v-model:limit.number="params._limit" />
        <hr class="my-4" />
        <AppGrid :items="posts">
            <template v-slot="{ item }">
                <PostItem
                    :title="item.title"
                    :content="item.content"
                    :created-at="item.createdAt"
                    @click="goPage(item.id)"
                    @modal="openModal(item)"
                ></PostItem>
            </template>
        </AppGrid>

        <AppPagination :current-page="params._page" :page-count="pageCount" @page="handlePageChange" />
        
        <Teleport to="#modal">
            <PostModal v-model="show" :title="modalTitle" :content="modalContent" :created-at="modalCreatedAt" />
        </Teleport>

        <template v-if="posts && posts.length > 0">
            <hr class="my-5" />
            <AppCard>
                <PostDetailView :id="posts[0].id"></PostDetailView>
            </AppCard>
        </template>
    </div>
</template>

<script setup>
    import { getPosts } from '@/api/posts';
    import { computed, ref, watchEffect } from 'vue';
    import { useRouter } from 'vue-router';
    import PostItem from '@/components/posts/PostItem.vue';
    import PostDetailView from '@/views/posts/PostDetailView.vue';
    import PostFilter from '@/components/posts/PostFilter.vue';
    import PostModal from '@/components/posts/PostModal.vue';
    import AppCard from '@/components/AppCard.vue';
    import AppPagination from '@/components/AppPagination.vue';
    import AppGrid from '@/components/AppGrid.vue';

    const router = useRouter();
    const posts = ref([]);
    const params = ref({
        _sort: 'createdAt',
        _order: 'desc',
        _page: 1,
        _limit: 3,
        title_like: '',
    });
    //page-ination
    const totalCount = ref(0);
    const pageCount = computed(() => Math.ceil(totalCount.value / params.value._limit));

    const fetchPosts = async () => {
        try {
            // console.log('요청 파라미터:', params.value);
            const { data, headers } = await getPosts(params.value);
            // console.log('받은 데이터:', data);
            posts.value = data;
            totalCount.value = headers['x-total-count'];
        } catch (error) {
            console.error(error);
        }
    };
    watchEffect(fetchPosts); //콜백함수 안에서 반응형 함수 실행되는거 반영해줌

    // Handle page change with 숫자 타입 보장
    const handlePageChange = page => {
        params.value._page = Number(page);
    };

    const goPage = id => {
        // router.push(`/posts/${id}`);
        router.push({
            name: 'PostDetail',
            params: {
                id,
            },
        });
    };
    const show = ref(false);
    const modalTitle = ref('');
    const modalContent = ref('');
    const modalCreatedAt = ref('');
    const openModal = ({ title, content, createdAt }) => {
        show.value = true;
        modalTitle.value = title;
        modalContent.value = content;
        modalCreatedAt.value = createdAt;
    };
</script>

<style lang="scss" scoped></style>
