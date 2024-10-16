<template>

    <section class="container">
        <h2>To Do Lists</h2>

        <table class="todo__table">
            <thead>
                <tr>
                    <th>No</th>
                    <th>Title</th>
                    <th>Action</th>
                
                </tr>
            </thead>

            <tbody>
                <tr v-for="(toDo,index) in toDoLists" :key="index" :class="{ done:toDo.completed}">
                    <td>{{ ++index }}</td>
                    <td>{{ toDo.title }}</td>
                    <td> <input @change="doneToDo(toDo)" type="checkbox" :checked="toDo.completed" > <button @click="removeToDo(toDo)">Remove</button></td>             
                </tr>
            </tbody>
            
            
        </table>

       

    </section>
    
</template>

<script >


export default{

    props: ["toDoLists"],
    emits:["done-to-do","remove-to-do"],

    setup(props,context){        
        
        function doneToDo(toDo){
            context.emit("done-to-do",toDo);
        }

        function removeToDo(toDo){
            context.emit("remove-to-do",toDo);
        }

        return{
            doneToDo,
            removeToDo,
        }

    }

    
}

</script>

<style>
.done{
    text-decoration: line-through;
    text-decoration-thickness: 1px;
}

.todo__table {
    width: 70%;
    border-collapse: collapse;
    margin: auto;
    box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
}

.todo__table th,
.todo__table td {
    padding: 12px 15px;
    text-align: left;
}

.todo__table thead {
    background-color: #007BFF;
    color: #ffffff;
}

.todo__table tbody tr {
    border-bottom: 1px solid #dddddd;
}

.todo__table tbody tr:nth-of-type(even) {
    background-color: #f9f9f9;
}

.todo__table tbody tr:hover {
    background-color: #f1f1f1;
}

.todo__table button {
    background-color: #dc3545;
    color: white;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.todo__table button:hover {
    background-color: #c82333;
}

</style>