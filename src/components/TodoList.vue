
<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What needs to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <transition-group
    name="fade-custom"
    enter-active-class="animated animate__fadeInUp"
    leave-active-class="animated animate__fadeOutDown"
  >
      <div v-for="(todo,index) in todosFiltered" :key="todo.id" class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed" />
            <p v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</p>
            <input
              v-else
              type="text" 
              class="todo-item-edit" 
              v-model="todo.title" 
              @blur="doneEdit(todo)" 
              @keyup.enter="doneEdit(todo)" 
              @keyup.esc="cancelEdit(todo)"
              v-focus>
          </div>
          <span class="remove-item" @click="removeTodo(index)">&times;</span>
      </div>
    </transition-group>
    <hr />
    <div class="todo-list-footer">
      <div><label><input type="checkbox" :checked="!anyRemainingTodo" @change="checkAllTodos">Check all</label></div>
      <div>{{ remaining }} items left</div>
    </div>
    <div class="todo-list-footer">
      <div>
        <button class="button" :class="{ active: filter == 'all'}" @click="filter = 'all'">All</button>
        <button class="button" :class="{ active: filter == 'active'}" @click="filter = 'active'">Active</button>
        <button class="button" :class="{ active: filter == 'completed'}" @click="filter = 'completed'">Completed</button>
      </div>
    <div>
      <transition name="fade">
        <a class="button--link" v-if="showClearCompletedButton" @click="clearCompleted()">Clear completed</a>
      </transition>
    </div>
    </div>
  </div>
</template>

<script>

import Vue from 'vue';

Vue.directive('focus', {
  // When the bound element is inserted into the DOM...
  inserted: function (el) {
    // Focus the element
    el.focus()
  }
})

export default {
  name: "todo-list",
  data() {
      return {
        newTodo: '',
        idForTodo: 3,
        beforeEditCache: '',
        filter: "all",
        todos: [
            {
              'id': 1,
              'title': 'Finish Vue Screencast',
              'completed': false,
              'editing': false
            },
             {
              'id': 2,
              'title': 'Take dog for a walk',
              'completed': false,
              'editing': false
            }
        ]
      }
  },
  computed: {
    remaining() {
      return this.todos.filter(item => !item.completed).length
    },
    anyRemainingTodo() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if(this.filter == "all") {
        return this.todos
      } else if(this.filter == "active") {
          return this.todos.filter(todo => !todo.completed)
      } else if(this.filter == "completed") {
         return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  methods: {
      addTodo() {
          if(this.newTodo.trim().length == 0){
            return
          }
          this.todos.push({
              'id': this.idForTodo,
              'title': this.newTodo,
              completed: false
          })
          this.newTodo = '';
          this.idForTodo++
      },
      editTodo(todo) {
        if(todo.title.trim() == '') {
          todo.title = this.beforeEditCache;
        }
        this.beforeEditCache = todo.title;
        todo.editing = true;
      },
      doneEdit(todo) {
        todo.editing = false;
      },
      cancelEdit(todo) {
        todo.title = this.beforeEditCache;
        todo.editing = false;
      },
      removeTodo(index) {
        this.todos.splice(index, 1)
      },
      checkAllTodos() {
        this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed)
      }
  }
};
</script>

<style lang="scss" scoped>
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css");

.todo-input {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }

  &::placeholder {
    font-family: Avenir, Helvetica, Arial, sans-serif;
  }
}

.todo-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
    animation-duration: 0.3s;
}

.remove-item {
    cursor: pointer;
    &:hover {
        color: purple;
    }
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.completed {
  text-decoration: line-through;
  color: grey;
}

.todo-list-footer {
  display: flex;
  justify-content: space-between;
  padding: 15px 0;
}

.button {
  display:inline-block;
  padding:0.3em 1.2em;
  margin:0 0.1em 0.1em 0;
  border:0.16em solid rgba(255,255,255,0);
  border-radius:2em;
  background-color: rgb(123, 227, 253);
  box-sizing: border-box;
  text-decoration:none;
  font-family:'Roboto',sans-serif;
  font-weight:300;
  text-align:center;
  transition: all 0.2s;

  &:focus {
    outline: none;
  }

  &:hover {
    border-color: rgba(255,255,255,1);
    cursor: pointer;
  }

  &.active {
    background-color: #4ef18f;
    color: white;
  }
}

.button--link {
  color: #0089c7;
  text-decoration: underline;
  text-decoration-color: #a2dffb;
  cursor: pointer;
  &:hover {
    opacity: .66;
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}


</style>
