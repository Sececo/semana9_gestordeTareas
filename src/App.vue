<template>
  <div class="container">
    <h1>Gestor de Tareas</h1>
    <input
      v-model="newTodo"
      placeholder="Ingresa el nombre de la tarea"
      @keyup.enter="addTodo" minlength='1' maxlength='35'
    />
    <button @click="addTodo">Agregar</button>
  </div>
  <hr />
  <!-- contenido que se muestra si el total de tareas (todo+doing+done>0)
  esta suma es para que al cambiar de arrays los task no tome el valor de 0 y el template
  desaparezca aun teniendo tareas-->
  <div class="main" v-if="total > 0">
    <!-- section con display flex para un estilo tipo trello -->
    <!-- section To-Do -->
    <section class="todo">
      <h2>To-Do</h2>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          {{ index + 1 }}. {{ todo }}
          <!-- boton para llamar la funcion correspondiente para cambiar de To-do a Doing (de un array al otro) -->
          <!-- importante importar el index en la funcion o sino se moveran las "cajas" de un array a otro pero vacias -->
          <button @click="moveDoing(index)">➡️</button>
        </li>
      </ul>
    </section>
    <!-- Section Doing -->
    <section class="doing">
      <h2>Doing</h2>
      <ul>
        <!-- iteramos el array doings -->
        <li v-for="(doing, index) in doings" :key="index">
          {{ index + 1 }}. {{ doing }}
          <!-- boton para llamar la funcion correspondiente para cambiar de doing a done -->
          <button @click="moveDone(index)">➡️</button>
        </li>
      </ul>
    </section>
    <!-- Section Done -->
    <section class="done">
      <h2>Done</h2>
      <ul>
        <!-- iteramos el array dones -->
        <li v-for="(done, index) in dones" :key="index">
          {{ index + 1 }}. {{ done }}
        </li>
      </ul>
    </section>
    <!-- total de tareas, usando el atributo position para que no afecte los otros elementos -->
    <p class="main-p">Total de Tareas: {{ total }}</p>
  </div>
  <!-- div si no hay tareas agregadas -->
  <div v-else>
    <p>No hay Tareas Registradas 🥳</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
// modelo: variables reactivas
const newTodo = ref('');
// crear arrays para las tareas
const todos = ref([]);
const doings = ref([]);
const dones = ref([]);
// crear variable para el total de tareas
const total = ref(0);

// viewModel: logica del componente
const addTodo = () => {
  const name = newTodo.value.trim();
  if (name !== '') {
    todos.value.push(name);
    console.log(name);
    console.log(todos.value);
    newTodo.value = '';
  }
  // actualizamos el total de tareas
  totalTasks();
};

// funciones para mover tareas entre listas
const moveDoing = (index) => {
  const task = todos.value[index];
  // agregamos la tarea al array doings (array destino)
  doings.value.push(task);
  // eliminamos la tarea del array todos (array origen)
  todos.value.splice(index, 1);
  // actualizamos el total de tareas
  totalTasks();
};

const moveDone = (index) => {
  const task = doings.value[index];
  // agregamos la tarea al array dones (array destino)
  dones.value.push(task);
  // eliminamos la tarea del array doings (array origen)
  doings.value.splice(index, 1);
  // actualizamos el total de tareas
  totalTasks();
};

// funcion para calcular el total de tareas
const totalTasks = () => {
  total.value = todos.value.length + doings.value.length + dones.value.length;
};
</script>

<style scoped>
.container {
  max-width: 100vh;
  margin: 50px auto;
  margin-bottom: 20px;
  font-family: sans-serif;
  color: white;
}


.main {
  color: white;
  display: flex;
  justify-content: space-evenly;
  position: relative;
}
.main-p {
  position: absolute;
  bottom: -30px;
  margin-top: 20px;
  font-weight: bold;
}
.main h2 {
  text-align: center;
  margin: 10px 15px;
}

section {
  min-width: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  padding: 8px;
  width: 80%;
}

button {
  cursor: pointer;
  margin-left: 10px;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  background-color: #cb96ea;
}

ul {
  list-style: none;
  padding: 0;
  width: 95%;
}

li {

  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding: 6px;
  margin-bottom: 10px;

  background-color: #6b4f8a;
  color: white;
  border-radius: 4px;
  font-size: 8px;
}

li button {
  cursor: pointer;
  background-color: transparent;
  border-style: none;
  padding: 0;
  margin: 0;
  font-size: 5vh;
}

</style>
