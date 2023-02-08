<script setup lang="ts">
import { Form } from 'vee-validate'
import Input from './Input.vue'

const validationSchema = {
    newPassword(value) {
        if (!value) {
            return 'Password is required'
        }
        if (value.length < 6) {
            return 'Password must be at least 6 characters'
        }

        return true
    },
    repeatPassword(value, { form }) {
        if (!value) {
            return 'Password is required'
        }
        if (value !== form.newPassword) {
            return 'Passwords do not match'
        }

        return true
    },
}

const submit = values => {
    console.log('submit', values)
}
</script>
<template>
    <Form :validationSchema="validationSchema" @submit="submit" class="change-password-form">
        <h2>Change Password</h2>
        <Input type="password" label="Current password" name="currentPassword" />
        <Input type="password" label="New password" name="newPassword" />
        <Input type="password" label="Repeat password" name="repeatPassword" />
        <SubmitButton>Change password</SubmitButton>
    </Form>
</template>
