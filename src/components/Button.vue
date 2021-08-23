<template>
  <a class="button">{{ text }}</a>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
      default: "Button",
    },
  },
};
</script>
