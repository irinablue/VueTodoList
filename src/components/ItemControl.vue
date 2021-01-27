<template>
    <div 
        class="edit-control"
        :class="[classObject, 't-' + type]"
        :title="title"
        @click="doAction"
    />
</template>

<script>
export default {
    props: {
        index: {
            type: Number,
            default: 0,
            required: true
        },
        type: {
            type: String,
            default: '',
            required: true
        },
        title: {
            type: String,
            default: ''
        },
        isDone: {
            type: Boolean,
            default: false,
            required: true
        }
    },
    computed: {
        classObject: function () {
            return {
                'is-check': this.isDone && this.type === 'done',
            };
        }
    },
    methods: {
        doAction() {
            this.$emit('do-' + this.type + '-action', this.index);
        }
    }
};
</script>

<style lang="scss" scoped>
    .edit-control {
        flex-basis: 36px;
        width: 36px;
        height: 36px;
        border: 3px transparent solid;
        cursor: pointer;
        filter: grayscale(1);
        transition: all 0.3s ease-out;
        background-size: cover;
        background-repeat: no-repeat;
        background-position: 50%;

        &.t-edit {
            background-image: url("../assets/images/pencil.svg");

            &:hover {
                filter: grayscale(0);
            }
        }
        &.t-delete {
            background-image: url("../assets/images/remove.svg");
            
            &:hover {
                filter: grayscale(0);
            }
        }
        &.t-done {
            background-image: url("../assets/images/check.svg");

            &.is-check {
                filter: grayscale(0);
            }
        }
    }
</style>