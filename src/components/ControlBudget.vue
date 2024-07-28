<script setup>

import { computed } from 'vue';
import CircleProgress from 'vue3-circle-progress';
import "vue3-circle-progress/dist/circle-progress.css";
import { formatMoney } from '../utils';

defineEmits(['reset-app'])

const props= defineProps({
    budget:{
        type:Number,
        required:true
    },
    availableAmount:{
        type:Number,
        required:true
    },
    spend:{
        type:Number,
        required:true
    },

})

const percent= computed(()=>{
    return parseInt(((props.budget-props.availableAmount)/props.budget)*100)
})
</script>
<template>
    <div class="two-column">

        <div class="container-chart">

            <p class="percent">{{ percent }} %</p>
            <CircleProgress 
            :percent="percent">
               
           </CircleProgress>
        </div>
        <div class="container-budget  w-full">

                <button
                class="text-white font-bold uppercase text-3xl rounded-xl bg-red-500 border-none p-4 w-full hover:cursor-pointer hover:bg-red-800 " 
                @click="$emit('reset-app')"
                >
                    Reset app
                </button>
                <p><span>BUDGET </span>  {{ formatMoney(budget) }}</p>
                <p><span>AMOUNT AVAILABLE </span> {{ formatMoney(availableAmount)}}</p>
                <p><span>SPENT </span> {{ formatMoney(spend) }}</p>


        </div>
    </div>

</template>

<style scoped>

.chart{
    position: relative;
}
.percent{
    position: absolute;
    z-index: 10;
    top: 40%;
    left: 15%;
    font-weight: 900;
    text-align: center;
    font-size: 3vw;
}
@media (max-width:768px) {
    .percent{
   
    top: 15%;
    left: 45%;

}
}
button{
    transition: background-color 500ms;
    margin-bottom: 10px;
}
.two-column{
    display: flex;
    flex-direction: column;
    gap: 4rem;
    align-items: center;
}
.container-budget p {
    font-size: 2.4rem;
    text-align: center;
    padding-top: 10px;
    color: var(--dark-gray);
}
.container-budget span{
    font-weight: 900;
    color: var(--blue);
}
@media (min-width:768px) {
    .two-column{
        flex-direction: row;
    }
    .container-budget p {
    font-size: 2.1rem;
    text-align: left;
}
}

</style>
