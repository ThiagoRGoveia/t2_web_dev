<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title> TRIVIA GAME </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer elevated >
      <q-toolbar style="min-height: 30px;">
        <q-toolbar-title>
          <q-avatar>
            <img id="img" :src="require('src/assets/icmc_logo.png')" alt="Logo" style="background-color: rgb(233, 207, 207);"/>
          </q-avatar>
          Desenvolvimento Web e Mobile - SSC0961
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>
 
<script>
export default {
  name: "MainLayout",
  components: {},
};
</script>