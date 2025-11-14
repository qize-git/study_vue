<template>
  <div class="app-container">
    <div class="todo-app">
      <div class="title">启泽的待办事项</div>

      <div class="todo-form">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo"
          class="todo-input" 
          type="text" 
          placeholder="在这里添加一个待办事项吧~"
        >
        <div @click="addTodo" class="todo-button">添加</div>
      </div>

      <div class="todo-list">
        <div 
          v-for="todo in todos" 
          :key="todo.id" 
          :class="['item', { completed: todo.completed }]"
        >
          <div class="item-content">
            <div 
              :class="['item-checkbox', { checked: todo.completed }]"
              @click="toggleComplete(todo.id)"
            ></div>
            <span class="name">{{ todo.name }}</span>
          </div>
          <div class="del" @click="removeTodo(todo.id)">删除</div>
        </div>
        
        <div v-if="todos.length === 0" class="empty-state">
          <div class="empty-icon">📝</div>
          <p>暂无待办事项，添加一个吧！</p>
        </div>
      </div>

      <div class="stats" v-if="todos.length > 0">
        <span>已完成 {{ completedCount }} / 总计 {{ todos.length }}</span>
        <span>{{ Math.round((completedCount / todos.length) * 100) }}% 完成</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([
  { id: 1, name: '学习Vue 3', completed: true },
  { id: 2, name: '完成待办事项项目', completed: false },
  { id: 3, name: '准备晚餐', completed: false },
  { id: 4, name: '运动30分钟', completed: false }
])

// 添加新待办事项
const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({
      id: Date.now(),
      name: newTodo.value,
      completed: false
    })
    newTodo.value = ''
  }
}

// 删除待办事项
const removeTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id)
}

// 切换完成状态
const toggleComplete = (id) => {
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

// 计算已完成的数量
const completedCount = computed(() => {
  return todos.value.filter(todo => todo.completed).length
})
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #43cea2, #182e9d);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.todo-app {
  width: 100%;
  max-width: 500px;
  background-color: #ffffff;
  border-radius: 20px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15);
  overflow: hidden;
}

.title {
  font-size: 28px;
  font-weight: 700;
  text-align: center;
  padding: 30px 20px 20px;
  background: linear-gradient(to right, #43cea2, #185a9d);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  position: relative;
}

.title::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 80px;
  height: 4px;
  background: linear-gradient(to right, #43cea2, #185a9d);
  border-radius: 2px;
}

.todo-form {
  display: flex;
  padding: 20px;
  gap: 0;
}

.todo-input {
  flex: 1;
  padding: 0 20px;
  border: 2px solid #e1e5e9;
  outline: none;
  height: 56px;
  border-radius: 12px 0 0 12px;
  font-size: 16px;
  transition: all 0.3s;
}

.todo-input:focus {
  border-color: #43cea2;
  box-shadow: 0 0 0 3px rgba(67, 206, 162, 0.2);
}

.todo-button {
  width: 100px;
  height: 56px;
  border-radius: 0 12px 12px 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(to right, #0fdadd, #185a9d);
  cursor: pointer;
  color: #fff;
  font-weight: 600;
  user-select: none;
  transition: all 0.3s;
}

.todo-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(67, 206, 162, 0.4);
}

.todo-list {
  padding: 0 20px 20px;
  max-height: 400px;
  overflow-y: auto;
}

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  margin-bottom: 12px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(149, 157, 165, 0.15);
  transition: all 0.3s;
  background-color: #fff;
  border-left: 4px solid #43cea2;
}

.item:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 15px rgba(149, 157, 165, 0.2);
}

.item-content {
  display: flex;
  align-items: center;
  gap: 12px;
}

.item-checkbox {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  border: 2px solid #ddd;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.item-checkbox.checked {
  background-color: #43cea2;
  border-color: #43cea2;
}

.item-checkbox.checked::after {
  content: "✓";
  color: white;
  font-size: 14px;
  font-weight: bold;
}

.name {
  font-size: 16px;
  transition: all 0.3s;
}

.item.completed .name {
  text-decoration: line-through;
  opacity: 0.6;
}

.del {
  color: #ff6b6b;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
  padding: 5px 10px;
  border-radius: 6px;
}

.del:hover {
  background-color: #ffebee;
  transform: scale(1.1);
}

.stats {
  padding: 15px 20px;
  background-color: #f8f9fa;
  border-top: 1px solid #eaeaea;
  display: flex;
  justify-content: space-between;
  font-size: 14px;
  color: #666;
}

.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: #999;
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 15px;
  opacity: 0.5;
}

@media (max-width: 480px) {
  .app-container {
    padding: 10px;
  }
  
  .todo-app {
    border-radius: 15px;
  }
  
  .title {
    font-size: 24px;
    padding: 20px 15px 15px;
  }
  
  .todo-form {
    flex-direction: column;
    padding: 15px;
    gap: 10px;
  }
  
  .todo-input {
    border-radius: 12px;
    height: 50px;
  }
  
  .todo-button {
    width: 100%;
    border-radius: 12px;
    height: 50px;
  }
  
  .todo-list {
    padding: 0 15px 15px;
  }
  
  .item {
    padding: 14px 16px;
  }
}
</style>