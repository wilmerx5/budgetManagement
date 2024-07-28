<script setup>

import { computed, ref } from 'vue';
import closeModal from '../assets/img/cerrar.svg';
import Alert from './Alert.vue';

const error = ref('')
const props = defineProps({
    modal: {
        type: Object,
        required: true
    },
    spent: {
        type: String,
        required: true
    }
    ,
    amount: {
        type: [String, Number],
        required: true
    },
    category: {
        type: String,
        required: true
    },
    availableAmount: {
        type: [String, Number],
        required: true
    },
    id: {
        type: [String, null],
        required: true
    }
})
const emit = defineEmits(['update:spent', 'update:amount', 'update:category', 'hide-modal', 'save-spend','delete-spend'])

const addSpent = () => {

    if (validate()) {
        emit('save-spend')
    }

}


const old = props.amount
const validate = () => {

    const { amount, category, spent, availableAmount, id } = props

    if (id) {
        if (amount > old + availableAmount) {
            error.value = 'Invalid Amount'
            setTimeout(() => {
                error.value = ''
            }, 3000);

            return false
        }
        return true
    } else {
        if ([amount, category, spent].includes('')) {

            error.value = 'Complete all the fields'
            setTimeout(() => {
                error.value = ''
            }, 3000);
            return false
        }
        else if (amount <= 0) {
            error.value = 'Invalid Amount'
            setTimeout(() => {
                error.value = ''
            }, 3000);
        }
        else if (availableAmount < amount) {
            error.value = 'YOU HAVE REACHED YOUR BUDGET'
            setTimeout(() => {
                error.value = ''
            }, 3000);
        }
        else {
            return true
        }
    }


}

const isEditing=computed(()=>{

    return props.id
})
</script>


<template>
    <div class="modal">
        <div class="close-modal">
            <img @click="$emit('hide-modal')" :src="closeModal" />

        </div>
        <div :class="[modal.animate ? 'animate' : 'close']" class="container container-form">

            <form @submit.prevent="addSpent" class="new-spent">
                <Alert v-if="error">
                    <h2>{{ error }}</h2>
                </Alert>
                <legend>{{isEditing?'UPDATE':'ADD SPENT'}}</legend>
                <div class="field">
                    <label for="spent">Spent</label>
                    <input @input="$emit('update:spent', $event.target.value)" :value="spent" type="text " id="spent"
                        placeholder="spent">

                </div>

                <div class="field">

                    <label for="Amount">Amount</label>
                    <input @input="$emit('update:amount', +$event.target.value)" :value="amount" type="text "
                        id="Amount" placeholder="Amount">

                </div>
                <div class="field">
                    <label for="category">category</label>
                    <select :value="category" @input="$emit('update:category', $event.target.value)" type="text "
                        id="category" placeholder="category">

                        <option value=""> --select </option>
                        <option value="saving"> SAVING </option>
                        <option value="food"> FOOD </option>
                        <option value="house"> HOUSE </option>
                        <option value="other"> OTHER </option>
                        <option value="leisure"> LEISURE </option>
                        <option value="health"> HEALTH </option>
                        <option value="subscriptions"> SUBSCRIPTIONS </option>

                    </select>

                </div>

                <input :value="[isEditing?'UPDATE':'ADD']" type="submit">
                <button type="button"
                @click="$emit('delete-spend',id)"
                v-if="isEditing"
                class="btn-delete" >DELETE</button>

            </form>

        </div>
    </div>
</template>

<style scoped>
.container-form {
    transition-property: all;
    transition-duration: 300;
    transition-timing-function: ease-in;
    opacity: 0;

}

.container-form.animate {
    opacity: 1;
}

.container-form.close {
    opacity: 0;
}

.modal {
    position: fixed;
    background-color: rgba(0, 0, 0, .9);
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;


}

.close-modal {
    position: absolute;
    right: 3rem;
    top: 3rem;
}

.close-modal img {
    width: 3rem;
    cursor: pointer;
}

.new-spent {
    margin:  auto 0 auto;
    display: grid;
    gap: 2rem;
}

.field {
    gap: 2rem;
    display: grid;
}

.new-spent legend {
    text-align: center;
    color: var(--white);
    font-size: 3rem;
    font-weight: 700;
}

.new-spent label {
    color: var(--white);
    font-size: 3rem;

}

.new-spent select,
.new-spent input {
    background-color: var(---light-gray);
    outline: none;
    border-radius: 1rem;
    font-size: 2.2rem;
    padding: 1rem;

}

.new-spent input[type="submit"] {
    font-weight: 900;
    cursor: pointer;
    background-color: var(--blue);
    text-transform: uppercase;
    color: var(--white);
}
.btn-delete{
    padding: 1rem;
    width: 100%;
    background-color: #ef444e;
    font-weight: 700;
    color: var(--white);
    cursor: pointer;
}
</style>