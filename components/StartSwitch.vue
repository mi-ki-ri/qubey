<template>
    <div class="swiwrap">
        <button class="myBtn" @click="myStartStop">
            Qubey Jump
        </button>
    </div>
</template>

<script>
export default {
    methods:{
        myStartStop: function(){
            this.$emit('startStop');
        }
    }
}
</script>


<style>
.myBtn{
    padding: 50px 25px;
    width: 100%;
    height: auto;
    background-color: #fff;
    border-radius: 3px;
}
</style>
