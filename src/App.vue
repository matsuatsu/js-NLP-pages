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

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>
      <h1>js-NLP-pages</h1>

      <v-spacer></v-spacer>
    </v-app-bar>
    <v-content>
      <v-container>
        <v-textarea v-model="inputText"></v-textarea>
        <v-btn @click="conv">変換</v-btn>
        <p>outputToken is: {{ outputToken }}</p>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import kuromoji from "kuromoji";
export default {
  name: "App",
  data() {
    return {
      inputText: "形態素解析される文字列",
      outputToken: [],
      builder: kuromoji.builder({ dicPath: "/dict" })
    };
  },
  methods: {
    conv: async function() {
      var vm = this;
      this.builder.build(
        await function(err, tokenizer) {
          if (err) {
            throw err;
          } else {
            var token = tokenizer.tokenize(vm.inputText);
            vm.outputToken = token;
          }
        }
      );
    }
  }
};
</script>
