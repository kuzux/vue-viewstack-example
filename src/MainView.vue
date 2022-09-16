<script>
export default {
    data() { return {
        items: [
            { type: "item", id: "1", name: "asd" },
            { type: "item", id: "2", name: "qwe" }
        ]
    }}
}
</script>

<template>
    <h2>Main View</h2>
    <ul>
        <li v-for="item in items">
            <p @click="$emit('selected', item)">{{ item.name }}</p>
        </li>
    </ul>
</template>