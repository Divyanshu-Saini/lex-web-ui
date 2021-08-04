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
import { mapGetters } from "vuex";

export default {
  name: "adaptive-card",
  props: ["customPayload"],
  components: {
    AdaptiveCards
  },
  data() {
    return {
      data: "",
      card: "",
      config: ""
    };
  },
  computed: {
    ...mapGetters(["getLexSessionAttributes"])
  },
  methods: {
    onItemClick(event) {
      let lexAttributes = this.getLexSessionAttributes;
      console.log("Adaptive Event :", event._processedData);
      let AdaptiveSessionAttributes = { ...event._processedData };
      delete AdaptiveSessionAttributes["dispMsg"];
      delete AdaptiveSessionAttributes["key"];
      const key = event._processedData["key"];
      const sessionAttribute = {
        [key]: JSON.stringify(AdaptiveSessionAttributes)
      };
      console.info("Session Attributes :", sessionAttribute);
      console.info("Lex Attributes :", lexAttributes);
      lexAttributes = { ...lexAttributes, ...sessionAttribute };
      console.info("Lex Attributes after merge :", lexAttributes);
      this.$store.commit("setLexSessionAttributes", lexAttributes);

      const message = {
        type: "human",
        text: event._processedData.dispMsg
      };

      this.$store.dispatch("postTextMessage", message);
    }
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
  }
};
</script>

<style></style>
