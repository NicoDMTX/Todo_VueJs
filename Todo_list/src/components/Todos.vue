<template>
    <section class="todoapp flex flex-col justify-center items-center">
        <header class="header flex flex-col items-center">
            <h1 class="text-slate-200">Todos</h1>
            <input 
            type="text" 
            maxlength="100"
            minlength="3"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" 
            placeholder="Add task" 
            v-model="newTodo" 
            @keyup.enter.prevent="addTodo"
            >
        </header>
        <div class="bg-slate-800 p-2 mt-2 rounded flex flex-col">
            <input type="checkbox" class="toggle-all" v-model="allDone">
            <span class="text-slate-200">Complete tasks</span>
        </div>

        <div class="main mx-auto">
            <ul class="todo-list">
                <li v-show="todo.name" class="todo flex flex-col" v-for="todo in filteredTodos" :key="todo.name">
                    <div class="view bg-slate-800 m-4 p-6 rounded-r-lg max-w-md shadow-inner shadow-lg">
                        <input type="checkbox" class="accent-indigo-500 checked:bg-indigo-500 m-2 w-4 h-4"
                        @click="todo.completed = !todo.completed" v-model="todo.completed">
                        <label class="text-slate-200 tracking-wide font-medium break-words text-xl" :class="{'line-through': todo.completed}">{{  todo.name }}</label>
                        <button class="destroy bg-indigo-500 text-slate-200 p-2 rounded m-2" @click.prevent="deleteTodo(todo)">x</button>
                    </div>
                </li>
            </ul>
        </div>
        <footer v-show="remaining > 0" class="footer flex flex-col items-center">
            <span class="todo-count text-indigo-500 underline underline-offset-4 decoration-2 mb-4">
                <strong>{{  remaining  }}</strong> 
                tâches à faire
            </span>
            <ul class="flex flex-row text-slate-200 bg-slate-800 rounded p-2 hover:bg-indigo-500 font-medium ">
                <li class="ml-4 mr-4"><a href="#" :class="{'text-amber-500': filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{'text-amber-500': filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li class="ml-4 mr-4"><a href="#" :class="{'text-amber-500': filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
                <button class="bg-indigo-500 p-1 rounded hover:bg-slate-800">Delete all tasks</button>
            </ul>
        </footer>
    </section>
</template>

<script>

export default {
    data() {
        return {
            todos: [{
                name: 'Test task',
                completed: false
            }],
            newTodo: '',
            filter: 'all',
        }
    },
    methods: {
        addTodo() {
            this.todos.push({
                completed: false,
                name: this.newTodo
            });

            this.newTodo = '';
        },
        deleteTodo(todo) {
            this.todos = this.todos.filter(t => t !== todo)
        }
    },
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length;
        },
        filteredTodos() {
            if(this.filter === 'todo') {
                return this.todos.filter(todo => !todo.completed);
            } else if (this.filter === 'done') {
                return this.todos.filter(todo => todo.completed);
            }
            return this.todos
        },
        allDone: {
            get() {
                return this.remaining === 0;
            },
            
            set(value) {
                this.todos.forEach(todo => {
                    todo.completed = value;
                })
            }
        }
    }
}
</script>

<style>

</style>