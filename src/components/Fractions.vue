<script>
import Fraction from './Fraction.vue'
export default {
    components: {
        Fraction,
    },
    data: () => ({
        fractions: [], 
    }),
    methods: {
        addFraction() {
            this.fractions.push({
                id: this.fractions.length + 1,
                summ: null,
            })
        },
        delFraction(i) {
            this.fractions.splice(i, 1)
        },
        setResultFraction(e, i) {
            this.fractions[i].summ = e
        },
        getSign(item) {
            return item === this.fractions.length ? '=' : '+'
        },
    },
    computed: {
        isBtnAddEnabled() {
            return this.fractions.length < 5
        },
        isBtnDelEnabled() {
            return this.fractions.length > 2
        },
        getSumm() {
            return this.fractions.reduce((total, fraction) => 
                total + fraction.summ
            , 0)
        },
    },
    mounted: function () {
        this.addFraction()
        this.addFraction()
    },
}   
</script>

<template lang="pug">

.flex
    template(v-for="(item, i) in fractions.length" :key="fractions[i].id")
    
        div(class="w-1/12 flex flex-col items-center relative")
            Fraction(v-on:listen="setResultFraction($event, i)")
                button(
                    v-if="isBtnDelEnabled" 
                    @click="delFraction(i)" 
                    type="button" 
                    class="btn del") &times;

        div(class="w-1/12 flex items-center justify-center")
            span(class="h-[28px]") {{ getSign(item) }}

    div(class="w-1/12 flex items-center justify-center") 
        span(class="h-[28px]") {{ getSumm }}

button(
    v-if="isBtnAddEnabled" 
    @click="addFraction()" 
    type="button" 
    class="btn add") &plus;

</template>
