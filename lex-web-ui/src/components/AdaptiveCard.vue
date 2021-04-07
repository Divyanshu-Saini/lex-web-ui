<template>
  <adaptive-cards
    :card="card"
    :data="data"
    :use-templating="true"
    :host-config="config"
    @onActionClicked="onItemClick($event)"
  />
</template>

<script>
/* eslint no-underscore-dangle: 0 */
/* eslint-disable */
import { AdaptiveCards } from "adaptivecards-vue";

export default {
  name: "adaptive-card",
  props: ["customPayload"],
  components: {
    AdaptiveCards,
  },
  data() {
    return {
      data: "",
      card: "",
      config: "",
    };
  },
  methods: {
    onItemClick(event) {
      const message = {
        type: "human",
        text: JSON.stringify(event._processedData),
        event: event._processedData,
      };

      this.$store.dispatch("postTextMessage", message);
    },
  },
  created() {
    console.info("In adaptiveCards :", this.customPayload);
    if (this.customPayload.isCustom && this.customPayload.customPayload) {
      this.card = this.customPayload.customPayload
        ? this.customPayload.customPayload
        : "";
      // this.data = this.customPayload.customPayload.data
      //   ? this.customPayload.customPayload.data
      //   : "";
      // this.config = this.customPayload.customPayload.config
      //   ? this.customPayload.customPayload.config
      //   : "";
    }
  },
};
</script>

<style>
</style>