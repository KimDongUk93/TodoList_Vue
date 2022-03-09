<template>
  <div id="app">
    <TodoList msg="Hello Vue!"/>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  //name: 'App',: 작성을 안해도 정상 동작한다?/ 작성을 안하면 파일명으로 export되는듯 
  components: {//임포트 하고싶은 컴포넌트를 모아 놓은 객체인 듯?
    TodoList
  }
}
</script>
