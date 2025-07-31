<script setup>
import { onMounted, reactive } from 'vue';
import HabitListItem from './HabitListItem.vue';
const emits = defineEmits({'select:habit':(ev) =>{
    return true;
}})

const habitList = reactive([
    {
        name: "Бег",
        description:"Начало в 7:00",
        frequency:1,
        period:365,
    },
    {
        name: "Отдых",
        description:"Начало в 9:00",
        frequency:1,
        period:365,
    }, 
    {
        name: "Растяжка",
        description:"Начало в 6:30",
        frequency:1,
        period:365,
    }, 
    
    
]);

const fetchHabits = async () => {
    try {
        const response = await fetch('/habit');
        const json = await response.json();
        habitList.length = 0;
        if(Array.isArray(json)) {
            habitList.push(...json);
        } {
            habitList.push(json)
        }
    } 
    catch (err) {
        console.log(err)
    }
}

onMounted(() => {
    fetchHabits();
})

const deleteHandler= (idx) => {
    habitList.splice(idx,1)
}
</script>
<template>
    <ul class="list">
        <HabitListItem v-for="(item, idx) in habitList" 
        :item="item" 
        :key="idx" 
        @click="$emit('select:habit', item)"
        class="list-item">
            <template #action>
                <button @click="deleteHandler(idx)">Удалить</button>
            </template>
        </HabitListItem>
    </ul>
</template>

<style>
.list{
    display: flex;
    flex-direction:column;
    /* align-items: flex-start; */
    flex-wrap: wrap;
    gap: 2rem;
    
    list-style: none;
    margin-top: 0;
    font-weight:bold;
}

.list-item{
    background-color: rgb(220, 255, 255);
    border-radius: 1rem;
    font-size: 20px;
    gap: 10px;
    padding: 20px;
    color: cadetblue;
    font-family:Verdana, Geneva, Tahoma, sans-serif;
    box-shadow: 0 25px 30px rgba(0, 0, 0, 0.274);
    height: 200px;
    width: 330px;
}

.p {
    margin: 20px 0;
}

.list-item:hover {
    background-color: rgb(85, 106, 122);
    cursor: pointer;
    color: rgb(211, 255, 255);
}



</style>