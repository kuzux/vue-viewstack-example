<script>
import data from './data'
import ItemView from './ItemView.vue'
import ListView from './ListView.vue'

export default {
    data() { return {
        viewStack: [data],
        highlighted: undefined
    }},
    components: {
        ItemView, ListView
    },
    methods: {
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
<ItemView v-if='currentView.type === "item"' :data='currentView' />
<ListView v-else :data='currentView' :highlighted='highlighted' @click='childClicked' />

</template>
