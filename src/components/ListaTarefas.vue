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
        v-for="(tarefa, i) in listaTarefas"
        :key="tarefa.i"
    >
    <button id="delete-button" @click="removeTarefa(i)">&#10060;</button>
    <button id="complete-button" @click="concluiTarefa(i)">&#9989;</button>

        {{ tarefa.nomeDaTarefa }}
    
    </div>
<h2>Things done:</h2>
    <div class="lista-tarefas-completas"
        v-for="(tarefaCompleta, j) in listaTarefasCompletadas"
        :key="tarefaCompleta.j"
    >

    <button id="delete-button" @click="removeTarefaCompletada(j)">&#10060;</button>
    <button id="return-button" @click="retornaTarefaCompletada(j)">&#8617;</button>
    {{ tarefaCompleta.nomeDaTarefa }}

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
                {nomeDaTarefa: 'cantar'},
            ], 
            

            listaTarefasCompletadas: [
                {nomeDaTarefa: 'apartando um boi na floresta'},
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

        removeTarefa(i){
            this.listaTarefas.splice(i, 1);

        },
        
        concluiTarefa(i){
            
            this.listaTarefasCompletadas.push({
                nomeDaTarefa:' ' + this.listaTarefas[i].nomeDaTarefa
            });
            
            this.removeTarefa(i)
        },

        retornaTarefaCompletada(i){
            console.log(this.listaTarefasCompletadas[i].nomeDaTarefa)
            
            this.listaTarefas.push({
                nomeDaTarefa:' ' + this.listaTarefasCompletadas[i].nomeDaTarefa
            });
            
            this.removeTarefaCompletada(i)
        },

        removeTarefaCompletada(i){
            this.listaTarefasCompletadas.splice(i, 1);
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
        color: rgb(255, 255, 255);
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