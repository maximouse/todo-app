<template>
    <div class="wrapper">
         <transition name="fade">
         <li>
        <span class="header" >
                <input type="checkbox"  v-model="task.completed"> 
                <h4 :class="{completed: task.completed}">{{task.title}}</h4>
                <button class="btn change" @click="$emit('change-group', task.id)">&#8633;</button>
                <button class="btn remove" @click="$emit('remove-task', task.id)">&times;</button>
        </span>
        <span class="text">
            <h5 :class="{completed: task.completed}">
                {{task.text}}
            </h5>
        </span>
    </li>    
         </transition>
    </div>
</template>

<script>
export default {
    name: "task",
    props: {
        task: Object,   
    }
}
</script>
<style scoped>
h4 {
 margin: .5rem; 
}
h5 {
  margin: 3px;
}
span {
    display: inline-grid;
    justify-content: space-between;
    margin: 0;
}
.completed{
  text-decoration: line-through;
}
.wrapper{
    display: inline-block;
}
li {
    width: 300px;
    height: 100%;
    display: grid;
    justify-content: space-between;
    /* background-color: aliceblue; */
    margin: 10px 10px;
    border-top: 1px solid grey;
    border-right: 1px dashed grey;
    border-bottom: 1px solid grey;
}
.header{
    width: 300px;
    height: 100%;
    display: inline-grid;
     grid-template-columns: 20px 230px 30px 30px;
    justify-content: space-between;
    margin: .5rem;
  }
a {
  color: #42b983;
}
input {
    margin: .5rem;
}
.btn{
     color: white;
    width: 20px;
    height: 20px;
    float: right;
    font-weight: bold;
    /* margin: .5rem; */
    border:none;
    margin-top: 5px;
    cursor: pointer;
    transition: transform .3s;
}
.change{
    background-color: green;
}
.btn:hover{
  transform: rotate(90deg)
}
.remove{
    background-color: crimson;
}

 .fade-enter-active, .fade-leave-active {
  transition: opacity .3s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active Ð´Ð¾ Ð²ÐµÑÑÐ¸Ð¸ 2.1.8 */ {
  opacity: 0;
}

</style>