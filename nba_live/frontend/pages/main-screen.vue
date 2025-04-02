<template>
  <div class="full-screen-container">
    <iframe :src="currentUrl" class="rotation-frame"></iframe>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const gameIds = ref([]);
const currentIndex = ref(0);
const pages = ["https://bsh-navy.vercel.app/playoff", "https://bsh-navy.vercel.app", "https://bsh-navy.vercel.app/quarter-stats"];
const currentUrl = ref(pages[0]); // Починаємо з загальної статистики

// ✅ Функція отримання ID матчів тільки для активних ігор
const fetchGameIds = async () => {
  try {
    const response = await fetch("https://nba-backend-p2yt.onrender.com/api/todays_games/");
    if (!response.ok) throw new Error("Failed to fetch game data");
    const data = await response.json();

    if (data.games && Array.isArray(data.games)) {
      gameIds.value = data.games
        .filter(game => game.status !== "Not Started" && game.status !== "Scheduled") // Відфільтровуємо тільки активні ігри
        .map(game => game.game_id);
    }
  } catch (err) {
    console.error("❌ Error fetching game IDs:", err);
  }
};

onMounted(async () => {
  await fetchGameIds();

  setInterval(() => {
    const activeGames = gameIds.value.map(id => `https://bsh-navy.vercel.app/game/${id}`);
    const rotationPages = [...pages, ...activeGames];

    if (rotationPages.length > 0) {
      currentIndex.value = (currentIndex.value + 1) % rotationPages.length;
      currentUrl.value = rotationPages[currentIndex.value];
    }
  }, 30000);
});
</script>

<style scoped>
/* ✅ Робимо контейнер на весь екран */
.full-screen-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

/* ✅ `iframe` на весь екран */
.rotation-frame {
  width: 100vw;
  height: 100vh;
  border: none;
}
</style>

<style>
/* ✅ Забираємо всі відступи */
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  width: 100%;
  height: 100%;
}
</style>
