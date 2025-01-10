<template>
    <div>Home View</div>
    <p>{{ $route.path }}</p>
    <p>{{ $route.name }}</p>
    <button class="btn btn-primary" @click="goAboutPage">About으로 이동</button>
    <hr class="my-4"/>
    <AppGrid :items="items" v-slot="{item}" colClass="col-6">
        <AppCard>{{item}}</AppCard>
    </AppGrid>
    <hr class="my-4" />
    <h2>{{ $person.name }}</h2>
    <button class="btn btn-primary" @click="person.say">click person</button>
</template>

<script>
export default {
    created(){
        // console.log(this.$person.name);
        // this.$person.say();
    },
}
</script>
<script setup>
import { inject, ref } from 'vue';
import { useRouter } from 'vue-router';
import AppCard from '@/components/app/AppCard.vue';
import AppGrid from '@/components/app/AppGrid.vue';

const router = useRouter();
const goAboutPage = () => {
    router.push('/about');
};

const items = ref(['사과','딸기','포도','바나나']);

const person = inject('person');
console.log('person.name: ', person.name)
</script>

<style lang="scss" scoped></style>
