<template>
    <div class="app-container-inner">
        <div class="left-container">
            <div
                v-if="tasks.length"
                class="task-list"
            >
                <div
                    v-for="(filteredTask, index) in filteredTasks"
                    :key="index"
                    class="task-list-column"
                >
                    <div
                        v-for="(task, innerIndex) in filteredTask"
                        :key="innerIndex"
                        class="task-list-item"
                        :class="{ 'is-done' : task.done}"
                    >
                        <div
                            class="task-list-item-inner"
                        >
                            <TaskListItem 
                                :task="task"
                                @do-delete-action="deleteTask"
                                @add-task="addTask"
                                @do-save-task="saveTask"
                                v-on="$listeners"
                            />
                        </div>
                    </div>
                </div>
            </div>
            <div 
                v-else 
                class="task-list-empty"
            >
                Нет задач!
            </div>
        </div>
        <div class="right-container">
            <TasksCounter
                :count="completedTasksCount"
                title="Выполненных задач"
            />
            <TasksCounter
                :count="uncompletedTasksCount"
                title="Невыполненных задач"
            />
            <CompleteAllTasksControl
                v-if="filteredTasks[0].length"
                title="Выполнить все задачи"
                @complete-all-tasks="completeAllTasks"
            />
            <TaskControlForm
                :new-task-similar-error-exist="newTaskSimilarErrorExist"
                :new-task-index="newTaskIndex"
                @add-task="addTask"
                @reset-similar-error="resetNewTaskSimilarError"
                v-on="$listeners"
            />
        </div>
    </div>
</template>

<script>
import TaskListItem from './TaskListItem.vue';
import TaskControlForm from './TaskControlForm.vue';
import TasksCounter from './TasksCounter.vue';
import CompleteAllTasksControl from './CompleteAllTasksControl.vue';

export default {
    components: {
        TaskListItem,
        TaskControlForm,
        TasksCounter,
        CompleteAllTasksControl
    },
    data() {
        return {
            tasks: [
                { title: 'Ремонт', descr: 'описание ремонта', done: false, isEditMode: false, isSimilarError: false },
                { title: 'Зарядка', descr: 'чтоб не болела спина', done: true, isEditMode: false, isSimilarError: false },
                { title: 'Заказать воду', descr: 'позвонить курьеру утром', done: false, isEditMode: false, isSimilarError: false },
                { title: 'Купить подарок', descr: 'поискать что подарить родителям', done: false, isEditMode: false, isSimilarError: false }
            ],
            newTaskSimilarErrorExist: false,
            newTaskIndex: 0
        };
    },
    computed: {
        filteredTasks: function () {
            let completedTasks = [];
            let uncompletedTasks = [];
            for (let i = 0; i < this.tasks.length; i++) {
                if (this.tasks[i].done) {
                    let k = completedTasks.push(this.tasks[i]);
                    completedTasks[k-1].key = i;
                } else {
                    let k = uncompletedTasks.push(this.tasks[i]);
                    uncompletedTasks[k-1].key = i;
                }
            }
            return [
                uncompletedTasks,
                completedTasks
            ];
        },
        uncompletedTasksCount: function () {
            return this.filteredTasks[0].length;
        },
        completedTasksCount: function () {
            return this.filteredTasks[1].length;
        }
    },
    methods: {
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        addTask(task) {
            if (!this.similarTaskExist(task.title, task.descr)) {
                this.newTaskIndex = this.tasks.push(task);
                this.resetNewTaskSimilarError();
            } else {
                this.newTaskSimilarErrorExist = true;
            }
        },
        saveTask(newTitle, newDescr, index) {
            if (newTitle !== this.tasks[index].title && this.similarTaskExist(newTitle, newDescr)) {
                this.tasks[index].isSimilarError = true;
            } else {
                this.tasks[index].title = newTitle;
                this.tasks[index].descr = newDescr;
                this.tasks[index].isEditMode = false;
                this.tasks[index].isSimilarError = false;
            }
        },
        similarTaskExist(title) {
            let exist = false;
            for (let i = 0; i < this.tasks.length; i++) {
                if (this.tasks[i].title === title) {
                    exist = true;
                    break;
                }
            }
            return exist;
        },
        completeAllTasks() {
            for (let i = 0; i < this.tasks.length; i++) {
                this.tasks[i].done = true;
            }
        },
        resetNewTaskSimilarError() {
            this.newTaskSimilarErrorExist = false;
        }
    }
};
</script>

<style scoped lang="scss">
.left-container {
    flex: 0 0 $twoThirdWidthColumn;

    @media all and (max-width: $breakpointTablet) {
        flex-basis: $fullWidthColumn;
        order: 1;
    }
}
.right-container {
    flex-basis: $thirdWidthColumn;
    margin-left: auto;

    .control-form-container {
        margin-top: 15px;
        padding-top: 15px;
        border-top: 1px #ccc solid;
    }

    @media all and (max-width: $breakpointTablet) {
        flex-basis: $fullWidthColumn;
        order: 0;
        margin-bottom: 5px;

        .control-form-container {
            margin-bottom: 10px;
            padding-bottom: 15px;
            border-bottom: 1px #ccc solid;
        }
    }
}
.task-list {
    display: flex;
    flex-wrap: wrap;

    .task-list-column {
        flex: 0 0 $halfWidthColumn;

        @media all and (max-width: $breakpointMobile) {
            flex-basis: $fullWidthColumn;
        }
    }

    .task-list-item {
        padding: 0 10px 10px 0;

        &.is-done {
            .task-list-item-inner {
                background-color: rgba(95,222,95,.1);
            }
        }

        .task-list-item-inner {
            position: relative;
            padding: 10px;
            height: 100%;
            border: 1px #ebebeb solid;
            border-radius: 5px;
        }
    }
}
</style>