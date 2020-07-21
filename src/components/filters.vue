<template>
    <div>
    <select class="" v-model="filter">
        <!-- <option disabled value="">Select filter</option> -->
        <option value="all">All</option>
        <option value="completed">Completed</option>
        <option value="uncompleted">Uncomplited</option>
    </select>  
    </div>
    
</template>

<script>
    export default{
        name: "filters",
        props: ['value'],
        computed: {
            filter: {                
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