<script>
import {onMount} from 'svelte';
let todoItem = $state('');
let todoList = $state([]);
let doneList = $state([]);
let storedList;

onMount(() => {
     storedList = localStorage.getItem('storedList');
     if (storedList){
          todoList = (JSON.parse(storedList));
     }
})

function updateList(){
     return storedList = localStorage.setItem('storedList', JSON.stringify(todoList));
}

function addItem(event){
     event.preventDefault();
     if(todoItem == ''){
          return;
     }
     todoList = [...todoList,{
          text: todoItem,
          done: false
     }];
     updateList();
     todoItem = '';
}

function removeItem(index){
    todoList = todoList.toSpliced(index, 1);
    updateList();
}

function clear(){
     todoList = [];
     localStorage.clear();
}

function clearDone(){
     todoList = todoList.filter((item) => !item.done);
     updateList();
}

$effect(() => {
     doneList = todoList.filter((item) => item.done);
     updateList();
})

function undoThis(item){
     item.done = !item.done;
     updateList();
}

$inspect('To Do List:', todoList);
$inspect('Done List:', doneList);

</script>
<div class="main">
     <form onsubmit={addItem}>
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
     </form>
     <div class="lists">
          <div class="todo-list">
               <ul>
                     {#each todoList as item, index}
                         <li>
                              <input type="checkbox" bind:checked={item.done}>
                              <span class:done={item.done}>{item.text}</span>
                              <button type="button" onclick={() => removeItem(index)}>X</button>
                         </li>
                    {/each}
               </ul>
               {#if (todoList.length > 0)}
                    <button class="clear" type="button" onclick={clear}>Clear</button>
                    <button class="clear" type="button" onclick={clearDone}>Clear Completed</button>
               {/if}
          </div>

          {#if (doneList.length > 0)}
          <div class="done-list">
               <h2>Completed:</h2>
               {#each doneList as item}
                    <li>
                         <span>{item.text}</span>
                         <button type="button" onclick={() => undoThis(item)}>&#8617;</button>
                    </li>
               {/each}
          </div>
          {/if}
     </div>
</div>

<style>
input[type="text"]{
     font-family: "Caveat", cursive;
     border-radius: 5px;
}

ul{
     list-style: none;
     margin: 0;
}

li.done{
     display: none;
}
span.done{
     color: rgb(198, 198, 198);
     text-decoration: line-through;
}
form button{
     background-color: transparent;
     color: rgb(55, 55, 55);
     font-family: "Caveat", cursive;
     font-size: 1em;
}
button{
     background-color: transparent;
     border-color: transparent;
     font-family: "Caveat", cursive;
     border-radius: 5px;
     color: rgb(55, 55, 55);
     font-weight: 700;
}
.clear{
     background-color: rgb(203, 133, 156);
     border-color: rgb(202, 112, 142);
     font-size: 1em;
}
</style>