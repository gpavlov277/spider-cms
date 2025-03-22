<script>
import { ref, onMounted } from "vue";
import Team from "./Team.vue";

export default {
  components: { Team },
  setup() {
    const match = ref(null);
    const loading = ref(true);
    const error = ref(null);

    onMounted(async () => {
      try {
        await new Promise((resolve) => setTimeout(resolve, 700));
        const response = await fetch("/data/match.json");
        if (!response.ok) throw new Error("Грешка при зареждане на данните");
        match.value = await response.json();
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    });

    return { match, loading, error };
  },
};
</script>

<template>
  <div v-if="loading">Loading...</div>
  <div v-else-if="error">{{ error }}</div>
  <div v-else>
    <div class="w-full text-center">
      <div class="grid grid-cols-2 gap-35">
        <Team v-for="team in match.teams" :key="team.id" :team="team" />
      </div>
      <div class="mt-6 text-center">
        <p class="font-bold">{{ match.stadium }}</p>
        <p class="font-bold">{{ match.referee }}</p>
      </div>
    </div>
  </div>
</template>
