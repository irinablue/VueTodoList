<template>
    <ul v-if="todos.length" class="todo-list">
        <li 
            v-for="(todo, index) in todos"
            :key="index"
            class="todo-list-item"
        >
            <div class="todo-list-item-container">
                <div v-if="todo.isEditMode">
                    <input v-model="updatedTitle"><br>
                    <textarea v-model="updatedDescr"></textarea><br>
                    <div v-if="errorText.length" class="error-common">{{ errorText }}</div>
                    <button @click="saveTodo(index, updatedTitle, updatedDescr)">Сохранить изменения</button>
                    <button @click="cancelSave(index)">Отмена</button>
                </div>
                <div v-else>
                    <h2>
                        <span 
                            class="material-icons action-element"
                            v-bind:title="[todo.done ? 'Отметить задачу не выполенной' : 'Отметить задачу выполенной']"
                            @click="doneTodo(index, !todo.done)"
                        >{{ todo.done ? 'check_circle' : 'check_circle_outline' }}</span>
                        {{ todo.title }}
                    </h2>
                    <p>{{ todo.descr }}</p>
                    <span 
                        @click="deleteTodo(index)"
                        class="material-icons action-element i-delete"
                        title="Удалить"
                    >delete_forever</span>
                    <span 
                        @click="editTodo(index)"
                        class="material-icons action-element i-edit"
                        title="Редактировать"
                    >edit</span>
                </div>
            </div>
        </li>
    </ul>
    <div v-else class="todo-list-empty">Список задач пуст!</div>
</template>

<script>
export default {
    data() {
        return {
            updatedTitle: '',
            updatedDescr: '',
            errorText: ''
        }
    },
    props: ['todos'],
    methods: {
        deleteTodo(index) {
            this.$emit('delete-todo', index);
        },
        editTodo(index) {
            this.errorText = '';
            this.$emit('edit-todo', index);
        },
        saveTodo(index) {
            if (this.updatedTitle != '' && this.updatedDescr != '') {
                this.$emit('save-todo', index, this.updatedTitle, this.updatedDescr);
                this.updatedTitle = '';
                this.updatedDescr = '';
                this.errorText = '';
            } else {
                this.errorText = 'Название и описание задачи не могут быть пустыми.';
            }
        },
        cancelSave(index) {
            this.errorText = '';
            this.$emit('cancel-save', index);
        },
        doneTodo(index, isDone) {
            this.$emit('done-todo', index, isDone);
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
.material-icons {
    font-family: "Material Icons";
}

.action-element {
    cursor: pointer;
}

.todo-list {
    display: flex;
    flex-wrap: wrap;
    list-style: none;
    margin: 0;
    padding: 0;
    flex: 0 0 70%;
}

.todo-list-item {
    @include box-sizing(border-box);
    flex: 0 0 33.33%;
    padding: 5px;

    h2 {
        margin: 0;
        padding-right: 55px;
        font-size: 130%;
        display: flex;

        .material-icons {
            font-size: 130%;
            margin-right: 5px;
        }
    }

    .i-delete {
        position: absolute;
        top: 5px;
        right: 5px;
    }
    .i-edit {
        position: absolute;
        top: 5px;
        right: 35px;
    }
}
.todo-list-item-container {
    @include box-sizing(border-box);
    position: relative;
    padding: 5px;
    height: 100%;
    border: 1px #ebebeb solid;
    border-radius: 5px;

    input {
        margin-bottom: 5px;
    }

    textarea {
        margin-bottom: 5px;
    }

    button + button {
        margin-left: 5px;
    }
}

@media all and (max-width: 1024px) {
    .todo-list-item {
        flex-basis: 50%;
    }
}
@media all and (max-width: 768px) {
    .todo-list {
        flex-basis: 60%;
    }
    .todo-list-item {
        flex-basis: 100%;
    }
}
@media all and (max-width: 567px) {
    .todo-list {
        flex-basis: 100%;
        order: 1;
    }
}
</style>