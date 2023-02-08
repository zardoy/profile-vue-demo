<script setup lang="ts"></script>
<template>
    <button><slot /></button>
</template>
<style lang="scss">
button {
    margin-top: 20px;
    padding: 10px;
    border-radius: 5px;
    border: none;
    background-color: #1586ff;
    color: white;
    font-size: 1.1rem;
    width: 100%;
    cursor: pointer;

    &:hover {
        background-color: #0f6ed5;
    }
}
</style>
