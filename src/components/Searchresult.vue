<script setup>
import { ref, defineProps, onUpdated, watch } from 'vue';
const activeindex = null




const isListVisible = ref(true);


const props = defineProps({
    data: Array,
    default: null
})

const emit = defineEmits(['placeData'])

const selectedPlace = (index) => {
    let data = props.data[index]


    emit('placeData', data)
    isListVisible.value = false;

}

watch(
    () => props.data,
    () => isListVisible.value = true
)


</script>
<template>
    <div class="absolute border-none  rounded-xl min-w-[500px] overflow-auto top-10 z-10 p-1 pl-2">
        <ul v-if="isListVisible">
            <li v-if="props.data" v-for=" (place, index) in props.data.slice(0, 5)" :key="index"
                @click="selectedPlace(index)" :class="[index % 2 == 0 ? 'bg-gray-100' : 'bg-sky-200']"
                class="hover:bg-slate-400 p-3 font-extralight">{{ place.name }}</li>
        </ul>
    </div>
</template>