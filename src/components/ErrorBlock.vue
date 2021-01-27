<template>
    <div class="error-common">
        {{ errorText }}
    </div>
</template>

<script>
export default {
    props: {
        errorText: {
            type: String,
            default: ''
        }
    }
};
</script>

<style lang="scss" scoped>
.error-common {
    margin: 5px 0;
    padding: 5px;
    background-color: rgba(255,0,0, 0.1);
}
</style>