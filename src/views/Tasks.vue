<script setup>
import { ref, onMounted, computed, watch } from "vue"

const tasks = ref([])
const name = ref('')

const input_content = ref('')

const tasks_asc = computed(() => tasks.value.sort((a, b) => {
    return b. createdAt - a.createdAt
}))

const addTask = () => {
    if (input_content.value.trim() === '') {
        return
    }

    tasks.value.push({
        content: input_content.value,
        done: false,
        createdAt: new Date().getTime()
    })

    input_content.value = ''
}

const removeTask = task => {
    tasks.value = tasks.value.filter(t => t !== task)
}

watch(tasks, newVal => {
    localStorage.setItem('tasks', JSON.stringify(newVal))
}, { deep: true })

watch (name, newVal => {
    localStorage.setItem('name', newVal)
})

onMounted(() => {
    name.value = localStorage.getItem('name') || ''
    tasks.value = JSON.parse(localStorage.getItem('tasks')) || []
})
</script>
<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">Твои задачи</h2>
        </section>

        <section class="create-task">

            <form @submit.prevent="addTask">
                <h4>Новая задача?</h4>
                <input 
                    type="text" 
                    placeholder="например, покушать" 
                    v-model="input_content" />

                <input type="submit" value="Создать"/>
            </form>
        </section>

        <section class="tasks-list">
            <h3>Список задач {{ tasks.length==0 ? "пока пуст. Добавь задачу ↑" : ''}}</h3>
            <div class="list">
                <div v-for="task in tasks_asc" :class="`task-item ${task.done && 'done'}`">
                    <label>
                        <input type="checkbox" v-model="task.done"/>
                        <span class="bubble"></span>
                    </label>
                    <div class="task-content">
                        <input type="text" v-model="task.content">
                    </div>
                    <div class="actions">
                        <button class="button delete" @click="removeTask(task)">Удалить</button>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>