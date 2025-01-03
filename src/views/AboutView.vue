<template>
    <div>About View</div>
    <div>
        <!-- 'button' 슬롯을 포함하여 Test 컴포넌트 사용 -->
        <Test :title="test.title">
            <template #button>
                <span>내용 1</span>
            </template>
        </Test>

        <!-- 'button' 슬롯 없이 Test 컴포넌트 사용 -->
        <Test title="제목 2" />
        <Test :title="title1.title" />

        <div v-for="post in posts" key="post.id">
            <Test :title2="post.title" :content2="post.content"></Test>
        </div>
    </div>
</template>

<script>
    import { reactive, ref } from 'vue';
    import Test from '../components/Test.vue';

    export default {
        components: {
            Test,
        },
        setup() {
            const title1 = ref({ title: '동적인 제목1' });
            const test = reactive({
                title: '제목222222',
            });
            const posts = reactive([
                { id: 1, title: '제목1', content: '내용1' },
                { id: 2, title: '제목2', content: '내용2' },
                { id: 3, title: '제목3', content: '내용3' },
                { id: 4, title: '제목4', content: '내용4' },
                { id: 5, title: '제목5', content: '내용5' },
            ]);
            return { test, title1, posts };
        },
    };
</script>

<style lang="scss" scoped></style>
