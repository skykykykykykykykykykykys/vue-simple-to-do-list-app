<template>
    <div>
        <input type="text" class="todo-input" placeholder="What should I do later" v-model="newTodo" @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animate__animated animate__fadeInUp" leave-active-class="animate__animated animate__fadeOutDown">
        <todo-item v-for="(todo, index) in todosFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining">
        </todo-item>
        </transition-group>

        <div class="items-left">
            <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
            <div>{{ remaining }} items left</div>
        </div>

        <div class="items-left">
            <div>
                <button :class="{ active: filter == 'all' }" @click="changeFilter('all')">All</button>
                <button :class="{ active: filter == 'active' }" @click="changeFilter('active')">Active</button>
                <button :class="{ active: filter == 'completed' }" @click="changeFilter('completed')">Completed</button>
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
import TodoItem from './TodoItem'


export default {
    name: 'todo-list',
    components: {
        TodoItem
    },
    data() {
        return {
            newTodo: '',
            idForTodo: 3,
        }
    },
    computed: {
        remaining() {
            return this.$store.getters.remaining
        },
        anyRemaining() {
            return this.$store.getters.anyRemaining
        },
        filter() {
            return this.$store.state.filter
        },
        todosFiltered() {
            return this.$store.getters.todosFiltered
        },
        showClearCompletedButton() {
            return this.$store.getters.showClearCompleted
        }
    },
    methods: {
        addTodo() {
            if (this.newTodo.trim().length == 0) {
                return
            }
            this.$store.dispatch('addTodo', {
                id: this.idForTodo,
                title: this.newTodo,
            })
            this.newTodo = ''
            this.idForTodo++
        },
        removeTodo(id){
            this.$store.dispatch('deleteTodo', id)
        },
        editTodo() {
            this.beforeEditCache = this.title
            this.editing = true
        },
        checkAllTodos() {
            this.$store.dispatch('checkAll', event.target.checked)
        },
        clearCompleted() {
            this.$store.dispatch('clearCompleted')
        },
        changeFilter(filter) {
            this.$store.dispatch('updateFilter', filter)
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">

    @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css');

    .todo-input {
        width: 100%;
        padding: 10px 10px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus {
            outline: 0;
        }
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        animation-duration: 0.5s;
    }

    .remove-item {
        cursor: pointer;
        margin-left: 0px;
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
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Arial, Helvetica, sans-serif;

        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .items-left {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;

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

    // CSS Transitions

    .fade-enter-active, .fade-leave-active {
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

</style>
