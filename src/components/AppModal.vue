<template>
    <div v-if="show">
        <div class="modal-backdrop fade show"></div>
        <!-- <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">Launch demo modal</button> -->
        <div class="modal fade show d-block" tabindex="-1" aria-labelledby="exampleModalLabel">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <slot name="header">
                            <h1 class="modal-title fs-5" id="exampleModalLabel">{{ title }}</h1>
                            <button
                                type="button"
                                class="btn-close"
                                data-bs-dismiss="modal"
                                aria-label="Close"
                                @click="$emit('close')"
                            ></button>
                        </slot>
                    </div>
                    <div class="modal-body">
                        <slot></slot>
                    </div>
                    <div class="modal-footer">
                        <slot name="actions"></slot>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
    defineProps({
        show: Boolean,
        title: String,
    });
    defineEmits(['close']);
</script>

<style lang="scss" scoped></style>
