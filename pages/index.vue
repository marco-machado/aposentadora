<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card>
        <v-card-title
          class="break-word headline primary--text text-center"
        >Calculadora da Aposentadoria</v-card-title>
        <v-card-text>
          <p>Calcule com quantos anos você poderá se aposentar e qual porcentual do seu benefício receberá de acordo com as novas regras da aposentadoria.</p>
        </v-card-text>
        <v-stepper vertical v-model="step">
          <v-stepper-step :complete="step > 1" step="1">Sexo</v-stepper-step>
          <v-stepper-content step="1">
            <v-btn @click="onStep1('f')" color="primary" :small="xsOnly">Feminino</v-btn>
            <v-btn @click="onStep1('m')" color="primary" :small="xsOnly">Masculino</v-btn>
          </v-stepper-content>

          <v-stepper-step :complete="step > 2" step="2">Data de nascimento</v-stepper-step>
          <v-stepper-content step="2">
            <v-text-field
              :dense="xsOnly"
              outlined
              placeholder="dd/mm/yyyy"
              type="tel"
              v-mask="'##/##/####'"
              v-model="dataNascimento"
            />
            <v-btn @click="step = 3" color="primary" :small="xsOnly">Continuar</v-btn>
          </v-stepper-content>

          <v-stepper-step :complete="step > 3" step="3">Setor</v-stepper-step>
          <v-stepper-content step="3">
            <v-btn color="primary" disabled :small="xsOnly">Publico</v-btn>
            <v-btn @click="step = 4" color="primary" :small="xsOnly">Privado</v-btn>
          </v-stepper-content>

          <v-stepper-step :complete="step > 4" step="4">Profissão</v-stepper-step>
          <v-stepper-content step="4">
            <v-btn color="primary" disabled :small="xsOnly">Professor(a)</v-btn>
            <v-btn @click="step = 5" color="primary" :small="xsOnly">Trabalhador(a) urbano</v-btn>
            <v-btn color="primary" disabled :small="xsOnly">Trabalhador(a) rural</v-btn>
          </v-stepper-content>

          <v-stepper-step :complete="step > 5" step="5">Tempo de contribuição</v-stepper-step>
          <v-stepper-content step="5">
            <v-text-field
              :dense="xsOnly"
              label="Anos"
              max="99"
              min="0"
              outlined
              type="number"
              v-mask="'##'"
              v-model="anosContribuicao"
            />
            <v-text-field
              :dense="xsOnly"
              label="Meses"
              max="11"
              min="0"
              outlined
              type="number"
              v-mask="'##'"
              v-model="mesesContribuicao"
            />
            <v-btn @click="onComplete()" color="primary" :small="xsOnly">Continuar</v-btn>
          </v-stepper-content>

          <v-stepper-step step="6">Resultado</v-stepper-step>
          <v-stepper-content step="6">
            <v-alert type="error">NUNCA</v-alert>
          </v-stepper-content>
        </v-stepper>
      </v-card>
    </v-flex>
    <v-dialog content-class="red" fullscreen v-model="dialog">
      <v-container fill-height>
        <v-row align="center" justify="center">
          <div class="white--text">Poderá se aposentar em:</div>
        </v-row>
        <v-row align="center" justify="center">
          <div class="display-3 white--text">NUNCA!</div>
        </v-row>
        <v-row justify="center">
          <v-btn @click="onReset()" color="white" outlined text>Voltar ao início</v-btn>
        </v-row>
      </v-container>
    </v-dialog>
  </v-layout>
</template>

<script>
import { mask } from "ke-the-mask";

export default {
  name: "Home",
  directives: {
    mask
  },
  data() {
    return {
      xsOnly: this.$vuetify.breakpoint.xsOnly,
      dialog: false,
      step: 1,
      sexo: "",
      dataNascimento: "",
      anosContribuicao: "",
      mesesContribuicao: ""
    };
  },
  methods: {
    onStep1(sexo) {
      this.sexo = sexo;
      this.step++;
    },
    onComplete() {
      this.dialog = true;
    },
    onReset() {
      this.dialog = false;
      this.step = 1;
      this.sexo = "";
      this.dataNascimento = "";
      this.anosContribuicao = "";
      this.mesesContribuicao = "";
    }
  },
  head: () => ({
    title: "Calculadora"
  })
};
</script>

<style scoped>
.break-word {
  word-break: break-word;
}
</style>
