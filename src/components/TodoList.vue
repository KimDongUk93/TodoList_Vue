<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- 반복문인듯하다. -->
    <ul class="todo-list" v-for="(item, i) in todoList" :key="i">
        <li>
          {{i + 1}}. {{item.content}} | 
          <button @click="handleEdit(item.id)">EDIT</button> 
          <button @click="handleDelete(item.id)">DEL</button>
        </li>
    </ul>
    <div class="todo-input">
      {{editMode ? "Edit" : "Add"}}
      <input type="text" v-model="todoItem.content"/>
    </div>
    <div class="todo-handle-btns">
      <button @click="handleTodoItem">
        {{editMode ? "Edit" : "Add"}}
      </button>
      <button v-if="editMode" @click="handleCancle">
        Cancel
      </button>
    </div>
  </div>
</template>

<script>
import Axios from "axios";

const todoUrl = "http://localhost:3500/todo"

export default {
  name: 'TodoList',
  props: {//컴포넌트로 전달된 props를 담은 객체인 듯?
    msg: String//props의 이름과 타입을 정한 듯 하다.
  },
  data(){//리액트의 state같은 개념?/ 왜 함수형으로 되 있는지 모르겠다. 아무튼 함수형으로 객체를 리턴해야함
    return{
      todoList: [],
      todoItem: {},
      editMode: false
    }
  },
  created(){//리액트의 useEffect 같은 개념?
    (async ()=>{
      const res = await Axios.get(todoUrl);
      this.todoList = res.data;
    })()
  },
  methods: {//컴포넌트 내의 메소드들을 정의해 놓는 곳 같다.
    handleEdit(id){
      this.editMode = true;
      this.todoItem = this.todoList.find(item => item.id == id)
    },
    handleCancle(){
      this.editMode = false;
      this.todoItem = ""
    },
    async handleTodoItem(){
      const id = this.todoItem.id;

      if(this.editMode){
        await Axios.put(`${todoUrl}/${id}`, this.todoItem);

        this.editMode = false;
        this.todoItem.content = "";
      } else {
        await Axios.post(todoUrl, this.todoItem);

        this.todoItem.content = "";
      }

      Axios.get(todoUrl).then(res => (
        this.todoList = res.data
      ))
    },
    async handleDelete(id){
      await Axios.delete(`${todoUrl}/${id}`);
      Axios.get(todoUrl).then(res => (
        this.todoList = res.data
      ))
    }
  }
}
</script>
