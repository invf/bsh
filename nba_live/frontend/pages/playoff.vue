<template>
  <div class="video-wrapper">
    <video autoplay muted loop playsinline class="full-video">
      <source src="/videos/playoff.mp4" type="video/mp4" />
      Your browser does not support the video tag.
    </video>
  </div>
</template>

<script setup>
// Якщо буде потрібно — додамо динаміку по ID
</script>

<style scoped>
.video-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: black; /* для плавного завантаження */
  z-index: 0;
}

.full-video {
  width: 100vw;
  height: 100vh;
  object-fit: cover; /* покриває повністю без чорних полів */
  border: none;
  outline: none;
}
</style>

<style>
/* Глобальні стилі для уникнення scroll */
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  height: 100%;
  width: 100%;
}
</style>
