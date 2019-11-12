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
        <label>{{builder_status}}</label>
        <v-textarea label="ここに文章を入力" v-model="inputText"></v-textarea>
        <v-btn @click="conv">変換</v-btn>
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">表層系</th>
                <th class="text-left">基本形</th>
                <th class="text-left">品詞</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item,index) in outputToken" :key="index">
                <td>{{ item.surface_form }}</td>
                <td>{{ item.basic_form }}</td>
                <td>{{ item.pos }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
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
      builder: "",
      builder_status: false
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
  },
  mounted: function() {
    this.builder = kuromoji.builder({ dicPath: "/dict" });
    this.builder_status = true;
  }
};
</script>
