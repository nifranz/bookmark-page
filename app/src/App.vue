<template>
  <v-app>
    <v-main>
      <div class="site-wrapper">
        <div class="credits"><a href="https://github.com/nifranz" target="_blank" rel="noopener noreferrer"> &#60; coded with <v-icon class="heart" icon="mdi-heart" x-small></v-icon> by <span class="github-link"><v-icon icon="mdi-github" x-small></v-icon>nifranz / &#62; </span></a></div>
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
      bookmarkDisplay.value[bookmarkDisplay.value.length - 1].del = -1;
      let currentMenu = bookmarkDisplay.value[bookmarkDisplay.value.length - 2];
      currentMenu.active = -1;
      setTimeout(() => {
        bookmarkDisplay.value[bookmarkDisplay.value.length - 1].del = undefined;
        bookmarkDisplay.value.pop();
      }, 101);
    }
  }
  console.log(e.key);
});



</script>

<style>
.github-link {
  display:inline-block;
}
.credits {
  z-index: 2000;
  text-align: center;
  width: 400px;
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  padding: 10px;
  font-size: 15px;
  color: #444444;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;

}
.credits a {
    color:#444444;;
    text-decoration: none;
}
.credits a:hover .heart {
  color: red;

}
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