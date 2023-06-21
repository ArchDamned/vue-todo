<script setup>
import {ref, onMounted, computed, watch} from "vue"

const name = ref('')

watch (name, (newVal) => {
    localStorage.setItem('name', newVal)
})

onMounted(() => {
    name.value = localStorage.getItem('name') || ''
})
</script>

<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">{{ name ? "Привет,": "Как тебя зовут?" }} <input type="text" placeholder="Введи имя" v-model="name"></h2>
        </section>
        <section><router-link to="/tasks" class="to-tasks">К задачам!</router-link></section>
    </main>
</template>