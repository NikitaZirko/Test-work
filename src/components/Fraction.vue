<script>
import useValidate from '@vuelidate/core'
import { integer, between } from '@vuelidate/validators'
export default {
    data: () => ({
        v$: useValidate(),
        top: null,
        bot: null,
    }),
    methods: {
        result() {
            this.v$.$validate()

            if (!this.v$.$error) {
                let res = this.top / this.bot;
                res = Math.round(res * 10) / 10;
                res = Number.isFinite(res) ? res : null;
                this.$emit('listen', res);
            }
        }
    },
    validations () {
        return {
            top: {  
                between: between(1, 99),
                integer,
            },
            bot: {  
                between: between(1, 99),
                integer,
            },
        }
    }
}   
</script>

<template lang="pug">

slot
input.inp(
    v-model.trim="top" 
    @input="result" 
    :class="{'err': v$.top.$error}")

span(
    v-if="v$.top.$error" 
    class="absolute w-max top-[37%] text-xs text-red-400") {{ v$.top.$errors[0].$message }}
    
hr.w-full

input.inp(
    v-model.trim="bot" 
    @input="result" 
    :class="{'err': v$.bot.$error}")

span(
    v-if="v$.bot.$error" 
    class="absolute w-max bottom-0 text-xs text-red-400") {{ v$.bot.$errors[0].$message }}

</template>

<style scoped>
.err {
    animation-name: shakeError;
    animation-fill-mode: forwards;
    animation-duration: .6s;
    animation-timing-function: ease-in-out; 
}

@keyframes shakeError {
  0% {
    transform: translateX(0); }
  15% {
    transform: translateX(0.375rem); }
  30% {
    transform: translateX(-0.375rem); }
  45% {
    transform: translateX(0.375rem); }
  60% {
    transform: translateX(-0.375rem); }
  75% {
    transform: translateX(0.375rem); }
  90% {
    transform: translateX(-0.375rem); }
  100% {
    transform: translateX(0); } 
}
</style>
