<script>
import { ref, computed } from "vue";
import CardButton from "./CardButton.vue";

export default {
  props: ["player"],
  components: { CardButton },
  emits: ["updateCards"],
  setup(props, { emit }) {
    const yellowCards = ref(0);
    const redCards = ref(0);

    const hasMaxYellow = computed(() => yellowCards.value >= 2);
    const hasRedCard = computed(() => redCards.value > 0);

    const handleCard = (cardType) => {
      props.player.cardColor = "text-red-500 font-bold";
      if (hasRedCard.value) return;

      if (cardType === "yellow" && !hasMaxYellow.value) {
        props.player.cardColor = "text-yellow-400 font-bold";
        yellowCards.value++;
        emit("updateCards", { cardType, playerId: props.player.id });

        if (yellowCards.value === 2) {
          props.player.cardColor = "text-red-500 font-bold";
          redCards.value++;
          emit("updateCards", { cardType: "red", playerId: props.player.id });
        }
      } else if (cardType === "red") {
        props.player.cardColor = "text-red-500 font-bold";
        redCards.value++;
        emit("updateCards", { cardType, playerId: props.player.id });
      }
    };
    return { yellowCards, redCards, hasMaxYellow, hasRedCard, handleCard };
  },
};
</script>

<template>
  <tr class="border-n w">
    <td class="py-0 px-2 text-left">{{ player.id }}</td>
    <td class="py-2 px-5 text-left" :class="player.cardColor">{{ player.name }}</td>
    <td class="py-0 px-2 text-left">{{ player.position }}</td>

    <CardButton @giveCard="handleCard" />
  </tr>
</template>
