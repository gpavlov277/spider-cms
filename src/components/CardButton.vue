<script>
export default {
  props: ["color"],
};
</script>

<template>
  <td class="py-1 px-4 flex justify-center space-x-3">
    <button
      class="cursor-pointer w-10 h-7 rounded bg-yellow-400"
      @click="$emit('giveCard', 'yellow')"
    ></button>
    <button
      class="cursor-pointer w-10 h-7 rounded bg-red-500"
      @click="$emit('giveCard', 'red')"
    ></button>
  </td>
</template>
