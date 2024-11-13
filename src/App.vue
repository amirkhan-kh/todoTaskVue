<template>
  <div class="flex flex-col justify-between min-h-screen mx-auto bg-[#050A14]">
    <Header />
    <main class="mx-auto">
      <section>
        <div class="container">
          <form
            @submit.prevent="addTask"
            action="#"
            class="w-[400px] bg-[rgb(29,29,29)] m-auto p-4 rounded-sm border border-white"
          >
            <label for="task-title" class="block mb-2">
              <p>Enter task title</p>
              <input
                v-model="taskTitle"
                type="text"
                placeholder="Enter task title"
                class="w-full h-7 rounded-xl py-4 px-6"
                id="task-title"
              />
            </label>
            <label for="task-deadline" class="block mb-2">
              <p>Enter task deadline</p>
              <input
                v-model="taskDeadline"
                type="date"
                placeholder="Enter task deadline"
                class="w-full h-7 rounded-xl py-4 px-6"
                id="task-deadline"
              />
            </label>
            <button
              type="submit"
              class="bg-[rgb(245,124,0)] p-1 text-black rounded-xl"
            >
              Add Task
            </button>
          </form>
          <div
            v-if="!tasks.length"
            class="mx-auto w-[400px] bg-[rgb(29,29,29)] border border-white p-3 my-2 rounded-sm"
          >
            <h1 class="text-white text-center">Not Found Tasks!</h1>
          </div>
          <ul
            v-else
            class="mx-auto bg-[rgb(29,29,29)] border border-white p-3 my-2 rounded-sm w-[400px]"
          >
            <li
              v-for="(el, index) in tasks"
              class="relative bg-white p-3 rounded-md my-2"
            >
              <strong class="absolute top-0">#{{ index + 1 }}</strong>
              <small v-if="el.completed" class="block my-2 line-through">{{
                el.title
              }}</small>
              <small v-else class="block my-2">{{ el.title }}</small>
              <div class="flex gap-2">
                <button
                  @click="() => deleteTask(el.id)"
                  class="bg-red-600 focus:bg-red-400 rounded-lg px-3 text-white"
                >
                  X
                </button>
                <button
                  @click="taskCompleted(el.id)"
                  :disabled="el.completed"
                  class="bg-green-600 focus:bg-green-400 rounded-lg px-3 text-white"
                >
                  DONE
                </button>
              </div>
            </li>
          </ul>
        </div>
      </section>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { ref } from "vue";
import { v4 as uuidv4 } from "uuid";
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";
import Header from "./components/layout/header.vue";
import Footer from "./components/layout/footer.vue";
const tasks = ref([]);
const taskTitle = ref("");
const taskDeadline = ref("");

const addTask = () => {
  if (taskTitle.value.trim().length && taskDeadline.value) {
    const newTask = {
      id: uuidv4(),
      creat_at: Date.now(),
      title: taskTitle.value,
      deadline: taskDeadline.value,
      completed: false,
    };
    tasks.value.push(newTask);
    taskTitle.value = "";
    taskDeadline.value = "";
    toast.success("Task added successfully!", {
      duration: 1000,
      autoClose: 1000,
    });
  } else {
    alert("Please fill all fields");
    toast.error("Please fill all fields!", {
      duration: 1000,
      autoClose: 1000,
    });
  }
};
const deleteTask = (id) => {
  tasks.value = tasks.value.filter((item) => item.id != id);
  toast.error("Task deleted successfully!", {
    duration: 1000,
    autoClose: 1000,
  });
};
const taskCompleted = (id) => {
  tasks.value.forEach((item) => {
    if (item.id === id) {
      item.completed = true;
    }
  });
  toast.warning("Task completed successfully!", {
    duration: 1000,
    autoClose: 1000,
  });
};
</script>

<style lang="css" scoped>
header > h1 {
  font-family: "Orbitron", sans-serif;
}
form,
label,
p,
h1 {
  font-family: "Orbitron", sans-serif;
  color: #fff;
}
footer > p {
  font-family: "Orbitron", sans-serif;
}
li,
strong {
  color: black;
}
input {
  color: #000;
  font-family: sans-serif;
}
</style>
