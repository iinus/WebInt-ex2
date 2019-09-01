<template>
<div id="app">
  <div id="header">
  <h1 id="header-heading"> Welcome to VuePlayer </h1>
  </div><br><br>
  <VideoPlayer id="videos-player"/>
</div>
</template>

<script>
import VideoPlayer from './components/VideoPlayer.vue';
import './styles/app.css'

export default {
  name: 'app',
  components: {
    VideoPlayer
  }
}
</script>
