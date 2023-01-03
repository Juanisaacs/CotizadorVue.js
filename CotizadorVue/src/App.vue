<script setup>
    import {ref, computed, watch} from "vue"
    import Header from "./components/Header.vue"
    import Button from "./components/Button.vue"
    import {calcularTotal} from "./utils"
    
    const cantidad = ref(10000);
    const meses = ref(6)
    const total = ref(0)
    const MIN= 0;
    const MAX= 20000;
    const STEP= 100;

    const FormatearDinero =(valor)=>{
        const formatter = new Intl.NumberFormat('es-ES',{
            style:'currency',
            currency:'EUR'
        })
        return formatter.format(valor)
    };
    watch([cantidad, meses], ()=>{
        total.value = calcularTotal(cantidad.value, meses.value)
    })
    const pagoMensual = computed (() => { 
        return total.value / meses.value;
    })
    const handletChangeDecremento = () => {
        const valor = cantidad.value - STEP
        if( valor < MIN ) {
            alert("La cantidad no es valida")
            return;
        }
        cantidad.value = valor;
    }
    const handletChangeIncremento= () => {
        const valor = cantidad.value + STEP
        if( valor > MAX ) {
            alert("La cantidad no es valida")
            return;
        }
        cantidad.value = valor;
    }
</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header />
    <div class="flex justify-between mt-10">
        <Button 
        :operador ="'-'"
        @fn="handletChangeDecremento"
        />
        <Button 
        :operador ="'+'"
        @fn="handletChangeIncremento"
        />

    </div>
    <div class="my-5">
        <input 
        type="range"
        class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
        :min="MIN"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
        />
        <p class="text-center my-10 text-5xl font-extrabold text-indigo-600">{{ FormatearDinero(cantidad)}}</p>
        <h2 class="text-2xl font-extrambold text-gray-500 text-center" >
                Elige un <span>Plazo</span> a pagar
        </h2>

        <select class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl
        font-bold text-gray-500 mt-5"
        :value="meses"
        v-model.number="meses">
            <option value="6">6 meses</option>
            <option value="12">12 meses</option>
            <option value="24">24 meses</option>
        </select>
    </div>
    <div v-if="total > 0" class="my-5 space-y-3 bg-gray-50 p-5">
        <h2 class=" text-2xl font-strabold text-gray-500 text-center">
            Resumen <span class="text-indigo-600">de pagos</span>
        </h2>
        <p class="text-xl text-gray-500 text-center font-bold">{{meses}} Meses</p>
        <p class="text-xl text-gray-500 text-center font-bold">Total a pagar : {{FormatearDinero( total ) }}</p>
        <p class="text-xl text-gray-500 text-center font-bold">Mensualidades : {{ FormatearDinero(pagoMensual) }}</p>

    </div>
    <p v-else class="text-center"> Añade una cantidad y plazo</p>

  </div>

 
</template>
