<script>
let todoItem = $state('');
let todoList = $state([]);

function addItem(event){
     event.preventDefault();
     if(todoItem == ''){
          return;
     }
     todoList = [...todoList,{
          text: todoItem,
          done: false
     }];
     todoItem = '';
}

function removeItem(index){
    todoList = todoList.toSpliced(index, 1);
}

function clear(){
     todoList.length = 0;
}

$inspect(todoList);

</script>
<div class="main">
     <form onsubmit={addItem}>
          <input type="text" bind:value={todoItem}>
          <button type="submit">Add</button>
     </form>

     <ul>
          {#each todoList as item, index}
               <li>
               <input type="checkbox" bind:checked={item.done}>
               <span class:done={item.done}>{item.text}</span>
                <button type="button" onclick={() => removeItem(index)}>x</button>
               </li>
          {/each}
     </ul>
     <ul>
          {#if (todoList.length > 0)}
               <button type="button" onclick={clear}>Clear</button>
          {/if}
     </ul>
</div>

<style>
     ul{
          list-style: none;
          margin: 0;
     }
span.done{
     color: rgb(198, 198, 198);
     text-decoration: line-through;
}
button{
     background-color: transparent;
     color: rgb(55, 55, 55);
     font-family: "Caveat", cursive;
}
input{
     font-family: "Caveat", cursive;
}
</style>