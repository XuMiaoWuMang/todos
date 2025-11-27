<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoList from './components/TodoList.vue'
import TodoFooter from './components/TodoFooter.vue';
export default {
  data() {
    return {
      //数据
      todos: JSON.parse(localStorage.getItem('vue-todos') || '[]'),
      tabType: 0 //默认为显示全部任务
    }
  }, components: {
    TodoHeader,
    TodoList,
    TodoFooter
  }, computed: {
    todoList() {
      if (this.tabType == 0) {//显示全部
        return this.todos;
      }
      const tabType = this.tabType;
      return this.todos.filter(function (item) {
        if (tabType == 1) //未完成
          return !item.completed;
        return item.completed; //已完成
      })
    }
  }, methods: {
    addTodo(newTodo) {
      console.log("新增加的任务信息为：" + newTodo);
      this.todos.push({
        id: new Date(),
        txt: newTodo,
        completed: false
      })
    }, changeTabType(type) {
      this.tabType = type;
    },
    delTodo(delTodo) {
      console.log("要删除的任务id为" + delTodo.id);
      this.todos = this.todos.filter(function (item) {
        return item.id !== delTodo.id;
      })
    }
  }, watch: {
    todos: {
      handler(todos) {
        localStorage.setItem('vue-todos', JSON.stringify(todos));
      },
      deep: true
    }
  }
}
</script>
<template>
  <TodoHeader @addTodo="addTodo"></TodoHeader>
  <TodoList :todos="todoList" @delTodo="delTodo"></TodoList>
  <TodoFooter :tabType="tabType" :count="todoList.length" @changeTabType="changeTabType"></TodoFooter>

</template>

<style>
html,
body {
  background-color: #f5f5f5;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  max-width: 980px;

  min-height: 100%;
  margin: 0 auto;
}
</style>