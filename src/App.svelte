<script>
  // #StateVariable
  let todoItems = [];
  // #StateVariable
  let newTodo = "";

  function addTodo() {
    newTodo = newTodo.trim();
    if (!newTodo) return;

    // #StateVariable immutable
    const todo = {
      text: newTodo,
      checked: false,
      id: Date.now(),
    };

    // #Reactive svelte will rerender each time todo items change (on assignment)
    todoItems = [...todoItems, todo];
    // #Reactive empties DOM form input value field
    newTodo = "";
  }
  function toggleDone(id) {
    // #ControlFlow related function — unpacks an array and applies a condition to elements
    const index = todoItems.findIndex((item) => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }
  function deleteTodo(id) {
    // #ControlFlow related function — unpacks an array and applies a condition to elements
    todoItems = todoItems.filter((item) => item.id !== Number(id));
  }
</script>

<main>
  <div class="container">
    <h1 class="app-title">todos</h1>
    <ul class="todo-list" />
    <div class="empty-state">
      <svg class="checklist-icon"><use href="#checklist-icon" /></svg>
      <h2 class="empty-state__title">Add your first todo</h2>
      <p class="empty-state__description">
        What do you want to get done today?
      </p>
    </div>
    <!-- #EventHandler for form submission -->
    <form on:submit|preventDefault={addTodo}>
      <input
        class="js-todo-input"
        type="text"
        aria-label="Enter a new todo item"
        placeholder="E.g. Build a web app"
        bind:value={newTodo}
      />
    </form>
    <br />
    <ul style="columns:2" class="todo-list">
      <!-- #ControlFlow "each" keywords allows todo items to render without an explicit loop -->
      {#each todoItems as todo (todo.id)}
        <li class="todo-item {todo.checked ? 'done' : ''}">
          <!-- Example of #Properties -->
          <input id={todo.id} type="checkbox" />
          <!-- #EventHandler for toggling done/todo -->
          <label
            for={todo.id}
            class="tick"
            on:click={() => toggleDone(todo.id)}
          />
          <!-- #Reactive todo.text could change based on todo item, initial input -->
          <span>{todo.text}</span>
          <!-- #EventHandler for deleting a todo list item -->
          <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
            <svg><use href="#delete-icon" /></svg>
          </button>
        </li>
      {/each}
    </ul>
  </div>
</main>
