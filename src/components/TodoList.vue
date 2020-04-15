<template>
    <div class="todolist">
        <h3>TodoList</h3>
        <div class="input-group mb-3">
            <input
                    type="text"
                    class="form-control"
                    placeholder="Todo"
                    aria-label="Todo"
                    aria-describedby="button-addon2"
                    v-model="todoInput"
            />
            <div class="input-group-append">
                <button
                        class="btn btn-outline-secondary"
                        type="button"
                        id="button-addon2"
                        @click="addTodo"
                >
                    ADD
                </button>
            </div>
        </div>
        <ul class="list-group">
            <li v-for="(todo, index) in todoList"
                :key="index"
                class="list-group-item"
                :class="{
                    'list-group-item': true,
                    'list-group-item-secondary': todo.checked
                }"
                @click="todo.toggleChecked()">
                {{todo.todo}}

                <button type="button" class="close" data-dismiss="modal" aria-label="Close"
                    @click.stop="deleteTodo(todo)">
                    <span aria-hidden="true">&times;</span>
                </button>
            </li>
        </ul>
    </div>
</template>

<script lang="ts">
    import {Component, Prop, Vue} from "vue-property-decorator";

    class Todo {
        todo: string;
        checked: boolean;

        constructor(todo: string) {
            this.todo = todo;
            this.checked = false;
        }
        toggleChecked(){
            this.checked= !this.checked;
        }
    }

    @Component
    export default class TodoList extends Vue {
        todoInput = '';
        todoList: Todo[] = [];

        addTodo() {
            console.log('todo input : ', this.todoInput);
            const todo = new Todo(this.todoInput);
            this.todoList.push(todo);
        }

        deleteTodo(todo: Todo){
            console.log('todo delete : ', todo);
            this.todoList = this.todoList.filter(todoItem=>{
                return todoItem !== todo;
            });
        }

    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .list-group-item-secondary{
        text-decoration: line-through;
    }
</style>
