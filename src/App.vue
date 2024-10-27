<template>
    <table style="width: 1000px">
        <thead>
            <th>Template with .value</th>
            <th>Template without .value</th>
            <th>v-model without .value</th>
            <th>With .value</th>
        </thead>
        <tr v-for="item in items" :key="item.id">
            <!--   ^^^^
                See intellisense here
            -->

            <!-- Extension marks this as incorrect -->
            <td>{{ item.name.value }}</td>
            <td>
                <!-- Extension marks this as correct -->
                {{ item.name }}
                <!-- Runtime doesn't mind, shows it correctly -->
            </td>

            <!-- But this is where the trouble begins -->

            <td>
                <!-- What the extension expects -->
                <input type="text" v-model="item.name" />
                <!-- Editing this field breaks the application -->
            </td>

            <td>
                <!-- What is actually correct -->
                <input type="text" v-model="item.name.value" />
            </td>
        </tr>
    </table>

    <!-- This is to show the application breaking after editing the input field without .value -->
    <div>
        <p style="margin-bottom: 0">Counter: {{ counter }}</p>
        <button @click="counter++">Count +1</button>
    </div>
</template>

<script setup lang="ts">
import { ref, shallowReactive, type Ref } from "vue";

const counter = ref(0);

interface MyItem {
    name: Ref<string>;
    id: string;
}

const items = shallowReactive<MyItem[]>([
    {
        name: ref("test"),
        id: crypto.randomUUID(),
    },
]);
</script>

<style>
table,
th,
td {
    border-collapse: collapse;
    border: 1px solid;
}
</style>
