<template>
    <div class="w-screen h-max min-h-screen bg-slate-100 flex justify-center flex-wrap content-start">
        <div class="w-11/12 mt-6 flex flex-nowrap">
            <h1 class="text-4xl font-bold">
                Hello, </h1>
            <input v-model="name" class="bg-transparent text-4xl font-bold outline-none w-full" type="text"
                placeholder="Your name here">
        </div>
        <form @submit.prevent="createToDo" class="w-full flex flex-wrap justify-center">
            <div class="w-11/12 mt-5">
                <p class="text-lg font-light">
                    CREATE YOUR TODO:
                </p>
            </div>
            <div class="w-11/12 mt-2">
                <input v-model="toDoStr" class="w-full text-lg outline-none p-2 rounded" type="text"
                    :placeholder="'For example: ' + examples[randomNum]">
            </div>
            <div class="w-11/12 mt-5">
                <p class="text-lg font-light">
                    PICK A CATEGORY:
                </p>
            </div>
            <div class="w-11/12 mt-2 flex gap-4 flex-nowrap">
                <label class="w-6/12 flex justify-center bg-white py-6 flex-wrap gap-3 cursor-pointer rounded">
                    <p class="text-lg font-medium text-green-400">
                        Privat
                    </p>
                    <div class="w-full flex justify-center">
                        <input v-model="category" class="" type="radio" name="category" value="privat">
                    </div>
                </label>
                <label class="w-6/12 flex justify-center bg-white py-6 flex-wrap gap-3 cursor-pointer rounded">
                    <p class="text-lg font-medium text-indigo-400">
                        Business
                    </p>
                    <div class="w-full flex justify-center">
                        <input v-model="category" class="" type="radio" name="category" value="business">
                    </div>
                </label>
            </div>
            <div class="w-11/12 mt-2">
                <input class="w-full bg-purple-500 text-white py-2 rounded cursor-pointer" type="submit"
                    value="Create ToDo">
            </div>
        </form>
        <div class="w-11/12 h-max flex flex-wrap gap-3 mt-14 mb-5">
            <div v-for="(item, index) in toDos" :key="index"
                class="w-full bg-white px-5 py-3 relative rounded flex justify-between items-center flex-wrap">
                <div class="w-1 h-full rounded-l absolute top-0 left-0"
                    :class="{ 'bg-green-400': item.category === 'privat', 'bg-indigo-400': item.category === 'business', 'bg-slate-200': item.done }">
                </div>
                <div class="flex gap-4">
                    <input class="cursor-pointer checked:outline-1" type="radio" :checked="item.done" @change="item.done = true">
                    <p class="text-lg" :class="{'text-slate-500': item.done}">{{ item.toDo }}</p>
                </div>
                <button @click="toDos.splice(index, 1)" class="bg-red-400 text-white py-1 px-3 rounded ml-auto"
                :class="{'bg-slate-400': item.done}">
                    Delete
                </button>
            </div>
            <div v-if="toDos.length === 0" class="w-full flex justify-center">
                <p class="text-lg font-medium">
                    You have currently no ToDo's
                </p>
            </div>
        </div>
    </div>
</template>

<script setup>
    import {ref, onMounted, watch} from 'vue';

    const name = ref('');
    const toDoStr = ref('');
    const category = ref(null);
    const randomNum = ref(0);
    const toDos = ref([]);
    const examples = [
        'Write an article',
        'Wash the car',
        'Clean the room',
        'Go shopping',
        'Prepare for a meeting',
        'Cook a meal',
        'Text someone',
        'Cut the grass',
    ];

    onMounted(() => {
        if (localStorage.getItem('name')) {
            name.value = localStorage.getItem('name');
        } else {
            name.value = '';
        }

        if (localStorage.getItem('toDos')) {
            toDos.value = JSON.parse(localStorage.getItem('toDos'));
        } else {
            toDos.value = [];
        }

        randomNum.value = Math.floor(Math.random() * 7);
        console.log(toDos.value);
    })

    watch(toDos, newToDos => {
        localStorage.setItem('toDos', JSON.stringify(newToDos));
    }, { deep: true });


    watch(name, newName => {
        localStorage.setItem('name', newName);
    });


    function createToDo() {
        if (toDoStr.value.trim() === '') {
            alert('ToDo can\'t  be empty');
        } else if (category.value === null) {
            alert('You haven\'t picked a category');
        } else {
            toDos.value.unshift({ toDo: toDoStr.value.trim(), category: category.value, done: false });
            toDoStr.value = '';
            category.value = null;
        }
    }


</script>

<style>

</style>
