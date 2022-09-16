<script>
import data from './data'
import ItemView from './ItemView.vue'
import ListView from './ListView.vue'
import SearchBox from './SearchBox.vue'

export default {
    data() { return {
        viewStack: [data],
        highlighted: undefined
    }},
    components: {
        ItemView, ListView, SearchBox
    },
    methods: {
        // basic operations
        goBack() {
            if(!this.canGoBack) return;
            
            if(this.highlighted) this.highlighted = undefined;
            this.viewStack.pop()
        },
        goTo(newView) {
            console.log("goto", newView);
            this.viewStack.push(newView)
        },
        highlight(child) {
            this.highlighted = child;
        },

        // event handlers
        childClicked(id) {
            if(!this.currentView.children) {
                log.error("Sidebar: no children");
                return;
            }

            console.log("childClicked", id);

            const child = this.currentView.children.find(c => c.id === id);
            // can load child data here
            if(child.type === "item") this.highlight(child);
            else this.goTo(child);
        },
        onSearchResults(results, searchStatus) {
            console.log("on search results", results, searchStatus);
            if(this.currentView.type === "search-results") {
                this.currentView.children = results;
                if(searchStatus === "cancelled") this.goBack();
            } else {
                this.goTo({
                    type: "search-results",
                    name: "Search Results",
                    children: results
                });
            }
        }
    },
    computed: {
        currentView() {
            return this.viewStack[this.viewStack.length-1];
        },
        canGoBack() {
            return this.viewStack.length > 1;
        }
    }
}
</script>
<template>
<p>
    <button v-if="canGoBack" @click="goBack">Go Back</button>
</p>
<SearchBox @results="onSearchResults" />

<ItemView v-if='currentView.type === "item"' :data='currentView' />
<ListView v-else :data='currentView' :highlighted='highlighted' @click='childClicked' />

</template>
