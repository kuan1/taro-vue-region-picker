<template>
  <view class="index">
    <region-picker v-model="value" @change="(_, name) => (regionName = name)">
      <button type="primary">选择省市</button>
    </region-picker>
    <view>{{ value.join("、") }}</view>
    <text>{{ regionName }}</text>
  </view>
</template>

<script>
import RegionPicker from "../../components/region-picker";

export default {
  components: {
    RegionPicker,
  },
  data() {
    return {
      value: [],
      regionName: "",
    };
  },
};
</script>
