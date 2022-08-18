<template>
  <div class="flex shadow-sm ml-auto border p-5">
    <img
      v-if="!roll"
      :src="image"
      width="150"
    >
    <i
      v-else
      class="bx bx-loader-circle text-white text-9xl bx-spin my-auto"
    />
  </div>
</template>

<script>
export default {
  name: 'SlotCardComponent',
  props: {
    image: {
      type: String,
      default: ''
    },
    roll: {
      type: Boolean,
      default: false
    }
  }
}
</script>
