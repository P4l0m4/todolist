<template>
  <main class="todo">
    <h1>To do :</h1>
    <div class="todo__add">
      <input type="checkbox" v-model="tasksAreDone" @change="checkAlltasks" />
      <input
        type="text"
        placeholder="New task"
        v-model="newTodo"
        @keydown.enter="addTodo"
      />
      <button @click="addTodo"><img src="@/assets/plus.svg" /></button>
    </div>
    <div class="todo__list">
      <ul>
        <li
          v-for="todo in filtered"
          :key="todo.id"
          @dblclick="editingToggle(todo)"
        >
          <input
            type="checkbox"
            v-model="todo.completed"
            v-show="!todo.editing"
          />
          <p :class="{ completed: todo.completed }">{{ todo.name }}</p>
          <img
            src="@/assets/close.svg"
            @click="supprTodo(todo)"
            v-show="!todo.editing"
          />
          <input
            class="edition"
            type="text"
            v-show="todo.editing"
            v-model="todo.name"
            @keydown.enter="saveTodoEdit(todo)"
            @blur="saveTodoEdit(todo)"
            @keydown.esc="cancelTodoEdit(todo)"
          />
        </li>
      </ul>

      <div class="filters">
        <p>Tasks remaining : {{ remainingTasks }}</p>

        <div class="filters__links">
          <p>Filtrer :</p>
          <a
            class="filter"
            href="#"
            :class="{ filter__selected: filter === 'all' }"
            @click="filter = 'all'"
            >All</a
          >
          <a
            href="#"
            :class="{ filter__selected: filter === 'done' }"
            @click="filter = 'done'"
            >Done</a
          >
          <a
            href="#"
            :class="{ filter__selected: filter === 'remaining' }"
            @click="filter = 'remaining'"
            >Remaining</a
          >
        </div>
        <button @click="clearCompleted" v-show="atLeastOneCompleted">
          <img src="@/assets/close.svg" />
        </button>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: "",
      filter: "all",
      tasksAreDone: false,
      cachedName: "",
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo) {
        this.todos.push({
          completed: false,
          name: this.newTodo,
          editing: false,
        });
        this.newTodo = "";
      }
    },
    supprTodo(todo) {
      let index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },

    checkAlltasks() {
      if (this.tasksAreDone) {
        this.todos = this.todos.map((todo) => ({
          ...todo,
          completed: true,
        }));
      } else {
        this.todos = this.todos.map((todo) => ({
          ...todo,
          completed: false,
        }));
      }
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
    editingToggle(todo) {
      todo.editing = true;
      this.cachedName = todo.name;
    },
    saveTodoEdit(todo) {
      todo.editing = false;
      if (todo.name === "") {
        this.supprTodo(todo);
      }
    },
    cancelTodoEdit(todo) {
      console.log(todo);
      todo.editing = false;
      todo.name = this.cachedName;
    },
  },
  computed: {
    remainingTasks() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    filtered() {
      if (this.filter === "remaining") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter === "done") {
        return this.todos.filter((todo) => todo.completed);
      }

      return this.todos;
    },
    atLeastOneCompleted() {
      return this.todos.filter((todo) => todo.completed).length;
    },
  },
};
</script>
<style lang="scss">
body {
  // background-color: #181818;
}
.todo {
  display: flex;
  flex-direction: column;
  padding: 16px;
  gap: 16px;
  justify-content: center;
  text-align: center;
  width: 100vw;

  @media (min-witdh: 768px) {
    padding: 24px;
    gap: 24px;
  }

  &__add {
    display: flex;
    gap: 16px;
    padding: 16px;
    justify-content: center;
    align-items: center;
    background-color: lightblue;
  }

  &__list {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }
}
ul {
  width: 100%;
  border: 1px solid black;
  position: relative;
}
li {
  display: flex;
  gap: 16px;
  width: 100%;
  padding: 16px;
  justify-content: space-between;
  align-items: center;
}
input,
button {
  border-color: transparent;
  display: flex;
  height: 40px;
  align-items: center;
}
.edition {
  border: red solid 2px;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 90px;
  right: 46px;
}
.completed {
  text-decoration: line-through;
  text-decoration-color: red;
}
.filters {
  border: 1px solid black;
  display: flex;
  padding: 16px;
  gap: 16px;
  justify-content: center;
  flex-direction: column;

  &__links {
    display: flex;
    gap: 16px;
  }
}
.filter {
  border: transparent;
  padding: 4px 8px;
  font-size: 16px;

  &__selected {
    padding: 4px 8px;
    background-color: lightblue;
  }
}
</style>
