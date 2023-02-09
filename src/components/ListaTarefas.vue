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

<script>
export default {
    name: 'AçõesDeTarefa',
    data() {
        return {
            isDarkMode: false,

            nomeDaTarefa: '',
            listaTarefas: [
                { nomeDaTarefa: 'Começar a ler um livro (Exemplo)' },
                { nomeDaTarefa: 'Meditar 5 minutos (Exemplo)' },
            ],


            listaTarefasCompletadas: [
                { nomeDaTarefa: 'Aqui ficam as tarefas já realizadas!' },
            ],

        }
    },

    methods: {
        toggle(currentTheme) {
            this.isDarkMode = currentTheme
            document.documentElement.className = currentTheme
            localStorage.setItem('theme', currentTheme)
        },

        salvaTarefa() {
            if (this.nomeDaTarefa.trim() === '') {
                return;
            }

            this.listaTarefas.push({
                nomeDaTarefa: this.nomeDaTarefa
            })

            this.nomeDaTarefa = ''

        },


        removeTarefa(i) {
            this.listaTarefas.splice(i, 1);
        },

        concluiTarefa(i) {

            this.listaTarefasCompletadas.push({
                nomeDaTarefa: ' ' + this.listaTarefas[i].nomeDaTarefa
            });

            this.removeTarefa(i)
        },

        retornaTarefaCompletada(i) {
            this.listaTarefas.push({
                nomeDaTarefa: ' ' + this.listaTarefasCompletadas[i].nomeDaTarefa
            });

            this.removeTarefaCompletada(i)
        },

        removeTarefaCompletada(i) {
            this.listaTarefasCompletadas.splice(i, 1);

        },

    },

    watch: {
        listaTarefas: {
            handler() {
                localStorage.setItem('listaTarefas', JSON.stringify(this.listaTarefas))
            },
            deep: true
        },

        listaTarefasCompletadas: {
            handler() {
                localStorage.setItem('listaTarefasCompletadas', JSON.stringify(this.listaTarefasCompletadas))
            },
            deep: true
        }

    },

    mounted() {
        if (localStorage.getItem('listaTarefas', 'listaTarefasCompletadas')) {
            this.listaTarefas = JSON.parse(localStorage.getItem('listaTarefas'))
            this.listaTarefasCompletadas = JSON.parse(localStorage.getItem('listaTarefasCompletadas'))
        }
        this.toggle(JSON.parse(localStorage.getItem('theme')))
    },

}
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