<script setup>
import { computed, onMounted, reactive, ref, watch } from 'vue';
import iconAddSpent from './assets/img/nuevo-gasto.svg';
import BudgetSetter from './components/BudgetSetter.vue';
import ControlBudget from './components/ControlBudget.vue';
import Filter from './components/Filter.vue';
import ModalAddSpent from './components/ModalAddSpent.vue';
import { generateId } from './utils';

import Spend from './components/Spend.vue';


const budget = ref(0)
const Spends = ref([])
const spendTotal = ref(0)
const filterSelected = ref('')



const availableAmount = ref(0)
const modal = reactive({
  show: false,
  animate: false
})

const spent = reactive({
  spent: "",
  amount: "",
  category: "",
  id: null,
  date: Date.now()
})
watch(Spends, () => {
  let spend = Spends.value.reduce((vc, va) => vc + va.amount, 0)
  availableAmount.value = budget.value - spend
  spendTotal.value = spend

  saveLocalStorage()

}, {
  deep: true
})

onMounted(() => {
  const savedSpends = localStorage.getItem('spends')
  if (savedSpends) {
    Spends.value = JSON.parse(savedSpends)
    budget.value = JSON.parse(localStorage.getItem('budget'))
  }
})
watch(budget, () => {

  saveLocalStorage()
})



const saveLocalStorage = () => {
  localStorage.setItem('budget', JSON.stringify(budget.value))
  localStorage.setItem('spends', JSON.stringify(Spends.value))

}
const setBudget = (quantity) => {

  budget.value = quantity
  availableAmount.value = quantity
}
const resetApp = () => {
  budget.value = 0
  Spends.value = []
  spendTotal.value = 0
  availableAmount.value = 0
}
const showModal = () => {
  modal.show = true
  modal.animate = false


  setTimeout(() => {
    modal.animate = true
  }, 300)
}
const hideModal = () => {
  modal.animate = false
  setTimeout(() => {
    modal.show = false
  }, 300)

  Object.assign(spent, {
    spent: "",
    amount: "",
    category: "",
    id: null,
    date: Date.now()
  })
}

const selectSpend = (id) => {

  const spendSelected = Spends.value.filter(e => e.id == id)[0]

  showModal()

  Object.assign(spent, spendSelected)


}

const saveSpend = () => {

  //if id already exist the spend exists an the user are updating
  if (spent.id) {
    const index = Spends.value.findIndex(el => el.id == spent.id)
    Spends.value[index] = { ...spent }
  } else {
    Spends.value.push({
      ...spent, id: generateId()
    })
  }


  hideModal()

}

const deleteSpend = id => {
  console.log('su llegue')
  if (confirm('delete')) {

    Spends.value = Spends.value.filter(e => e.id != id)
    hideModal()
  }
}
const filterSpends = computed(() => {
  if (filterSelected.value) {
    return Spends.value.filter(e => e.category == filterSelected.value)
  }

  return Spends.value
})
</script>

<template>


  <header>
    <h1>Budget Management</h1>

    <div class="container-budget container shadow ">

      <BudgetSetter v-if="budget === 0" @set-budget="setBudget" />
      <ControlBudget
    
      :spend="spendTotal" @reset-app="resetApp" :budget="budget" :availableAmount="availableAmount"
        v-else />

    </div>
  </header>
  <main v-if="budget > 0">

    <Filter v-model:filter="filterSelected" v-if="Spends.length > 0"></Filter>
    <div class="list-spends container">
      <h2>{{ Spends.length > 0 ? 'Spends' : 'There are no spends' }}</h2>

      <Spend v-for="spent in filterSpends" @spend-selected="selectSpend" :key="spent.id" :spend="spent">

      </Spend>
    </div>

    <div class="create-spent">
      <img @click="showModal" :src="iconAddSpent" />
    </div>

    <ModalAddSpent @delete-spend="deleteSpend" :id="spent.id" :availableAmount="availableAmount" :modal="modal"
      v-model:spent="spent.spent" v-model:amount="spent.amount" v-model:category="spent.category"
      @hide-modal="hideModal" v-if="modal.show" @save-spend="saveSpend">

    </ModalAddSpent>
  </main>

</template>

<style>
:root {
  --blue: #3b82f6;
  --white: #FFF;
  ---light-gray: #f5f5f5;
  --gray: #94a3b8;
  --dark-gray: #64748b;
  --black: #000;
}

.fixed {
  overflow: hidden;
  height: 100vh;
}

html {
  font-size: 62%;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(---light-gray);
}

h1 {
  font-size: 4rem;
}

header {
  background-color: var(--blue);
}

header h1 {
  text-align: center;
  text-transform: uppercase;
  padding: 3rem 0;
  margin: 0;
  font-weight: bolder;
  color: var(--white);
}

.container {
  width: 90%;
  max-width: 80rem;
  border-radius: 10px;
  margin: 0 auto;
  padding: 2rem;
}

.container-budget {

  margin-top: -5rem;
  transform: translateY(5rem);
  padding: 5rem;
}

.shadow {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--white);
}

.create-spent {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
}

.create-spent img {
  width: 5rem;
  cursor: pointer;
}

.list-spends {
  margin-top: 3rem;
}

.list-spends h2 {
  font-weight: 900;
  font-size: 3rem;
  text-transform: uppercase;
  color: var(--gray);
}
</style>
