<template>
  <div id="app">

    <div class="header">
      <h1>Task Manager</h1>
    </div>

    <div class="container">

      <div class="lists">

        <div class="list" v-for="list in lists" :key="list.id">
          <h2 id="list-name">{{ list.name }}</h2>

          <div class="tasks">
            <div class="task" v-for="task in list.tasks" :key="task.id">
              <p>{{ task.name }}</p>
              <button @click="deleteTask(task.id)">Delete</button>
            </div>
          </div>

          <form @submit.prevent="addTask(list.id)">
            <input type="text" v-model="newTask" placeholder="Enter new task" />
            <button>Add</button>
          </form>
          
        </div>

      </div>

      <form @submit.prevent="addList" id="lastInput">
          <input type="text" v-model="newList" placeholder="Enter new list name" />
          <button>Add</button>
      </form>

    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const lists = ref([
      {
        id: 1,
        name: "To Do",
        tasks: [
          { id: 1, name: "Learn Vue 3" },
          { id: 2, name: "Build a website" },
        ],
      },
      {
        id: 2,
        name: "In Progress",
        tasks: [
          { id: 3, name: "Design a UI" },
          { id: 4, name: "Sleep Well" }
        ],
      },
      {
        id: 3,
        name: "Done",
        tasks: [
          { id: 5, name: "Create a repo" },
          { id: 6, name: "Play Football" },
        ],
      },
    ]);

    const newList = ref("");
    const newTask = ref("");

    const addList = () => {
      if (newList.value) {
        const id = lists.value.length + 1;
        const name = newList.value;
        lists.value.push({ id, name, tasks: [] });
        newList.value = "";
      }
    };

    const addTask = (listId) => {
      if (newTask.value) {
        const list = lists.value.find((list) => list.id === listId);
        const id = list.tasks.length + 1;
        const name = newTask.value;
        list.tasks.push({ id, name });
        newTask.value = "";
      }
    };

    const deleteTask = (taskId) => {
      lists.value.forEach((list) => {
        list.tasks = list.tasks.filter((task) => task.id !== taskId);
      });
    };

    return {
      lists,
      newList,
      newTask,
      addList,
      addTask,
      deleteTask,
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body { 
  background-color: black;
}

h1, h2, p {
  color: white;
}

.header h1 {
  text-align: center;
  margin: 20px;
  background: linear-gradient(to right, white, black);
  -webkit-background-clip: text; /* for webkit browsers like Chrome and Safari */
  color: transparent;
}

.container {
  max-width: 800px;
  margin: 0 auto;
}

.lists {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.list {
  width: 300px;
  border: 4px solid white;
  border-radius: 10px;
  padding: 15px;
  margin: 15px;
}

#list-name {
  position: relative;
  padding: 10px;
  margin-bottom: 20px;
}

#list-name::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px; /* Adjust as needed */
  background: linear-gradient(to right, white, black);
}

.tasks {
  min-height: 200px;
  margin-top: 5px;
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

form {
  display: flex;
  flex-direction: column;
}

input {
  margin-bottom: 10px;
  padding: 5px 10px;
  outline: none;
  border: 1px solid white;
  border-radius: 5px;
  background-color: black;
  color: white;
}

button {
  padding: 5px 10px;
  border: 1px solid white;
  border-radius: 5px;
  outline: none;
  background-color: black;
  color: white;
  cursor: pointer;
  transition: transform 0.3s;
}

button:active {
  transform: scale(1.2);
}

#lastInput {
  width: 300px;
  margin: auto;
}
</style>

