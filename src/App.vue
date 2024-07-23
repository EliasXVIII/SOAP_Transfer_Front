<template>
  <div id="app">
    <AppNav />
    <UploadFile />
    <DownloadFile />
    <AppFooter />
  </div>
</template>

<script>
import UploadFile from './components/UploadFile.vue';
import DownloadFile from './components/DownloadFile.vue';
import AppFooter from './components/AppFoote.vue';
import AppNav from './components/AppNav.vue'

export default {
  name: 'App',
  components: {
    AppNav,
    UploadFile,
    DownloadFile,
    AppFooter
    
  }
};
</script>
