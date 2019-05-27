<template>
  <div>
    <b-container fluid>
      <b-row>
        <b-col sm="5" class="bcol">
          <label :for="ccuConfigId">{{title}}</label>
        </b-col>
        <b-col :sm="unit? 6 : 7" class="bcol">
          <b-form-input
            :id="ccuConfigId"
            v-model="computedValue"
            @change="inputHasChanged"
          ></b-form-input>
        </b-col>
        <b-col sm="1" v-if="unit" class="bcol">{{unit}}</b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { eventBus, ccuConfig } from "../main";

export default {
  name: "default-input",
  data() {
    return {
      ccuConfig,
      pleaseUpDate: false,
    };
  },
  props: ["ccuConfigId", "title", "unit"],
  computed: {
    computedValue() {
      if (this.pleaseUpDate) {
        // eslint-disable-next-line
        this.pleaseUpDate = false;
        // eslint-disable-next-line
        console.log(this.pleaseUpDate);
      }
      // eslint-disable-next-line
      // console.log(
      //   "this is in the function computed " + ccuConfig[this.ccuConfigId]
      // );
      return ccuConfig[this.ccuConfigId];
    },
  },
  methods: {
    inputHasChanged(value) {
      const newValue = value;
      const n = newValue.length;
      // eslint-disable-next-line
      console.log(n);
      if (n <= 5) {
        eventBus.dataChanged({ id: this.ccuConfigId, contents: newValue });
      } else {
        this.pleaseUpDate = true;
        eventBus.dataChanged({
          id: this.ccuConfigId,
          contents: `${ccuConfig[this.ccuConfigId]}`,
        });
        // eslint-disable-next-line
        console.log(ccuConfig[this.ccuConfigId]);
      }
    },
  },
};
</script>

<style scoped>
.bcol {
  padding: 2px;
}
</style>
