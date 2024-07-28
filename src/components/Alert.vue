<template >
    <div class="alert">
<slot></slot>
    </div>
</template>

<style scoped>

.alert{
    padding: 1rem;
    font-weight: 900;
    text-transform: uppercase;
    font-size:2.6rem;
    text-align: center;
    color:red;
    background-color: var(--white);
    border-left: 5px solid red;
}
</style>