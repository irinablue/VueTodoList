<template>
    <div class="count-text">{{ title }}: {{ count }}</div>
</template>

<script>
export default {
    props: {
        count: {
            type: Number,
            default: 0,
            required: true
        },
        title: {
            type: String,
            default: '',
            required: true
        }
    }
};
</script>

<style lang="scss" scoped>
.count-text {
    margin-top: 10px;
}
</style>