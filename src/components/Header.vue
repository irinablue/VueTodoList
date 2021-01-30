<template>
    <h1 class="site-title">{{ siteName }}</h1>
</template>

<script>
export default {
    props: {
        siteName: {
            type: String,
            default: '',
            required: true
        }
    }
};
</script>

<style scoped lang="scss">
.site-title {
    font-size: 170%;
}
</style>