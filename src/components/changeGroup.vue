<template>
    <div class="modal">
        <p>{{message}}</p>
        <select class="" v-model="groupSelector">
        <option v-for="group in groups" :key="group"  :value="group">{{group}}</option> 
        </select>
        <input type="text" placeholder="title search" v-model="enterGroup">
        <button class="btn confirm" @click="change(group)">Ok</button>
        <button class="btn cancel" @click="cancel()">Cancel</button>
    </div>
    
</template>

<script>
import { EventBus } from '../eventBus'
export default {
    name: "changeGroup",
    data: function(){
        return {
          enterGroup: ""
           
        }
    },
    props: ['value', 'message', 'groups', 'group']
        
    ,
     methods: {
    change(){
        EventBus.$emit('change', true)
    },
    cancel(){
        EventBus.$emit('cancel', true)
    }
    },
    computed: {
            groupSelector: {                
                get() {
                    return this.value
                },
                set(val) {
                    this.$emit('input', val)
                }              
            },
        
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
        background-color: aliceblue;
        margin: auto;
        top: 30%;
        left: 50%;
        margin-left: -25%;
        border: solid 1px darkgrey
    }
    .modal>*{
        padding: .3rem;
        margin: 0.1rem;
    }
  
</style>