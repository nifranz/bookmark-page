<script setup>
import { defineProps, onBeforeUnmount, ref } from 'vue';

const props = defineProps(['node', 'index']);
let showAnim = ref(props.index != 0);
let showBackAnim = ref(false);  
const zIndex = 1000 - props.index;

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
    <div class="node-wrapper" :class="{'anim': showAnim}" :style="{'z-index': zIndex}">
        <h2 class="mb-3">{{ node.title }}</h2>
        <ul >
            <li 
                v-for="(child, index) in node.children" 
                key:child 
                :class="{'active': node.active == index}"
            >
                <v-icon :icon="child.children ? 'mdi-folder' : 'mdi-web'" size="small" color="#444444"></v-icon> 
                {{ index + 1 }} - {{ child.title }}
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
    cursor: pointer;
    padding: 5px;
    padding-inline: 10px;
    border-radius: 5px;
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 10px;
}
li:hover, .active{
    background-color: #f3f3f3;
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
.anim-back {
    animation: fadeIn 0.2s cubic-bezier(.05, .8, .1, .95);
    animation-direction: reverse;
}

.anim {
    animation: fadeIn 0.2s cubic-bezier(.05, .8, .1, .95);
}
h2 {
    color: rgb(35, 35, 35);
    padding-inline: 10px;;
}
.node-wrapper {
    color: rgb(42, 42, 42);
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
}
</style>