<template>
    <button class="v-button">
        <slot />
    </button>
</template>

<style lang='scss'>
    .v-button {
        cursor: pointer;
        border: none;
        padding: 10px 20px;
        background-color: #474747;
        color: white;
        transition: all .2s ease-in-out;

        &:hover { opacity: .8; }
    }
</style>