<script>
  import { writable } from 'svelte/store';

  const todos = writable([]);
  let newTodo = '';

  const addTodo = () => {
    if (newTodo.trim()) {
      todos.update((items) => [...items, { text: newTodo, completed: false }]);
      newTodo = '';
    }
  };

  const toggleTodo = (index) => {
    todos.update((items) => {
      items[index].completed = !items[index].completed;
      return items;
    });
  };

  const deleteTodo = (index) => {
    todos.update((items) => items.filter((_, i) => i !== index));
  };
</script>

<main>
  <h1>To-Do List</h1>

  <input
    type="text"
    bind:value={newTodo}
    placeholder="Add a new task..."
    on:keydown={(e) => e.key === 'Enter' && addTodo()}
  />
  <button on:click={addTodo}>Add</button>

  <ul>
    {#each $todos as todo, index}
      <li>
        <input
          type="checkbox"
          bind:checked={todo.completed}
          on:change={() => toggleTodo(index)}
        />
        <span class:completed={todo.completed}>{todo.text}</span>
        <button on:click={() => deleteTodo(index)}>Delete</button>
      </li>
    {/each}
  </ul>
</main>

<style>
  h1 {
    color: #333;
  }

  .completed {
    text-decoration: line-through;
    color: #999;
  }

  button {
    margin-left: 10px;
  }

  input[type="text"] {
    margin-right: 10px;
    padding: 4px;
  }
</style>
