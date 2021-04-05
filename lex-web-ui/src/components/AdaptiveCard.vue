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
import { AdaptiveCards } from 'adaptivecards-vue';
import * as aConfig from '../config/adaptive.card.template.json';

export default {
  name: 'adaptive-card',
  props: ['customPayload'],
  components: {
    AdaptiveCards,
  },
  data() {
    return {
      data: '',
      card: '',
      config: '',
    };
  },
  methods: {
    onItemClick(event, item) {
      console.info('item', item);
      console.info('event', event._processedData);
      const message = {
        type: 'human',
        text: JSON.stringify(event._processedData),
        event: event._processedData,
      };

      this.$store.dispatch('postTextMessage', message);
    },
  },
  created() {
    console.info('In adaptiveCards :', this.customPayload);
    console.info('Adavtive jsons', aConfig);
    const acard = aConfig.filter(ele => ele.id === this.customPayload.customPayload.id)[0];
    console.info('acard :', acard);
    this.data = acard.data;
    this.card = acard.card;
    this.config = acard.config;
  },
};
</script>

<style>

</style>