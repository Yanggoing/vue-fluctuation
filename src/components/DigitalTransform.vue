<template>
  <transition-group class="digital-transform" name="vdt-slide-y" tag="div">
    <DigitalTransfromScroll
      class="digital-transform-item"
      v-for="(item, i) in digitals"
      :key="i"
      :to="item"
      :interval="interval"
      :dislocation="dislocation"
      from="0"
    >{{ item }}
    </DigitalTransfromScroll>
  </transition-group>
</template>

<script>
import { looseDigitalValidator } from './validator';
import DigitalTransfromScroll from './DigitalTransfromScroll';

export default {
  name: 'DigitalTransfrom',
  components: {
    DigitalTransfromScroll,
  },
  props: {
    value: {
      validator: looseDigitalValidator,
      default: undefined,
      required: true,
    },
    dislocation: {
      type: Boolean,
      default: false,
    },
    interval: {
      type: Number,
      default: 500,
    },
    useGrouping: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      digitals: [],
      oldDigtals: [],
    };
  },
  watch: {
    value: {
      immediate: true,
      handler(value) {
        this.parseDigital(value);
      },
    },
  },
  methods: {
    parseDigital(digitals) {
      let digitalsStr = `${digitals}`;
      if (this.useGrouping) {
        digitalsStr = digitalsStr.replace(/^-?\d+/g, (m) => m.replace(/(?=(?!\b)(\d{3})+$)/g, ','));
      }
      const digitalArr = digitalsStr.split('');
      this.oldDigtals = this.digitals.concat();
      this.digitals = digitalArr;
    },
  },
};
</script>

<style lang="stylus" scoped>
  .digital-transform
    display inline-flex

  .digital-transform-item
    display inline-block;
    transition opacity 0.3s, transform 0.3s;

  .vdt-slide-y-enter,
  .vdt-slide-y-leave-to
    opacity 0;
    transform translateY(10px);
</style>
