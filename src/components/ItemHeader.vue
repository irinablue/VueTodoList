<template>
    <div class="item-header-container">
        <h2 class="item-title">{{ title }}</h2>
        <ItemControl 
            v-for="control in controls"
            :key="control"
            :type="control"
            :index="index"
            :is-done="isDone"
            v-on="$listeners"
        />
    </div>
</template>

<script>
import ItemControl from './ItemControl.vue';

export default {
    components: {
        ItemControl
    },
    props: {
        index: {
            type: Number,
            default: 0,
            required: true
        },
        title: {
            type: String,
            default: '',
            required: true
        },
        isDone: {
            type: Boolean,
            default: false,
            required: true
        }
    },
    data() {
        return {
            controls: [
                'edit',
                'delete',
                'done'
            ]
        };
    },
    computed: {
        controlTitle: function () {
            return this.isDone ? 'Отметить задачу не выполенной' : 'Отметить задачу выполенной';
        }
    }
};
</script>

<style lang="scss" scoped>
.item-header-container {
    display: flex;
    
    .item-title {
        margin: 0;
        padding-top: 5px;
        font-size: 130%;
        display: flex;
        flex-basis: calc(100% - 36px * 3);
    }
}
</style>