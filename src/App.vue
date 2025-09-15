<script setup>
import { ref } from 'vue'
import Lineal from '../src/components/Lineal.vue'
import Multiplicativo from '../src/components/Multiplicativo.vue'

const algs = ref(null)

</script>

<template>
  <v-app class="bg">
    <v-main>
      <v-container>
        <v-row class="mt-8">
          <v-col cols="12">
            <p class="title">Generador de Números Pseudoaleatorios</p>
          </v-col>
        </v-row>
        <v-row>
          <v-container class="container">
            <v-tabs v-model="algs">
              <v-tab class="tab" value="lineal" :class="{'reversed-tab': algs === 'lineal'}">Algoritmo Lineal</v-tab>
              <v-tab class="tab" value="multiplicativo" :class="{'reversed-tab': algs === 'multiplicativo'}">Algoritmo Multiplicativo</v-tab>
            </v-tabs>
            <v-tabs-window v-model="algs">
              <v-tabs-window-item value="lineal">
                <Lineal/>
              </v-tabs-window-item>
              <v-tabs-window-item value="multiplicativo">
                <Multiplicativo/>
              </v-tabs-window-item>
            </v-tabs-window>
          </v-container>
        </v-row>
      </v-container>
    </v-main>
    <v-footer class="footer order-last">
      <div class="flex-1-0-100 text-center mt-2">
        {{ new Date().getFullYear() }} — <strong>Alina Mollinedo Dávila</strong>
      </div>
    </v-footer>
  </v-app>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Sour+Gummy:ital,wght@0,100..900;1,100..900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Patrick+Hand&family=Sour+Gummy:ital,wght@0,100..900;1,100..900&display=swap');

html, body {
  height: 100%;
  margin: 0;
}

.v-main{
  flex-direction: column;
}

.bg{
  background: linear-gradient(0deg, rgb(196, 73, 0, 0.15), rgb(196, 73, 0, 0.1));
}

.v-container{
  flex: 1;
}

.title {
  font-family: 'Sour Gummy', sans-serif;
  font-size:xx-large;
  font-weight: 700;
  color: #04151F;
}

.v-tabs{
  border-bottom: 5px solid #183A37;
}

.tab{
  font-family: 'Patrick Hand', cursive;
  font-weight: 500;
  font-size: larger;
  position: relative;
  transition: color 1s ease;
  background-color: transparent;
  color: #183A37;
}

.reversed-tab{
  background-color: #183A37;
  color: #f8eedd;
  top: 100%;
  animation: fillUp 0.3s ease-out forwards;
}

@keyframes fillUp {
	10% {
		top: 100%;
	}
	100% {
		top: 0%;
	}
}

.footer{
  background-color: #183A37;
  color: #f8eedd;
  width: 100%;
  max-height: 100px;
  padding: 10px 0;
}

</style>
