<template>
    <div class="add-form">
        <div>
            <label>Название задачи</label>
            <input v-model="newTitle">
        </div>
        <div>
            <label>Описание задачи</label>
            <textarea v-model="newDescr"></textarea>
        </div>
        <div v-if="errorText.length" class="error-common">{{ errorText }}</div>
        <button @click="addTodo">Добавить задачу</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTitle: '',
            newDescr: '',
            errorText: ''
        }
    },
    methods: {
        addTodo() {
            if (this.newTitle != '' && this.newDescr != '') {
                this.$emit('add-todo', { title: this.newTitle, descr: this.newDescr, done: false, isEditMode: false });
                this.newTitle = '';
                this.newDescr = '';
                this.errorText = '';
            } else {
                this.errorText = 'Название и описание задачи не могут быть пустыми.';
            }
        }
    },
    watch: {
        newTitle: function () {
            this.errorText = '';
        },
        newDescr: function () {
            this.errorText = '';
        }
    }
}
</script>

<style lang="scss">
@mixin box-sizing($property) {
    -webkit-box-sizing: $property;
    -ms-box-sizing: $property;
    box-sizing: $property;
}
.add-form {
    @include box-sizing(border-box);
    margin-left: auto;
    flex: 0 0 30%;
    padding-left: 5px;
    padding-right: 5px;

    label {
        display: block;
    }
}
@media all and (max-width: 768px) {
    .add-form {
        flex-basis: 40%;
    }
}
@media all and (max-width: 567px) {
    .add-form {
        flex-basis: 100%;
        order: 0;
        margin-bottom: 5px;
    }
}
</style>