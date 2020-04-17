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
      <li
        v-for="(todo, index) in todoList"
        :key="index"
        class="list-group-item"
        :class="{
          'list-group-item': true,
          'list-group-item-secondary': todo.checked
        }"
        @click="checkTodo(todo)"
      >
        {{ todo.todo }}

        <button
          type="button"
          class="close"
          data-dismiss="modal"
          aria-label="Close"
          @click.stop="deleteTodo(todo)"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import * as firebase from "firebase/app";
import "firebase/firestore";
import firebaseConfig from "@/firebaseConfig";

firebase.initializeApp(firebaseConfig);

class Todo {
  id?: string;
  todo: string;
  checked: boolean;

  constructor(todo: string) {
    this.todo = todo;
    this.checked = false;
  }

  toggleChecked() {
    this.checked = !this.checked;
  }
}

@Component
export default class TodoList extends Vue {
  todoInput = "";
  todoList: Todo[] = [];

  mounted() {
    firebase
      .firestore()
      .collection("todolists")
      .onSnapshot(snapshot => {
        const todolists = snapshot.docs.map(_doc => {
          const data = _doc.data();
          const todo = new Todo(data.todo);
          todo.id = _doc.id;
          todo.checked = data.checked;
          return todo;
        });
        console.log("firebase todolists: ", todolists);
        this.todoList = todolists;
      });
  }

  addTodo() {
    console.log("todo input : ", this.todoInput);
    const todo = new Todo(this.todoInput);

    firebase
      .firestore()
      .collection("todolists")
      .add({
        todo: todo.todo,
        checked: todo.checked
      })
      .then(result => console.log("result:", result))
      .catch(error => console.log("error:", error));
  }

  checkTodo(todo: Todo) {
    todo.toggleChecked();
    firebase
      .firestore()
      .collection("todolists")
      .doc(todo.id)
      .set({
        todo: todo.todo,
        checked: todo.checked
      })
      .then(result => console.log("result:", result))
      .catch(error => console.log("error:", error));
  }

  deleteTodo(todo: Todo) {
    console.log("todo delete : ", todo);
    /*this.todoList = this.todoList.filter(todoItem => {
      return todoItem !== todo;
    });*/

    firebase
      .firestore()
      .collection("todolists")
      .doc(todo.id)
      .delete()
      .then(result => console.log("result:", result))
      .catch(error => console.log("error:", error));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.list-group-item-secondary {
  text-decoration: line-through;
}
</style>
