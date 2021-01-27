<template>
    <button
        class="button"
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
.button {
    margin-top: 10px;
}
</style>