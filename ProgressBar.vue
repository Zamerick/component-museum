<template>
<div class="h-2 my-4 mx-2 w-full">
  <div id="incomplete" class="h-2 w-full bg-ghost"></div>
  <div id="complete" class="h-2 bg-green relative" :style="width" ></div>
</div>
</template>

<script>
export default {
  props: ["total", "completed", "tense"],
  name: "progress-bar",
  computed: {
    width() {
      // progress bars from past stages should be display at 100%,
      // The current one should calculate the width,
      // and the future ones should be 0%.
      let width = 0;
      switch (this.tense) {
        case "past":
          width = 100;
          break;

        case "future":
          width = 0;
          break;

        case "present":
          width = (this.completed / this.total) * 100;
          break;
      }
      // If width would go over 100, reset to 100. Edge case related to testing functions,
      // real data would not realistically get into this state.
      // But, eh might as well cover that scenario anyway.
      width = width >= 100 ? 100 + "%" : width + "%";
      const style = {
        width: width
      };
      return style;
    }
  }
};
</script>

<style>
#complete {
  bottom: 8px;
}
</style>
