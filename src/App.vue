<template>
    <div id="app">
        <Header :site-name="siteName" />
        <TaskListContainer />
    </div>
</template>

<script>
import Header from './components/Header.vue';
import TaskListContainer from './components/TaskListContainer.vue';

export default {
    name: 'App',
    components: {
        Header,
        TaskListContainer
    },
    data() {
        return {
            siteName: 'Список задач'
        };
    }
};
</script>

<style scoped lang="scss">
.app-container-inner {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    word-break: break-all;
}
</style>
