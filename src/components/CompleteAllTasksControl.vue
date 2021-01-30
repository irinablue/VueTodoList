<template>
    <button
        class="btn"
        @click="doAction"
    >
        {{ title }}
    </button>
</template>

<script>
export default {
    props: {
        title: {
            type: String,
            default: '',
            required: true
        }
    },
    methods: {
        doAction() {
            this.$emit('complete-all-tasks');
        }
    }
};
</script>

<style lang="scss" scoped>
.btn {
    margin-top: 10px;
}
</style>