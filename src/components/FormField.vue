<template>
    <div class="control-field">
        <label class="field-label">{{ element.label }}</label>
        <input
            v-if="element.type === 'input-text'"
            :value="value"
            class="field-input"
            type="text"
            @input="$emit('input', $event.target.value)"
        >
        <textarea
            v-else-if="element.type === 'textarea'"
            :value="value"
            class="field-textarea"
            @input="$emit('input', $event.target.value)"
        />
        <ErrorBlock
            v-if="element.isErrorExist"
            :error-text="element.errorText"
        />
    </div>
</template>

<script>
import ErrorBlock from './ErrorBlock.vue';

export default {
    components: {
        ErrorBlock
    },
    props: {
        element: {
            type: Object,
            default: function () {
                return { type: '', label: '', errorText: '', isErrorExist: false};
            }
        },
        value: {
            type: String,
            default: ''
        }
    }
};
</script>

<style lang="scss" scoped>
.control-field {
    margin-bottom: 5px;
}
.field-label {
    display: block;
}
.field-input {
    width: 100%;
    height: 30px;
}
.field-textarea {
    width: 100%;
    height: 50px;
}
</style>