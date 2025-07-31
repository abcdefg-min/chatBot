<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    item:{
        type: Object,
        required: true,
    }
})
</script>

<template>
    <li >
            <div v-text="item.name"></div>
            <div style="margin: 20px 0;" v-text="item.description"></div>
            <p>
                <template v-if="item.frequency === 1">{{ "Каждый день" }}</template>
                <template v-else-if="item.frequency === 7">{{ "Раз в неделю" }}</template>
                <template v-if="item.frequency === 30">{{ "Раз в месяц" }}</template>
            </p>
            <slot name="action"></slot>
            <!-- <div v-text="item.period"></div> -->
        </li>
</template>