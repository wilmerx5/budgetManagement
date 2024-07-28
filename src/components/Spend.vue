<script setup>
    import IconSaving from '../assets/img/icono_ahorro.svg'
import IconHouse from '../assets/img/icono_casa.svg'
import IconFood from '../assets/img/icono_comida.svg'
import IconOther from '../assets/img/icono_gastos.svg'
import IconLeisure from '../assets/img/icono_ocio.svg'
import IConHealth from '../assets/img/icono_salud.svg'
import IconSubscriptions from '../assets/img/icono_suscripciones.svg'
import { formatDate, formatMoney } from '../utils'
    const dictionaryIcons = {
        saving : IconSaving,
        food : IconFood,
        house : IconHouse,
        other : IconOther,
        leisure : IconLeisure,
        health : IConHealth,
        subscriptions : IconSubscriptions
    }
const props = defineProps({
    spend: {
        type: Object,
        required: true
    }
})

defineEmits(['spend-selected'])
</script>

<template>
    <div class="spend shadow">
        <div class="content">
            <img
                :src="dictionaryIcons[spend.category]"
                :alt="icon-spend"
                class="icon"
                />
            <div class="details">
             
                <p class="category"> {{ spend.category }}</p>
                <p 
                @click="$emit('spend-selected',spend.id)"
                class="spent"> {{ spend.spent }}</p>
                <p class="date"> {{ formatDate(spend.date)}}</p>


            </div>
        </div>


        <p class="amount">
            {{ formatMoney(spend.amount) }}
        </p>
    </div>
</template>

<style scoped>

.spend{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 2rem;
    padding-top: 1rem;
    padding-bottom: 1rem;
    border-radius: 10px;
    
}
.content{
    flex-shrink: 1;
    display: flex;
    gap:2rem;
    align-items: center;

}
.icon{
    width: 5rem;
    margin-left: 1rem;
    
}
.details{

}
.category{
    color: var(--gray);
font-size: 1.2rem;
text-transform: uppercase;
font-weight: 900;
}
.spent{
color: var(--dark-gray);
font-size: 2.4rem;
font-weight: 700;
cursor: pointer;
}
.date{
font-size: 1.6rem;
font-weight: 800;
}
.amount{
    font-size: 2rem;
    font-weight: 900;
    padding-right: 1rem;
}
</style>