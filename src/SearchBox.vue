<script>
export default {
    data(){ return {
        searchQuery: "",
        searchData: {},
        status: "not-started"
    }},
    created() {
        // load, process & populate search data here
        this.searchData = {
            "asd": { type: "item", id: "1", name: "asd" },
            "qwe": { type: "item", id: "2", name: "qwe" }
        }
    },
    methods: {
        onSearchInput(evt) {
            console.log("on search query")

            const newQuery = evt.target.value;
            if(this.searchQuery !== "" && newQuery === "") {
                this.status = "cancelled";
            }
            if(newQuery !== "") {
                this.status = "searching";
            } 

            this.searchQuery = newQuery;
            let searchResults = [];

            if(this.searchQuery !== "") {
                // we might customize how to search here
                for (const [key, value] of Object.entries(this.searchData)) {
                    console.log("checking", key)
                    if(key.startsWith(this.searchQuery)) searchResults.push(value);
                }
            }

            this.$emit("results", searchResults, this.status);
        },
        onFocus(evt) {
            this.searchQuery = evt.target.value;
            this.onSearchInput(evt);
        }
    }
}
</script>
<template>
    <input type="text" @input="onSearchInput" @focus="onFocus" placeholder="Search" />
</template>