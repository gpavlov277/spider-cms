<script>
import { ref } from "vue";
import Player from "./Player.vue";
export default {
  props: ["team"],
  components: { Player },

  setup() {
    const yellowCount = ref(0);
    const redCount = ref(0);
    const playerCards = ref({});

    function updateCards({ cardType, playerId }) {
      if (!playerCards.value[playerId]) {
        playerCards.value[playerId] = { yellow: 0, red: 0 };
      }

      if (cardType === "yellow" && playerCards.value[playerId].yellow < 2) {
        playerCards.value[playerId].yellow++;
        yellowCount.value++;
      }
      if (cardType === "red" && playerCards.value[playerId].red === 0) {
        playerCards.value[playerId].red++;
        redCount.value++;
      }
    }
    return { yellowCount, redCount, updateCards };
  },
};
</script>

<template>
  <div>
    <h2 class="text-xl font-bold mb-4 uppercase">{{ team.name }}</h2>
    <div
      class="p-4 rounded-lg shadow-md border-1 border-solid"
      :class="team.id == 2 ? 'right-table' : 'left-table'"
    >
      <table class="w-full">
        <thead>
          <tr class="border-b-2 border-gray-200">
            <th class="py-0 px-2 text-left">No</th>
            <th class="py-0 px-2 text-left">Name</th>
            <th class="py-0 px-2 text-left">Position</th>
            <th class="py-0 px-2 text-center"></th>
          </tr>
        </thead>
        <tbody>
          <Player
            v-for="player in team.players"
            :key="player.id"
            :player="player"
            @updateCards="updateCards"
          />
        </tbody>
      </table>
    </div>
    <p class="mt-2 text-sm font-bold">
      {{ yellowCount }} yellow card &nbsp;&nbsp; {{ redCount }} red card
    </p>
  </div>
</template>

<style>
.left-table {
  background-color: #fcfbff;
}
.right-table {
  background-color: #e9edfe;
}
</style>
