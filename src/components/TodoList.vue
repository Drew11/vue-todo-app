<template>
    <div>
        <input
             type="text"
             class="todo-input"
             placeholder="What needs to be done"
             v-model="newTodo"
             @keypress.enter="addTodo"
        >

        <div v-for="todo in todos"
             :key="todo.id"
             class="todo-item"
        >
            <div class="todo-item-left">
                <div class="todo-item-label">{{todo.title}}</div>

                <input
                        class="todo-item-edit"
                        type="text"
                        v-model="todo.title">
            </div>

            <div
                    class="remove-item"
                    @click="removeTodo(todo.id)"
            >
                &times;
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
                idForTodo: 4,
                todos: [
                    {
                        id: 1,
                        title: 'Finish vue screencast',
                        completed: false
                    },
                    {
                        id: 2,
                        title: 'Some text',
                        completed: false
                    },
                    {
                        id: 3,
                        title: 'sometext',
                        completed: true
                    },
                ]
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
                    completed: false
                });

                this.newTodo = '';
                this.idForTodo++;
            },
            removeTodo (id) {
                this.todos = this.todos.filter(todo => todo.id !== id);
            }
        }
    }
</script>

<style lang="scss">

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
        border: 1px solid white;
        font-family: 'Avenir', Helvetica, Arial, sans-serif ;
        &:focus {
            outline: none;
        }
    }
</style>