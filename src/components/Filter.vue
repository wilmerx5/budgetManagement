<script setup>

defineEmits('update:filter')
const props = defineProps({
    filter:{
        type:String,
        required:true
    }
})
</script>

<template>
    <div class="container shadow filters">
        <form>

            <div class="field">
                <label for="filter">category</label>
                    <select :value="filter" @input="$emit('update:filter', $event.target.value)" type="text "
                        id="filter" placeholder="category">

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
        </form>
    </div>
</template>

<style scoped>
.filters{
    margin-top: 10rem;
}
.filters .field{
    display: flex;
    align-items: center;
    gap : 2rem;
}
.filters label{
    font-size: 3rem;
}
.filters select{
    flex:1;
    padding: 1rem;
    border: none;
    border-radius: 1rem;
    background-color:var(---light-gray);
    outline:none;
    text-align: center;
}


</style>