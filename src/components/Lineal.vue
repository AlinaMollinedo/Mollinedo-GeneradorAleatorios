<script setup>
import { ref } from 'vue'

const isValid = ref(false)
const submitted = ref(false)

const P = ref(null)
const x0 = ref(null)
const k = ref(null)
const c = ref(null)
const g = ref(null)
const m = ref(null)
const a = ref(null)

const xi = ref([])
const ri = ref([])

const Pcopy = ref(null)
const range = (n) => Array.from({ length: n }, (_, i) => i + 1)
const digits = ref(3)

const calc = () => {
    submitted.value = true

    P.value = parseInt(P.value)
    x0.value = parseInt(x0.value)
    k.value = parseInt(k.value)
    c.value = parseInt(c.value)

    g.value = Math.ceil(Math.log10(P.value) / Math.log10(2))
    m.value = Math.pow(2, g.value)
    a.value = 1 + 4*k.value

    xi.value = []
    ri.value = []
    xi.value.push(x0.value)

    for(let i = 1; i <= P.value; i++){
        xi.value.push((a.value * xi.value.at(i - 1) + c.value) % m.value)
        ri.value.push(xi.value.at(i) / (m.value - 1))
    }

    Pcopy.value = P.value
}

const clear = () => {
    isValid.value = false
    submitted.value = false

    P.value = null
    x0.value = null
    k.value = null
    c.value = null
    g.value = null
    m.value = null
    a.value = null

    xi.value = []
    ri.value = []
}

const lessDigits = () => {
    digits.value--
}

const moreDigits = () => {
    digits.value++
}

</script>

<template>
    <v-form v-model="isValid" @submit.prevent="calc()">
        <v-row class="mt-5">
            <v-col cols="12" sm="3">
                <v-text-field
                class="input"
                v-model="P"
                label="Periodo"
                type="number"
                :rules="[
                    v => !!v || 'Se requiere un periodo',
                    v => (Number.isInteger(parseFloat(v)) && v > 0) || 'El periodo debe ser un número entero positivo'
                ]"
                required
                />      
            </v-col>
            <v-col cols="12" sm="3">
                <v-text-field
                class="input"
                v-model="x0"
                label="Semilla (X₀)"
                type="number"
                :rules="[
                    v => !!v || 'Se requiere una semilla',
                    v => (Number.isInteger(parseFloat(v)) && v > 0) || 'La semilla debe ser un número entero positivo'
                ]"
                required
                />  
            </v-col>
            <v-col cols="12" sm="3">
                <v-text-field
                class="input"
                v-model="k"
                label="k"
                type="number"
                :rules="[
                    v => !!v || 'Se requiere la constante k',
                    v => (Number.isInteger(parseFloat(v)) && v > 0) || 'La constante k debe ser un número entero positivo'
                ]"
                required
                /> 
            </v-col>
            <v-col cols="12" sm="3">
                <v-text-field
                class="input"
                v-model="c"
                label="c"
                type="number"
                :rules="[
                    v => !!v || 'Se requiere la constante c',
                    v => (Number.isInteger(parseFloat(v)) && v > 0) || 'La constante c debe ser un número entero positivo',
                    v => (v > 1 && Array.from({ length: v - 2 }, (_, i) => i + 2).every(i => v % i !== 0)) || 'La constante c debe ser un número primo'
                ]"
                required
                /> 
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="6">
                <v-btn
                class="btn"
                @click="calc"
                :disabled="!isValid"
                block>
                    Generar
                </v-btn>
            </v-col>
            <v-col cols="6">
                <v-btn
                class="btn"
                @click="clear"
                :disabled="!submitted"
                block>
                    Limpiar
                </v-btn>
            </v-col>
        </v-row>
    </v-form>
    <v-row class="my-8 justify-center" v-if="submitted">
        <v-col cols="12" sm="3">
            <v-card>
                <v-card-text class="card-title">Constantes calculadas</v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" sm="3">
            <v-card>
                <v-card-text class="card-text text-center">g = {{ g }}</v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" sm="3">
            <v-card>
                <v-card-text class="card-text text-center">m = {{ m }}</v-card-text>
            </v-card>
        </v-col>
        <v-col cols="12" sm="3">
            <v-card>
                <v-card-text class="card-text text-center">a = {{ a }}</v-card-text>
            </v-card>
        </v-col>
    </v-row>
    <v-row class="px-2 pb-12" v-if="submitted">
        <v-col cols="12">
            <v-table class="elevation-1" striped="even">
                <thead>
                    <tr>
                        <th class="header">i</th>
                        <th class="header">Operación</th>
                        <th class="header">X<sub>i</sub></th>
                        <th class="header">
                            r<sub>i</sub>
                            <v-btn
                            class="ml-3"
                            icon
                            size="30"
                            color="#183A37"
                            @click="lessDigits"
                            >
                            <v-icon size="20">mdi-chevron-left</v-icon>
                            </v-btn>
                            <v-btn
                            class="ml-2"
                            icon
                            size="30"
                            color="#183A37"
                            @click="moreDigits"
                            >
                            <v-icon size="20">mdi-chevron-right</v-icon>
                            </v-btn>
                        </th>
                    </tr>
                </thead>
                <tbody>
                    <tr
                    v-for="i in range(Pcopy)"
                    :key="i"
                    >
                        <td class="table-content">{{ i }}</td>
                        <td class="table-content">({{ a }} * {{ xi.at(i - 1) }} + {{ c }}) MOD({{ m }})</td>
                        <td class="table-content">{{ xi.at(i) }}</td>
                        <td class="table-content">{{ ri.at(i - 1).toFixed(digits) }}</td>
                    </tr>
                </tbody>
            </v-table>
        </v-col>
    </v-row>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Funnel+Display:wght@300..800&display=swap');

.input{
    font-family: 'Funnel Display', sans-serif;
    font-size: large;
    color: #183A37;
}

.btn{
    font-family: 'Funnel Display', sans-serif;
    font-size: larger;
    background-color: transparent;
    color: #432534;
    border: 2px solid #432534;
    transition: all 0.5s ease;
}

.btn:disabled{
    background-color: transparent;
    color: rgb(67, 37, 52, 0.4);
    border: 2px solid rgb(67, 37, 52, 0.4);
}

.btn:hover{
    background-color: #432534;
    color: #f8eedd;
}

.v-card{
    background-color: rgb(255, 242, 235, 1);
}

.card-title{
    font-family: 'Funnel Display', sans-serif;
    font-weight: 600;
    font-size: larger;
}

.card-text{
    font-family: 'Funnel Display', sans-serif;
    font-weight: 400;
    font-size: large;
}

.v-table{
    background-color: #fff2eb;
    border-radius: 10px;
}

.header{
    font-family: 'Funnel Display', sans-serif;
    font-size: large;
    background-color: #183A37;
    color: #f8eedd;
}

.table-content{
    font-family: 'Funnel Display', sans-serif;
    font-weight: 300;
    font-size: medium;
}

</style>