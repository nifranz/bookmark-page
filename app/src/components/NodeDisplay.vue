<script setup>
import { defineProps, onBeforeUnmount, onUpdated, ref } from 'vue';

const props = defineProps(['node', 'index']);
let showAnim = ref(props.index != 0);

const zIndex = 1000 - props.index;

const opensite = (url) => {
    window.open(url);
    window.close();
};


onBeforeUnmount(async () => {
    showBackAnim.value = true;
    await new Promise((resolve) => {
        setTimeout(() => {
        resolve(0);
        }, 1000);
    });
    console.log('unmounted'); 
});

</script>
<template>
        <div :class="{ 'active-node': !node.del, 'anim': showAnim}" class="node-wrapper"  :style="{'z-index': zIndex}">
            <h2 class="mb-3">{{ index == 0 ? 'Bookmark Navigator' : node.title }}</h2>
                <ul >
                    <li 
                    
                    v-for="(child, index) in node.children" 
                    key:child 
                    :class="{'active': node.active == index, 'hover': child.url}"
                    >
                        <v-icon :icon="child.children ? 'mdi-folder' : 'mdi-web'" size="small" color="#444444"></v-icon> 
                        <div @click="if (child.url) opensite(child.url);">{{ index + 1 }} - {{ child.title }}</div>
                    </li>
                </ul>

    </div>
</template>

<style scoped>
ul {
    font-size: 16px;

    list-style-type: none;
    padding: 0;
    display: flex;
    flex-direction: column;
    gap: 10px;
}
li {
    padding: 5px;
    padding-inline: 10px;
    border-radius: 5px;
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 10px;
}
.active{
    background-color: #f3f3f3;
}
.hover:hover div { 
    cursor: pointer;
    text-decoration: underline;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateX(-100px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}

.anim {
    animation: fadeIn 0.2s cubic-bezier(.05, .8, .1, .95);
}
h2 {
    color: rgb(55, 52, 48);
    padding-inline: 10px;;
}
.node-wrapper {
    transition: .1s;
    color: rgb(55, 52, 48);
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 220px;
    padding-inline: 5px;
    padding-top: 10px;
    overflow: hidden;
    overflow-y: scroll;
    border-right: 1px solid #e1e1e1;
    background-color: white;
    opacity: 0;
    transform: translateX(-100px);

}
.active-node {
        opacity: 1;
        transform: translateX(0);
}
.node-body {
    display: flex;
    flex-direction: column;
    height: 100%;
    width: 100%;
}
</style>