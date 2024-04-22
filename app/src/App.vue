<template>
  <v-app>
    <v-main>
      <div class="site-wrapper">
        <div class="nodes-wrapper">
          <NodeDisplay v-for="(node, index) in bookmarkDisplay" :node="node" :index="index"></NodeDisplay>
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script setup>
import NodeDisplay from './components/NodeDisplay.vue';
import { onBeforeUnmount, onMounted, ref } from 'vue';
let searchedBookmarkArray = [];
let t = ref(false);
let bookmarkArray = [];
let bookmarks = []
let bookmarkDisplay = ref([]);

chrome.bookmarks.getTree((tree) => {
  bookmarks = tree[0].children[0];
  bookmarkDisplay.value.push(tree[0].children[0]);
});

let keys = ref('');
window.addEventListener('keypress', (e) => {
  console.log(e);
  console.log(bookmarkDisplay)
  if (!parseInt(e.key)) return 
  const key = parseInt(e.key) -1;

  keys.value = keys.value + key;

  let currentMenu = bookmarkDisplay.value[bookmarkDisplay.value.length - 1];

  const bookmark = currentMenu.children[key];  
  console.log("bookmark", bookmark);
  if (bookmark.children) {
    currentMenu.active = key;
    bookmarkDisplay.value.push(bookmark);
    console.log("bookmarkDisplay", bookmarkDisplay.value);  
  } else {
    window.open(bookmark.url);
    window.close();
  }
});


document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape' || e.key === 'ArrowLeft') {
    if (bookmarkDisplay.value.length > 1) {
      bookmarkDisplay.value.pop();
      let currentMenu = bookmarkDisplay.value[bookmarkDisplay.value.length - 1];
      currentMenu.active = -1;
    }
  }
  console.log(e.key);
});



</script>

<style>
.nodes-wrapper {
  display: flex;
  gap: 10px;
  height: 100%;
  width: 100%;
}
.site-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw; 
}
</style>