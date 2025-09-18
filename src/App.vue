<!-- <template>
  <p :style="{color: count > 5 ? 'red' : 'green'}">Compteur : {{ count }}</p>
  <button v-on:click="increment">Incrementer</button>
  <button v-on:click="decrement">Decrementer</button>
  <hr>
  <button @click="sortMovies">Reorganiser</button>
  <form action="" @submit.prevent="addMovie">
    <input type="text" placeholder="Nouveau film"
    v-model="movieName">
    <button>Ajouter</button>
  </form>
  <ul>
    <li v-for="movie in movies">
      {{ movie }} <button @click="deleteMovie(movie)"> Supp</button>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue'

const movieName = ref('')

const movies = ref([
  'SnowFall',
  'TVD',
  'Originals',
  'BMF'
])

const addMovie = () => {
  event.preventDefault()
  movies.value.push(movieName.value)
  movieName.value = ''
}

const deleteMovie = (movie) => {
  movies.value = movies.value.filter(m => m != movie)
}

const sortMovies = () => {
  movies.value.sort((a,b) => a > b ? 1 : -1)
}

const count = ref(0)

const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}

</script> -->

<!-- todo list -->
<!-- <template>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input v-model="newtodo" type="text" placeholder="Tache a faire">
      <button :disabled="newtodo.length == 0">Ajouter</button>
    </fieldset>
  </form>
  <div v-if="todos.length == 0">Vous n'avez pas de tache a faire</div>
  <div v-else>
    <ul>
      <li 
        v-for="todo in sortedTodos()" 
        :key="todo.date"
        :class="{completed: todo.completed}"
        >
        <label>
          <input type="checkbox" v-model="todo.completed">
          {{ todo.title }}
        </label>
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les taches completees
    </label>
    <p v-if="tacheRestantes > 0">
      {{ tacheRestantes }} tache{{ tacheRestantes > 1 ? 's' : '' }} a faire
    </p>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const newtodo = ref('')
const todos = ref([
  {
    title: "coucou",
    completed: true,
    date: 1
  },
  {
    title: "mama",
    completed: false,
    date: 2
  }
])
const addTodo = () => {
  todos.value.push({
    title: newtodo.value,
    completed: false,
    date: 1
  })
  newtodo.value = ''
}
const hideCompleted = ref(false)
const sortedTodos = () => {
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
  if (hideCompleted.value == true){
    return sortedTodos.filter(t => t.completed == false)
  }
  return sortedTodos
}
const tacheRestantes = computed(() => {
  return todos.value.filter(t => t.completed == false).length
})
</script>

<style>
.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style> -->

<!-- calculatrice simple -->
<!-- <template>
  <div class="calculator">
    <input type="text" v-model="current" disabled />

    <div class="buttons">
      <button @click="clear">C</button>
      <button @click="append('/')">/</button>
      <button @click="append('*')">*</button>
      <button @click="backspace">←</button>
    </div>

    <div class="buttons">
      <button @click="append('7')">7</button>
      <button @click="append('8')">8</button>
      <button @click="append('9')">9</button>
      <button @click="append('-')">-</button>
    </div>

    <div class="buttons">
      <button @click="append('4')">4</button>
      <button @click="append('5')">5</button>
      <button @click="append('6')">6</button>
      <button @click="append('+')">+</button>
    </div>

    <div class="buttons">
      <button @click="append('1')">1</button>
      <button @click="append('2')">2</button>
      <button @click="append('3')">3</button>
      <button class="equal" @click="calculate">=</button>
    </div>

    <div class="buttons">
      <button @click="append('0')">0</button>
      <button @click="append('.')">.</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const current = ref("");

function append(value) {
  current.value += value;
}

function clear() {
  current.value = "";
}

function backspace() {
  current.value = current.value.slice(0, -1);
}

function calculate() {
  try {
    current.value = eval(current.value).toString();
  } catch (e) {
    current.value = "Erreur";
  }
}
</script>

<style scoped>
.calculator {
  width: 220px;
  margin: 50px auto;
  padding: 15px;
  border: 2px solid #000000;
  border-radius: 10px;
  text-align: center;
  background: #f9f9f9;
}

input {
  width: 95%;
  height: 50px;
  font-size: 20px;
  text-align: right;
  margin-bottom: 15px;
  border-radius: 5px;
  border: 1px solid #000000;
  padding-right: 10px;
}

.buttons {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

button {
  width: 45px;
  height: 45px;
  font-size: 18px;
  border: none;
  border-radius: 5px;
  background: #000000;
  cursor: pointer;
  transition: 0.2s;
}

button:hover {
  background: #bbb;
}

.equal {
  background: #4caf50;
  color: white;
}

.equal:hover {
  background: #96ff9b;
}
</style> -->

<!-- quiz avec json  -->
<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>
        Impossible de charger le quiz
      </p>
    </div>
    <div :aria-busy="state === 'loading'">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Quiz from './components/Quiz.vue';

const quiz = ref(null)
const state = ref('loading')

onMounted(() => {
  fetch('/quizz.json')
    .then(r => {
      if (r.ok) {
        return r.json()
      }
      throw new Error('Impossible de recuperer le json')
    })
    .then(data => {
      quiz.value = data
      state.value = 'idle'
    })
    .catch(e => {
      state.value = 'error'
    })
})
</script>

<style>
.container{
  margin-top: 2rem;
}
</style>