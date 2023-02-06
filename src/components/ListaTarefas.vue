<template>
    <div>
        <p class="titulo">O que você tem para fazer?</p>
    </div>
    <div class="tarefa">
        <input 
        id="input-task"
        v-model="nomeDaTarefa"
        type="text"
        > 
        <button
            id="save-button"
            @click="salvaTarefa()"
            >
             Salvar
        </button>
    </div>
    <div 
        class="lista-itens" 
        v-for="(tarefa, index) in listaTarefas"
        :key="tarefa.index"
    >
    <button id="delete-button" @click="removeTarefa(index)">&#10060;</button>
    <button id="complete-button" @click="concluiTarefa(index)">&#9989;</button>

        {{ tarefa.nomeDaTarefa }}
    
    </div>
<h2>Things done:</h2>
    <div class="lista-tarefas-completas"
        v-for="tarefaCompleta in listaTarefasCompletadas"
        :key="tarefaCompleta.index"
    >

    <button id="delete-button" @click="removeTarefaCompletada(index)">&#10060;</button>
    {{ tarefaCompleta.nomeDaTarefa2 }}

    </div>
</template>

<script>
export default {
    data() {
        return {
            nomeDaTarefa: '',
            listaTarefas: [
                {nomeDaTarefa: 'arrumar'},
                {nomeDaTarefa: 'comer'},
                {nomeDaTarefa: 'dormir'},
                {nomeDaTarefa: 'dialogar'},
                {nomeDaTarefa: 'cantar'},
            ], 
            
            nomeDaTarefa2: '',

            listaTarefasCompletadas: [
                {nomeDaTarefa2: 'task 1'},
                {nomeDaTarefa2: 'task 2'},
            ],


        }
    },

    methods: {
        salvaTarefa(){

            if(this.nomeDaTarefa.trim() === ''){
                return;
            }

            this.listaTarefas.push({
                nomeDaTarefa: this.nomeDaTarefa
            })
            this.nomeDaTarefa= '' 
        },

        removeTarefa(index){
            this.listaTarefas.splice(index, 1);

        },

        removeTarefaCompletada(index){
            this.listaTarefasCompletadas.splice(index, 1);
        },

        concluiTarefa(){
            this.listaTarefasCompletadas.push({
                nomeDaTarefa: this.nomeDaTarefa2
            })
            this.removeTarefa(this.index);
        }
    },
}
</script>

<style>
    .titulo, .tarefa{
        font-size: 28px;
        padding: 3px;
    }

    .lista-itens{
        background: rgb(7, 213, 249);
        color: rgb(8, 8, 8);
        border-radius: 30px;
        border: black solid 3px;
        margin: 8px 0 8px 0;
        padding: 15px;
        text-align: left;
        width: 740px;
        height: 15px;
        display: inline-block;
        
    }

    .lista-tarefas-completas{
        background: rgb(110, 116, 117);
        color: rgb(8, 8, 8);
        border-radius: 30px;
        border: black solid 3px;
        margin: 8px 0 8px 0;
        padding: 15px;
        text-align: left;
        text-decoration: line-through;
        width: 740px;
        height: 15px;
        display: inline-block;
        
    }

    #save-button{
        background-color: blue; 
        color: rgb(247, 94, 39);
        font-weight: bold;
        text-align: center;
        font-size: 28px;
        margin-top: 15px;
        margin-left: 20px;
    }

    #save-button:hover{
        font-size: 32px;
        background-color: black;
        color: white;
        
    }

    #input-task{
        font-size: 30px;
        width: 40%;
    }

    #delete-button{
        background-color: black;
        color: white;
        margin: 0px 0px 0px 690px;
        display: inline-block;
        position: absolute;
    }

    #delete-button:hover{
        background-color: black;
        color: red;
        font-size: 15px;
    }

</style>