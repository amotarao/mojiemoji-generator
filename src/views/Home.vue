<template>
  <div class="home">
    <mojiemoji-generator />
  </div>
</template>

<script>
// @ is an alias to /src
import MojiemojiGenerator from "@/components/MojiemojiGenerator.vue";

export default {
  name: "home",
  components: {
    MojiemojiGenerator
  }
};
</script>
