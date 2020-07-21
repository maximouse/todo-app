<template>
    <div class="modal">
        <p><span>{{message}}</span></p>
        <button class="btn confirm" @click="confirm()">Ok</button>
        <button class="btn cancel" @click="cancel()">Cancel</button>
    </div>
</template>

<script>
   import { EventBus } from '../eventBus'
   export default{
    name: "modal",
    props: {
        message: String, 
    },
    methods: {
    confirm(){
        EventBus.$emit('confirm', true)
    },
    cancel(){
        EventBus.$emit('cancel', true)
    }
}
   }

</script>
<style scoped>
   
    .modal{
        position: absolute;
        display: flex;
        flex-direction: column;
        width: 400px;
        padding: 20px;
        background-color: white;
        margin: auto;
        top: 30%;
        left: 50%;
        margin-left: -25%;
        filter: drop-shadow(1px 1px 1px grey);
        border: solid 1px darkgrey
    }
    .modal>*{
        padding: .3rem;
        margin: 0.1rem;
    }
    span{
        font-weight: bold;
    }
  
</style>