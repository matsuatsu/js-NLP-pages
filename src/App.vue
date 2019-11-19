<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />
      </div>
      <h1>js-NLP-pages</h1>

      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content>
      <v-container>
        <v-textarea label="ここに文章を入力" v-model="inputText"></v-textarea>
        <v-btn @click="conv">変換</v-btn>
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">形態素</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in outputToken" :key="index">
                <td>{{ item }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
/* eslint-disable no-console */
import TinySegmenter from "tiny-segmenter";

export default {
  name: "App",
  data() {
    return {
      inputText: "",
      outputToken: [],
      segmenter: ""
    };
  },
  methods: {
    conv: async function() {
      let segs = this.segmenter.segment(this.inputText);
      this.outputToken = segs;

      // var vm = this;
      // this.builder.build(
      //   await function(err, tokenizer) {
      //     if (err) {
      //       throw err;
      //     } else {
      //       var token = tokenizer.tokenize(vm.inputText);
      //       vm.outputToken = token;
      //     }
      //   }
      // );
    }
  },
  mounted: function() {
    this.segmenter = new TinySegmenter();
  }
};
</script>
