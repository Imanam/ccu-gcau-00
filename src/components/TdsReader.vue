<template>
  <div>
    <!-- <b-form-file multiple :file-name-formatter="formatNames"></b-form-file> -->
    <b-form-file v-model= "file" v-on:change = "onFileChange"></b-form-file>
    <p>{{ counter2 }}</p>
    <p v-for="display in tdsData" v-bind:key="display">{{ display }}</p>
  </div>
</template>

<script>
/* eslint-disable */

import { tdsData } from "../global.js";

export default {
  name: "tdsReader",
  data() {
    return {
      counter2: "",
      // formatNames: null,
      file: null,
      tdsData:{
        "Check_AC"  : null,
        "Check_DEF_CE"   : null,
        "Check_SHUNT_SB"  : null,          
        "Combo_DEF_TDB"  : null,
      },

    };
  },
  // tdsData: [tdsData],
  methods: {
    onFileChange: function(event) {
      // eslint-disable-next-line
      console.log("coucou");
      const file = event.target.files[0];

      if (!file) {
        return false;
      }
      const n = file.name;
      const s = file.size;
      const t = file.type;
      this.counter2 = `file selected!! [${n} | ${s} | ${t} ]`;

      const reader = new FileReader();
      reader.readAsText(file);
      reader.onload = e => {
        let text = e.target.result;
        var lines = text.toString().split("\n");

        var pattLabel = /<Label>(.*)<\/Label>/i;
        var pattData = /<Donnee>(.*)<\/Donnee>/i;
        let label;
        let data;
        // var display = {};

        lines.forEach(line => {
          var resultLabel = line.match(pattLabel);
          if (resultLabel) {
            label = resultLabel[1];
            // eslint-disable-next-line
            console.log(label);
          }
          var resultData = line.match(pattData);
          if (resultData) {
            data = resultData[1];
            // eslint-disable-next-line
            console.log(data);
            tdsData[label] = data;
            //  tdsData.push(display);
            // eslint-disable-next-line
            console.log(tdsData);
            // this.tdsData.push(display);
          }
          // display = {};
        });
        // eslint-disable-next-line
        // console.log(tdsData);
        // eslint-disable-next-line
        // console.log(this.tdsData);
        // eslint-disable-next-line
        // console.log(tdsData[1].data);
        // eslint-disable-next-line
        // console.log(this.tdsData[1].data);
      };
      // reader.readAsText(file)
      // return this.tdsData;
    },
  },
};
</script>

