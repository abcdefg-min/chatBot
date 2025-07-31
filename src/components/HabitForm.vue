<script setup>
import { computed, reactive, ref, watch } from 'vue';
const habit = defineModel({default: {name:'foo'}});

const sum = computed(() => habit.value.period * habit.value.frequency);
const label = ref();

watch(() => {
    return {name: habit.value.name, description: habit.value.description}
    }, (newVal) => {
    label.value = `${newVal.name} ${newVal.description}`
})

const postHabit = async () => {
    fetch('/habit', {
        method: 'POST',
        body:JSON.stringify(habit.value),
        headers: {
            'accept': 'application/json',
            'content-type': 'application/json'
        }
        
    })
    .catch(error => {
        console.warn(error)
    })
}

const putHabit = (id) => {
    fetch('/habit/' + id, {
        method: 'PUT',
        body:JSON.stringify(habit.value),
        headers: {
            'accept': 'application/json',
            'content-type': 'application/json'
        }
        
    })
    .catch(error => {
        console.warn(error)
    })
}

const submitHandler = async () => {
    if(habit.id != null) {
        putHabit();
    }
    else {
        postHabit();
    }
}

 </script>

<template>
    <div class="container">
        <form @submit.prevent="submitHandler()">
            <h1>Трекер привычек</h1>
            {{ label }}
            <div class="from-group">
                <label for="habitName">Название привычки<br>
                    <input v-model.trim="habit.name" id="habitName" name="habitName" placeholder="Введите название привычки"
                        required></label>
                <br><br>
            </div>

            <div class="form-group">
                <label for="description">Описание:</label><br>
                <textarea v-model.trim="habit.description" id="description" rows="5" placeholder="Опишите вашу привычку"
                    required></textarea>
                <br><br>
            </div>

            <div class="form-group">
                <label for="frequency">Частота выполнения:
                    <select v-model.trim="habit.frequency" name="frequency" id="frequency">
                        <option :value="1">Ежедневно</option>
                        <option :value="7">Еженедельно</option>
                        <option :value="30">Ежемесячно</option>
                    </select>
                </label>
                <br>
            </div>

            <div class="form-group">
                <label for="period">Количество повторений <br>
                    <input  v-model.number="habit.period" id="period" type="period" placeholder="Введите количество повторений" required>
                </label>
                <br><br>
            </div>
            <div class="sum-group">Общее количество выполнений <br>
                <span id="sum">{{ sum }}</span></div>
            <button type="submit">Сохранить</button>
            <button type="button">простое нажатие</button>
        </form>
    </div>
</template>