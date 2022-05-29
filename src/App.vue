<template>
  <div class="monaco-editor">
      	<to-editor></to-editor>
  </div>
</template>
<script>
 import ToEditor from "./components/ToEditer.vue";
 export default {
   name:"Programming",
   components: { ToEditor },
 };
</script>

<style>
#app,
body,
html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;

}

body,
body * {
  box-sizing: border-box;
}
.monaco-editor {
  width: 100%;
  height: 100%;
  /* margin-top: 40px; */
}
</style>
