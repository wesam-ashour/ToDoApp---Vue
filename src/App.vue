<script setup>
import {ref, computed} from 'vue'

const header = ref('Shopping List App')
const characterCount = computed(() => {
    return newItem.value.length
})
const editing = ref(false)
const items = ref([
    {id: 1, label: "10 party hats", purchased: true, HighPriority: true},
    {id: 2, label: "2 board games", purchased: true, HighPriority: false},
    {id: 3, label: "20 cups", purchased: false, HighPriority: true},
])
const newItem = ref("")
const newItemHighPriority = ref(false)

const saveItem = () => {
    items.value.push({id: items.value.length + 1, label: newItem.value, HighPriority: newItemHighPriority.value});
    newItem.value = "";
    newItemHighPriority.value = false;
}

const deleteItem = (id) => {
    items.value = items.value.filter(item => item.id !== id);
};

const doEdit = (e) => {
    editing.value = e;
    newItem.value = ""
    newItemHighPriority.value = false;

}

const togglpurchased = (item) => {
    item.purchased = !item.purchased
}
</script>

<template>
    <div class="header">
        <h1>{{ header }}</h1>
        <button v-if="editing" class="btn" @click="doEdit(false)">cancel</button>
        <button v-else class="btn btn-primary" @click="doEdit(true)">Add item</button>
    </div>

    <form class="add-item-form"
          v-if="editing"
          @submit.prevent="saveItem">
        <input v-model.trim="newItem"
               type="text" placeholder="Add an item">
        <label style="color: red">
            <input type="checkbox" v-model="newItemHighPriority">
            High Priority
        </label>
        <button :disabled="newItem.length === 0" class="btn btn-primary">
            Save Item
        </button>
    </form>
    <p class="counter">
        {{characterCount}}/200
    </p>
    <ul>
        <li v-for="(item, index) in items" :key="item.id"
            @click="togglpurchased(item)"
            :class="[{strikeout:item.purchased , priority:item.HighPriority}]">
            {{ item.label }}
            <button @click="deleteItem(item.id)">delete</button>

        </li>
    </ul>
    <p v-if="!items.length">There is no items!</p>
</template>

<style scoped>

</style>
