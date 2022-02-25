<template>
  <div id="app">
    <input type="text" v-model="newTodoItem" @keydown="addTodo" />

    <!-- CRUD(크루드) / 웹의 기본적인 기능
    1. C: Create - 새로운 항목 추가(insert)
    2. R: Read - 데이터 읽어오기 
    3. U: Update - 수정
    4. D: Delete - 삭제 -->

    <table>
      <thead>
        <tr>
          <th>NO.</th>
          <th>TODO</th>
          <th>DONE</th>
          <th>EDIT</th>
          <th>DELETE</th>
        </tr>
      </thead>

      <tbody>
        <!-- 반복문(loop): i:iterate / for(let i=0; i<10; i++) {} -->
        <!-- v-for="(objectName, index) in arrayName" v-bind:key="objectName.id" -->
        <!-- unique, null =/= '' -->
        <tr v-for="(todo, index) in todoList" :key="'todo_' + todo.id">
          <td>{{ todo.id }}</td>

          <td v-if="todo.isEdit === false">{{ todo.todo }}</td>
          <td v-else>
            <input
              type="text"
              :placeholder="todo.todo"
              v-model="editTodoItem"
              @keydown="editTodoText($event, index)"
            />
          </td>

          <td @click="toggleStatus(index)">{{ todo.isCompleted }}</td>
          <td><button @click="editTodo(index)">EDIT</button></td>
          <td><button @click="deleteTodo(index)">DELETE</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      newTodoItem: "",
      editTodoItem: "",

      // Object - key:value
      todoList: [
        {
          id: 1,
          todo: "Study Javascript",
          isCompleted: false,
          isEdit: false,
        },
        {
          id: 2,
          todo: "Do Homework",
          isCompleted: false,
          isEdit: false,
        },
        {
          id: 3,
          todo: "Take a shower",
          isCompleted: false,
          isEdit: false,
        },
        {
          id: 4,
          todo: "Brush Teeth",
          isCompleted: false,
          isEdit: false,
        },
        {
          id: 5,
          todo: "Talk to mom",
          isCompleted: false,
          isEdit: false,
        },
      ],
    };
  },

  methods: {
    toggleStatus(index) {
      // this.todoList[index].isCompleted = !this.todoList[index].isCompleted

      if (this.todoList[index].isCompleted) {
        this.todoList[index].isCompleted = false;
      } else {
        this.todoList[index].isCompleted = true;
      }
    },

    addTodo(e) {
      if (e.keyCode === 13) {
        let newTodoObj = {
          id: this.todoList[this.todoList.length - 1].id + 1,
          // id: this.todoList.length + 1,
          todo: this.newTodoItem,
          isCompleted: false,
          isEdit: false,
        };

        this.todoList.push(newTodoObj);
        this.newTodoItem = "";
      }
    },

    deleteTodo(index) {
      this.todoList.splice(index, 1);
    },

    editTodo(index) {
      this.todoList[index].isEdit = !this.todoList[index].isEdit;
    },

    editTodoText(e, index) {
      if (e.keyCode === 13) {
        this.todoList[index].todo = this.editTodoItem;
        this.todoList[index].isEdit = !this.todoList[index].isEdit;
        this.editTodoItem = "";
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

table thead tr th {
  height: 30px;
  background-color: black;
  color: white;
}

table thead tr th:nth-child(2) {
  width: 150px;
}

table tbody tr {
  height: 30px;
}

/* odd - 홀수 / even - 짝수 */
table tbody tr:nth-child(odd) {
  background-color: #cccccc;
}

table tbody tr:hover {
  background-color: lightgreen;
  cursor: pointer;
}

input {
  margin-bottom: 20px;
  padding: 10px;
  font-size: 14px;
  border-radius: 10px;
  border: 1px solid lightgrey;
}
</style>
