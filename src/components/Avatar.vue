<script setup lang="ts">
import { ref } from 'vue'
import demoPng from '../demo-avatar.png?url'

const avatarInput = ref<HTMLInputElement>()
const avatarUrl = ref(demoPng)

const handleChange = () => {
    avatarInput.value?.click()
    const file = avatarInput.value?.files?.[0]
    if (!file) return
}

const fileSelected = () => {
    const file = avatarInput.value?.files?.[0]
    if (!file) return
    const reader = new FileReader()
    reader.onload = e => {
        avatarUrl.value = e.target?.result?.toString() ?? avatarUrl.value
        avatarInput.value!.value = undefined as any
    }
    reader.readAsDataURL(file)
}
</script>
<template>
    <input type="file" accept="image/*" aria-hidden="true" tabindex="-1" hidden ref="avatarInput" @change="fileSelected" />
    <div class="avatar-container">
        <img loading="lazy" :src="avatarUrl" class="avatar-image" alt="User Avatar" />
        <div class="avatar-remove-action-container">
            <div class="avatar-action">
                <!-- remove icon -->
                <svg viewBox="0 0 24 24">
                    <path d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1 1H5v2h14V4z"></path>
                </svg>
            </div>
        </div>
        <div class="avatar-container--avatar-edit">
            <!-- edit icon -->
            <svg viewBox="0 0 24 24" @click="handleChange">
                <path
                    d="M3 17.25V21h3.75L17.81 9.94l-3.75-3.75L3 17.25zM20.71 7.04c.39-.39.39-1.02 0-1.41l-2.34-2.34a.9959.9959 0 0 0-1.41 0l-1.83 1.83 3.75 3.75 1.83-1.83z"
                ></path>
            </svg>
        </div>
    </div>
</template>
