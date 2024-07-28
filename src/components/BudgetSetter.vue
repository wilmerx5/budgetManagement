<script setup>
import { ref } from 'vue';
import Alert from './Alert.vue';

const budget= ref(0)
const error=ref('');


const emit= defineEmits(['set-budget'])

const saveBudget=()=>{

  if(validate(budget.value)){

    emit('set-budget',budget.value)
  }
}

const validate=(budget)=>{

    if(budget>=1){
        return true
    }
    else{


        error.value="Amount invalid"
        setTimeout(() => {
            error.value=''
        }, 3000);
        return false
    }

}
</script>
<template>

    <form @submit.prevent="saveBudget" class="w-full budget " >
        <div class="field ">
            <label for="budget" class="text-indigo-500" >
                <Alert v-if="error">
                    {{ error }}
                </Alert>
                Set your budget
            </label>
            <input 
            v-model="budget"
            type="number" id="budget" placeholder="Add your budget" class="" />
            <input type="submit" value="set budget"
                class="text-3xl  bg-indigo-600 w-full text-white border-0 p3 font-bold " />
        </div>
    </form>



</template>

<style scoped>
.field {
    display: grid;
    gap: 2rem;
}

.budget input[type="number"] {
    background-color: var(---light-gray);
    border-radius: 1rem;
    padding: 1rem;
    border: none;
    outline: var(--blue) 1px;
    font-size: 2.2rem;
    text-align: center;
}

.budget input[type="submit"] {
    background-color: var(--blue);
    text-transform: uppercase;
    padding: 1rem;
    border: none;
    text-align: center;
    font-size: 2rem;
    margin-top: 2rem;
    font-weight: 900;
    transition: background-color 500ms ease;
}
.budget input[type="submit"]:hover{
    background-color: #1048A4;
    cursor: pointer;
} 
.budget label{
text-align: center;
font-size: 2.8rem;
}
</style>