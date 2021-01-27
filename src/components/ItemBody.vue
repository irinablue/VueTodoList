<template>
    <div class="task-body">{{ descr }}</div>
</template>

<script>
export default {
    props: {
        descr: {
            type: String,
            default: '',
            required: true
        }
    }
};
</script>

<style lang="scss" scoped>
.task-body {
    padding-top: 10px;
    padding-bottom: 10px;
}
</style>