<template>
  <picker
    mode="multiSelector"
    :range="range"
    range-key="name"
    @columnchange="columnchange"
    @change="change"
  >
    <slot />
  </picker>
</template>

<script>
import data from "./region.json";

export default {
  model: {
    event: "change",
  },
  props: {
    value: {
      type: Array,
      default: () => [],
    },
    data: {
      type: Array,
      default: () => data,
    },
  },
  data() {
    return {
      dataValue: [0, 0],
    };
  },
  computed: {
    province() {
      return this.data.filter((item) => !item.pid);
    },
    city() {
      const [provinceIndex] = this.dataValue;
      const { id: provinceId } = this.province[provinceIndex];
      return this.data.filter((item) => item.pid === provinceId);
    },
    range() {
      return [this.province, this.city];
    },
    selectTarget() {
      const [provinceIndex, cityIndex] = this.dataValue;
      return [this.province[provinceIndex], this.city[cityIndex]];
    },
    codeValue() {
      const [{ id: pid = "" } = {}, { id = "" } = {}] = this.selectTarget;
      return [pid, id];
    },
    regionName() {
      const [
        { name: provinceName = "" } = {},
        { name: cityName = "" } = {},
      ] = this.selectTarget;
      return [provinceName, cityName].filter((t) => t).join(" ");
    },
  },
  watch: {
    value: {
      immediate: true,
      handler(value = []) {
        const [pid, id] = value;
        const provinceIndex = this.province.findIndex(
          (item) => item.id === pid
        );
        const city = this.data.filter((item) => item.pid === pid);
        const cityIndex = city.findIndex((item) => item.id === id);
        const fixIndex = (id) => (id < 0 ? 0 : id);
        this.dataValue = [fixIndex(provinceIndex), fixIndex(cityIndex)];
      },
    },
  },
  methods: {
    columnchange(e) {
      const { column, value } = e.detail;
      if (column === 0) {
        this.dataValue = [value, 0];
      } else if (column === 1) {
        this.dataValue = [this.dataValue[0], value];
      }
      this.$emit("columnchange", this.codeValue, this.regionName);
    },
    change() {
      this.$emit("change", this.codeValue, this.regionName);
    },
  },
};
</script>

<style>
</style>
