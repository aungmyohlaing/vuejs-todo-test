<template>

    <section>
        <form @submit.prevent="addToDo">

            <div>
                <label id="input__label" for="to_do_input">Add to Do List</label>
                <input type="text" id="to_do_input" v-model="enteredText">
            </div>

            <p class="warning__text" v-if="invalidInput">Please enter something.</p>

            <button class="add__btn">Add todo</button>           


        </form>
    </section>
    
</template>

<script>

import { ref } from "vue";

export default {
    emits:["add-to-do"],

    setup(props,context) {
        const enteredText = ref("");
        const invalidInput = ref(false);


        function addToDo(){
            invalidInput.value = false;
            if(enteredText.value ===""){
                invalidInput.value = true;
                return;
            }

            context.emit("add-to-do", enteredText.value);
            enteredText.value = "" ;
        }


        return{
            enteredText,
            invalidInput,
            addToDo
        }


    }
}


</script>

<style scoped>
form {
  text-align: center;
}

.warning__text{
    color: red;
    font-size: large;
}

#input__label {
  display: block;
  font-weight: bold;
  margin-bottom: 0.5rem;
  font-size: larger;
  
}

#to_do_input {
  display: block;
  font-weight: bold;
  width: 50%;
  margin: auto;
  padding: 10px;  
  margin-bottom: 0.5rem;
}

.add__btn{
    width:100px;
    height: 40px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 8px;;
    cursor: pointer;
}
</style>