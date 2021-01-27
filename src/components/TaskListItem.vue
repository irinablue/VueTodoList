<template>
    <div>
        <div v-if="task.isEditMode">
            <TaskControlForm
                :is-edit-mode-on="true"
                :data="editFormData"
                :is-similar-error="task.isSimilarError"
                v-on="$listeners"
                @off-edit-mode="offEditMode"
                @do-save-item="saveTask"
            />
        </div>
        <div v-else>
            <ItemHeader 
                :title="task.title"
                :index="task.key"
                :is-done="task.done"
                @do-edit-action="enableEditMode"
                @do-done-action="changeDoneStatusTask"
                v-on="$listeners"
            />
            <ItemBody :descr="task.descr" />
        </div>
    </div>
</template>

<script>
import ItemHeader from './ItemHeader.vue';
import ItemBody from './ItemBody.vue';
import TaskControlForm from './TaskControlForm.vue';

export default {
    components: {
        ItemHeader,
        ItemBody,
        TaskControlForm
    },
    props: {
        task: {
            type: Object,
            default: function () {
                return { title: '', descr: '', done: false, isEditMode: false, key: 0, isSimilarError: false };
            }
        }
    },
    computed: {
        editFormData: function () {
            return [
                this.task.title,
                this.task.descr
            ];
        }
    },
    methods: {
        offEditMode() {
            this.task.isEditMode = false;
        },
        saveTask(newTitle, newDescr) {
            this.$emit('do-save-task', newTitle, newDescr, this.task.key);
        },
        enableEditMode() {
            this.task.isEditMode = true;
        },
        changeDoneStatusTask() {
            this.task.done = !this.task.done;
        },
    }
};
</script>