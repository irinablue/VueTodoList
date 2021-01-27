<template>
    <div class="control-form-container">
        <form class="control-form">
            <FormField
                v-model="formTitle"
                :element="formElements[0]"
                v-on="$listeners"
            />
            <FormField
                v-model="formDescr"
                :element="formElements[1]"
                v-on="$listeners"
            />
            <ErrorBlock
                v-if="isSimilarError"
                :error-text="similarErrorText"
            />
            <ErrorBlock
                v-if="newTaskSimilarErrorExist"
                :error-text="similarErrorText"
            />
            <div v-if="isEditModeOn">
                <button
                    type="button"
                    @click="saveItem"
                >
                    Сохранить
                </button>
                <button
                    type="button"
                    @click="cancelEdit"
                >
                    Отменить
                </button>
            </div>
            <div v-else>
                <button
                    type="button"
                    @click="addTask"
                >
                    Добавить задачу
                </button>
                <button
                    type="reset"
                    @click="resetForm"
                >
                    Очистить
                </button>
            </div>
        </form>
    </div>
</template>

<script>
import FormField from './FormField.vue';
import ErrorBlock from './ErrorBlock.vue';

export default {
    components: {
        FormField,
        ErrorBlock
    },
    props: {
        data: {
            type: Array,
            default: function () {
                return [
                    '',
                    ''
                ];
            }
        },
        newTaskSimilarErrorExist: {
            type: Boolean,
            default: false
        },
        isEditModeOn: {
            type: Boolean,
            default: false
        },
        isSimilarError: {
            type: Boolean,
            default: false
        },
        newTaskIndex: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            emitEnabled: false,
            formTitle: this.data[0] || '',
            formDescr: this.data[1] || '',
            formElements: [
                { type: 'input-text', label: 'Название задачи', errorText: 'Название задачи не должно быть пустыми.', isErrorExist: false},
                { type: 'textarea', label: 'Описание задачи', errorText: 'Описание задачи не должно быть пустыми.', isErrorExist: false}
            ],
            similarErrorText: 'Задача с таким названием уже есть.',
        };
    },
    watch: {
        newTaskIndex: function (val) {
            if (val > 0) {
                this.formTitle = '';
                this.formDescr = '';
            }
        }
    },
    methods: {
        cancelEdit() {
            this.$emit('off-edit-mode');
            this.$emit('reset-similar-error');
        },
        saveItem() {
            let newTitle = this.formTitle.trim();
            let newDescr = this.formDescr.trim();
            this.checkFormData(newTitle, newDescr);
            if (this.emitEnabled) {
                this.$emit('do-save-item', newTitle, newDescr);
            }
        },
        resetForm() {
            for (let i = 0; i < this.formElements.length; i++) {
                this.formElements[i].isErrorExist = false;
            }
            this.formTitle = '';
            this.formDescr = '';
            this.$emit('reset-similar-error');
        },
        addTask() {
            let newTitle = this.formTitle.trim();
            let newDescr = this.formDescr.trim();
            this.checkFormData(newTitle, newDescr);
            if (this.emitEnabled) {
                this.$emit('add-task', { title: newTitle, descr: newDescr, done: false, isEditMode: false, isSimilarError: false });
            }
        },
        checkFormData(title, descr) {
            if (title.length && descr.length) {
                this.emitEnabled = true;
                this.formElements[0].isErrorExist = false;
                this.formElements[1].isErrorExist = false;
            } else {
                this.emitEnabled = false;
                this.formElements[0].isErrorExist = title.length < 1;
                this.formElements[1].isErrorExist = descr.length < 1;
            }
        }
    }
};
</script>

<style scoped lang="scss">
.control-form {
    button + button {
        margin-left: 5px;
    }
}
</style>