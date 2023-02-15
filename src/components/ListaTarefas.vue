<template>

    <body>

        <button class="theme-button" @click="toggle(!isDarkMode)">&#128161;</button>

        <div class="titulo">
            <p>O que você tem para fazer?</p>
        </div>
        <div class="tarefa">
            <input id="input-task" v-model="nomeDaTarefa" type="text">
            <button id="save-button" @click="salvaTarefa()">
                Salvar
            </button>
        </div>
        <div class="lista-itens" v-for="(tarefa, i) in listaTarefas" :key="tarefa.i">
            <button id="delete-button" @click="removeTarefa(i)">&#10060;</button>
            <button id="complete-button" @click="concluiTarefa(i)">&#9989;</button>

            {{ tarefa.nomeDaTarefa }}

        </div>
        <h2>Concluídas:</h2>
        <div class="lista-tarefas-completas" v-for="(tarefaCompleta, j) in listaTarefasCompletadas"
            :key="tarefaCompleta.j">

            <button id="delete-button" @click="removeTarefaCompletada(j)">&#10060;</button>
            <button id="return-button" @click="retornaTarefaCompletada(j)">&#8617;</button>
            {{ tarefaCompleta.nomeDaTarefa }}

        </div>
    </body>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue';

// state
const isDarkMode = ref(false);
const nomeDaTarefa = ref('');
const listaTarefas = ref([
    { nomeDaTarefa: 'Começar a ler um livro (Exemplo)' },
    { nomeDaTarefa: 'Meditar 5 minutos (Exemplo)' },
]);

const listaTarefasCompletadas = ref([
    { nomeDaTarefa: 'Aqui ficam as tarefas já realizadas!' },
]);

// functions
function toggle(currentTheme) {
    isDarkMode.value = currentTheme;
    document.documentElement.className = currentTheme;
    localStorage.setItem('theme', currentTheme);
}

function salvaTarefa(){
    if (nomeDaTarefa.value.trim() === '') return;
    listaTarefas.value.push({
        nomeDaTarefa: this.nomeDaTarefa
    })    
    nomeDaTarefa.value = ''
}

function removeTarefa(i){
    listaTarefas.value.splice(i, 1);
}

function concluiTarefa(i) {
    listaTarefasCompletadas.value.push({
        nomeDaTarefa: ' ' + listaTarefas.value[i].nomeDaTarefa
    });
    removeTarefa(i)
}

function retornaTarefaCompletada(i) {
    listaTarefas.value.push({
        nomeDaTarefa: ' ' + listaTarefasCompletadas.value[i].nomeDaTarefa
    });
    removeTarefaCompletada(i)
}

function removeTarefaCompletada(i) {
    listaTarefasCompletadas.value.splice(i, 1);
}


// watchers
watch(() => listaTarefas, (newList) => {
    localStorage.setItem('listaTarefas', JSON.stringify(newList.value))
}, {deep: true})
watch(() => listaTarefasCompletadas, (newList) => {
    localStorage.setItem('listaTarefasCompletadas', JSON.stringify(newList.value))
}, {deep: true})

// lifecycle
onMounted(() => {
    const todoList = localStorage.getItem('listaTarefas')
    const todoListCompleted = localStorage.getItem('listaTarefasCompletadas')
    const preferedTheme = localStorage.getItem('theme');
    if (todoList) {
        listaTarefas.value = JSON.parse(localStorage.getItem('listaTarefas'))
    }
    if(todoListCompleted){
        listaTarefasCompletadas.value = JSON.parse(localStorage.getItem('listaTarefasCompletadas'))
    }
    if (!preferedTheme && window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        // dark mode
        toggle(true);
    }
    else{
        toggle(JSON.parse(preferedTheme))
    }
})
</script>

<style>
:root.true {
    --b: #c0c0c0;
    --b2: hsl(0, 2%, 35%);
    --bg: black;
    --text: white;
    --border: #faf7f7 solid 3px;
    --bg-btn-save: rgb(255, 255, 255);
    --text-btn-save: rgb(0, 0, 0);
}

:root.false {
    --b: #48b7f7;
    --b2: #2574a1;
    --bg: white;
    --text: black;
    --border: black solid 3px;
    --bg-btn-save: blue;
    --text-btn-save: rgb(247, 94, 39);
}

.theme-button {
    width: 40px;
    height: 40px;
}

body {
    background-color: var(--bg);
}

.titulo,
h2,
.tarefa {
    font-size: 28px;
    padding: 3px;
    color: var(--text);
}

.lista-itens {
    background-color: var(--bg);
    color: var(--text);
    background: var(--b);
    border-radius: 30px;
    border: var(--border);
    margin: 8px 0 8px 0;
    padding: 15px;
    text-align: left;
    width: 740px;
    height: 15px;
    display: inline-block;

}

.lista-tarefas-completas {
    background-color: var(--bg);
    color: var(--text);
    background: var(--b2);
    border-radius: 30px;
    border: var(--border);
    margin: 8px 0 8px 0;
    padding: 15px;
    text-align: left;
    text-decoration: line-through;
    width: 740px;
    height: 15px;
    display: inline-block;

}

#save-button {
    background-color: var(--bg-btn-save);
    color: var(--text-btn-save);
    font-weight: bold;
    text-align: center;
    font-size: 28px;
    margin-top: 15px;
    margin-left: 20px;
}

#save-button:hover {
    font-size: 30px;

}

#input-task {
    font-size: 30px;
    width: 40%;
}

#delete-button {
    background-color: var(--bg);
    color: var(--text);
    margin: 0px 0px 0px 690px;
    display: inline-block;
    position: absolute;
}

#delete-button:hover {
    font-size: 15px;
}
</style>