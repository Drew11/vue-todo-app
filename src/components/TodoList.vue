<template>
    <div>
        <input
             type="text"
             class="todo-input"
             placeholder="What needs to be done"
             v-model="newTodo"
             @keypress.enter="addTodo"
        >
        <transition-group
                name="fade"
                enter-active-class="animated fadeInUp"
                leave-active-class="animated fadeOutDownUp"
        >
            <div v-for="todo in todosFiltered"
                 :key="todo.id"
                 class="todo-item"
            >
                <div class="todo-item-left">
                    <input type="checkbox" v-model="todo.completed">
                    <div
                            :class="[{completed: todo.completed}, 'todo-item-label']"
                            v-if="!todo.editing"
                            @dblclick="editTodo(todo)"
                    >
                        {{todo.title}}
                    </div >

                    <input
                            class="todo-item-edit"
                            type="text"
                            v-model="todo.title"
                            v-else
                            @blur="doneEdit(todo)"
                            @keyup.enter="doneEdit(todo)"
                            @keyup.esc="cancelEdit(todo)"
                            v-focus
                    >
                </div>

                <div
                        class="remove-item"
                        @click="removeTodo(todo.id)"
                >
                    &times;
                </div>
            </div>
        </transition-group>

        <div class="extra-container">
            <div>
                <label>
                    <input
                            type="checkbox"
                            :checked="!anyRemaining"
                            @change="checkAllTodos"
                    >Check All
                </label>
            </div>
            <div>{{remaining}} items left</div>
        </div>

        <div class="extra-container">
            <div>
                <button
                        :class="{ active: filter === 'all' }"
                        @click="filter = 'all'"
                >
                    All
                </button>

                <button
                        :class="{ active: filter === 'active' }"
                        @click="filter = 'active'"
                >
                    Active
                </button>

                <button
                        :class="{ active: filter === 'completed' }"
                        @click="filter = 'completed'"
                >
                    Completed
                </button>

            </div>

            <div>
                <transition name="fade">
                    <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
                </transition>
            </div>
        </div>


    </div>

</template>

<script>
    export default {
        name: 'todo-list',
        data() {
            return {
                newTodo: '',
                titleFromCache: '',
                idForTodo: 4,
                filter: 'all',
                todos: [
                    {
                        id: 1,
                        title: 'Finish vue screencast',
                        completed: false,
                        editing: false
                    },
                    {
                        id: 2,
                        title: 'Some text',
                        completed: false,
                        editing: false,
                    },
                    {
                        id: 3,
                        title: 'sometext',
                        completed: true,
                        editing: false
                    },
                ]
            }
        },
        computed: {
            remaining(){
                return this.todos.filter(todo=>!todo.completed).length
            },
            anyRemaining() {
                return this.remaining !== 0
            },
            todosFiltered() {
              if (this.filter === 'all')
                  return this.todos;

              else if ( this.filter === 'active')
                  return this.todos.filter(todo=>!todo.completed);

              else if( this.filter === 'completed')
                  return this.todos.filter(todo=>todo.completed);

              return this.todos
            },
            showClearCompletedButton() {
                return this.todos.filter(todo=>todo.completed).length > 0
            }
        },
        directives: {
            focus: {
                inserted: function (el) {
                    el.focus()
                }
            }
        },
        methods: {
            addTodo() {
                if(this.newTodo.trim() === '') {
                    return
                }

                this.todos.push({
                    id: this.idForTodo,
                    title: this.newTodo,
                    completed: false,
                    editing: false,
                });

                this.newTodo = '';
                this.idForTodo++;
            },

            editTodo(todo) {
                if(todo.completed) return;
                this.titleFromCache = todo.title;
                todo.editing = true;
            },

            doneEdit(todo) {
                if(todo.title.trim() === ''){
                    todo.title =  this.titleFromCache;
                }
                todo.editing = false;
                todo.title = todo.title.trim();
            },

            cancelEdit(todo) {
                todo.title = this.titleFromCache;
                todo.editing = false;
            },

            removeTodo (id) {
                this.todos = this.todos.filter(todo => todo.id !== id);
            },

            checkAllTodos() {
                this.todos.forEach(todo => todo.completed = event.target.checked)
            },

            clearCompleted() {
                this.todos = this.todos.filter(todo => !todo.completed );
            }
        }
    }
</script>

<style lang="scss">
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
        border-radius: 3px;
        border: 1px solid lightgrey;

        &:focus {
            outline: none;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: black;
        }
    }

    .todo-item-left {
        display: flex;
        align-items: center;
    }

    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif ;
        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        margin-right: 10px;
        font-size: 14px;
        background-color: white;
        appearance: none;
        cursor: pointer;
        &:hover {
            background: lightgreen;
        }
        &:focus {
            outline: none;
        }
    }
    .active {
        background: lightgreen;
    }

    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

</style>