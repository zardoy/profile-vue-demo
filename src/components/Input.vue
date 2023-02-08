<script lang="ts" setup>
import { Field, useField } from 'vee-validate'
import { computed, watch } from 'vue'

const { name } = defineProps<{
    name: string
    label?: string
}>()

const { errorMessage, meta, value, validate } = useField(
    name,
    {},
    {
        validateOnValueUpdate: false,
    },
)

const isValid = computed(() => meta.valid && meta.dirty && value.value)
watch(value, () => {
    if (errorMessage.value) {
        validate()
    }
})
</script>
<script lang="ts">
export default {
    inheritAttrs: false,
}
</script>
<template>
    <label class="input-outer-container">
        <p class="input--label">{{ label }}</p>
        <div class="input--container">
            <slot />
            <slot name="input">
                <Field
                    :name="name"
                    :placeholder="label"
                    class="input--field"
                    v-bind="$attrs"
                    v-on="$attrs"
                    :class="{
                        error: errorMessage,
                        success: isValid,
                    }"
                />
            </slot>
        </div>
        <span class="error-text">{{ errorMessage }}</span>
    </label>
</template>
<style scoped lang="scss">
.input--label {
    font-size: 1.1rem;
    text-align: start;
    margin-top: 0;
    margin-bottom: 5px;
}
.input-outer-container {
    --error-color: #f23648;
    --error-bg-color: #fddfe2;
    --success-color: #21a67a;
    --success-bg-color: #e0eee4;

    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    /* add margin bottom */
    &:not(:last-of-type) {
        /* provide additional spacing between elements */
        padding-bottom: calc(1rem + 6px);
    }
}
.input--container {
    display: flex;
    justify-content: flex-start;
    width: 100%;
    & > input {
        flex: 1;
    }
}
.input--field {
    border-radius: 5px;
    font-size: 1rem;
    outline: none;
    background-color: #f2f5f7;
    padding: 15px 10px;
    border: 2px solid transparent;
    transition: border-color 0.3s ease-in-out, color 0.3s ease-in-out, background-color 0.3s ease-in-out;

    &:focus {
        border-color: #1e90ff;
    }
    &.error {
        background-color: var(--error-bg-color);
        &:focus {
            border-color: var(--error-color);
        }
    }
    &.success {
        background-color: var(--success-bg-color);
        &:focus {
            border-color: var(--success-color);
        }
    }
}
.error-text {
    position: absolute;
    left: 0;
    bottom: 2px;
    font-size: 0.94rem;
    color: red;
    line-height: 1rem;
    height: 1rem;
}
</style>
