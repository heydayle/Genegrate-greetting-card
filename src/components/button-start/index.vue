<template>
  <div class="tw-flex tw-justify-center tw-h-100vh tw-w-full tw-m-auto tw-p-4">
    <v-btn
      :disabled="disabled"
      outlined
      color="#fff"
      class="tw-border tw-border-white tw-animate-bounce"
      @click="start()"
    >
      {{ text }}
    </v-btn>
  </div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: "Create and Copy URL!",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },

  methods: {
    start() {
      this.$emit("Event");
    },
  },
};
</script>
<style lang="scss">
.btn-style {
  background: transparent;
}
</style>
