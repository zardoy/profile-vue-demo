<script lang="ts" setup>
import { Field, useField } from 'vee-validate'

const { name } = defineProps<{
    name: string
    label?: string
}>()

const { errorMessage } = useField(name)
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
                <Field :placeholder="label" :name="name" class="input--field" v-bind="$attrs" v-on="$attrs" />
            </slot>
        </div>
        <span class="error">{{ errorMessage }}</span>
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
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    & + & {
        /* provide additional spacing between elements */
        margin-top: 12px;
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
    outline: none;
    background-color: #f2f5f7;
    padding: 15px 10px;
    border: 2px solid transparent;
    transition: border-color 0.3s ease-in-out, color 0.3s ease-in-out, background-color 0.3s ease-in-out;

    &:focus {
        border-color: #1e90ff;
    }
}
.error {
    color: red;
    margin-top: 5px;
}
</style>
