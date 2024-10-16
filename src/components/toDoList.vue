<template>


    <div v-if="loading">
            <p>Loading...</p>
    </div>

    <div v-else-if="error">
        {{ error }}

    </div>


    <div v-else>

        <add-new-to-do-list @add-to-do = "addToDo"> </add-new-to-do-list>

        <show-to-do :toDoLists = "toDoLists" @done-to-do="doneToDo" @remove-to-do="removeToDo"> </show-to-do>


       
    </div>

    
</template>

<script>

import {ref, computed, onMounted, watch} from 'vue';

import addNewToDoList from './addNewToDoList.vue';

import showToDo from './showTodo.vue';


export default {

    components:{
        addNewToDoList,
        showToDo
    },

    setup(){
        const toDoLists = ref([]);
        const apiResult = ref([]);
        const error = ref(null);
        const loading = ref(true);
        const toDoListFromLocalStorage = ref([]);


        const fetchData = async() => {
            try{
                const response = await fetch('https://jsonplaceholder.typicode.com/todos?_limit=5');
                if(!response.ok){
                    throw new Error('Network response was not ok');
                }
                apiResult.value = await response.json();  
                if(apiResult.value){
                    toDoLists.value = apiResult.value;
                } 
                
                console.log(apiResult.value);

                
            } catch (err){
                error.value = err.message;
            } finally{
                loading.value = false;
                console.log(apiResult.value);

            }
        }            


            async function retriveToDoListFromLocalStorage(){
            if(localStorage.getItem('toDoListFromLocalStorage')){
                toDoListFromLocalStorage.value = await JSON.parse(localStorage.getItem('toDoListFromLocalStorage'));
                toDoLists.value.concat(toDoListFromLocalStorage.value);
                toDoListFromLocalStorage.value.forEach( (localToDo)=> toDoLists.value.push(localToDo) );
                console.log(toDoLists.value);
            }
           
        } 


        function arrangeId(toDo,index){
            toDoLists.value[index].id = index + 1;
            
        } 


        onMounted( async ()=>
        {
            await fetchData();
            await retriveToDoListFromLocalStorage();
            toDoLists.value.forEach(arrangeId);
            console.log(apiResult.value);          
            console.log(toDoLists.value); 

        } );

        watch(toDoListFromLocalStorage, (newVal) => {
                localStorage.setItem('toDoListFromLocalStorage', JSON.stringify(newVal) );

        },{deep:true});


        const toDoListSize = computed(function(){

            return toDoLists.value.length;
        });
        

        function addToDo(text){
            let size = ++toDoListSize.value;
            console.log(size);


            const newToDo = {
                userId:1,
                id: size,
                title:text,
                completed:false  
            
            }

            toDoLists.value.push(newToDo);
            toDoListFromLocalStorage.value.push(newToDo);

        }

        function doneToDo(toDo){

            toDoLists.value[toDo.id-1].completed = !toDo.completed;
            console.log(toDoLists.value[toDo.id-1].completed);


        }

        function removeToDo(toDo){
            toDoLists.value = toDoLists.value.filter(
                (obj) => {
                    console.log(obj.id != toDo.id);
                    return obj.id != toDo.id} 
            );
            toDoListFromLocalStorage.value = toDoListFromLocalStorage.value.filter(
                (obj) => {
                    return obj.id != toDo.id;
                }
            );


            console.log(toDoLists.value);
            console.log("Size" + toDoListSize.value + " " + toDo.id);

        }

        return {        
        addToDo,
        toDoLists,
        doneToDo,
        removeToDo,
        error,
        loading,
        toDoListFromLocalStorage
        

    }


    }



  

}



</script>

<style>

</style>