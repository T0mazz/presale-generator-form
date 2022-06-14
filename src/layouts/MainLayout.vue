<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated id="hdr-c">
      <q-toolbar>
        <q-btn class="nav-item" to="/" flat>
          <img src="~assets/uncx_logo.svg" />
        </q-btn>
        <q-btn class="nav-item" to="/form" flat label="PG Form" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MainLayout',
});
</script>
