<template>
    <div id="arrayToDo">
        <TodoListPadrao :list="list" @mudar-risco="mudarRisco" v-for="list in data" :key="list.id"/>
    </div>
</template>
<script>
import TodoListPadrao from '../components/TodoListPadrao.vue';
export default {
    components:{
        TodoListPadrao
    },
    data(){
        return{
        check1: true,
        riscadoOuNao: 'riscado',
        data: null
        }
    },
    methods: {
        async salvarList(idList){
        this.data.forEach(el=>{
            delete el.edit;
            el.allItems.forEach(it=>{
            delete it.edit;
            });
        });
        const listToUpdate = this.data.find(list=>list.id === idList);
        const dataJson = JSON.stringify(listToUpdate);
        const req = await fetch(`https://makeyourburger-xxqo.onrender.com/allTodoList/${idList}`,{
            method: "PUT",
            headers: { "Content-Type" : "application/Json" },
            body: dataJson
        })
        const res = await req.json();
        console.log(res);
        this.getAllLists();
        },
        async deleteList(idList){

        },
        inicioEditTitle(idList){
        this.data.forEach(el=>{
            if(el.id == idList){
            el.edit = true;
            }
        });
        },
        fimEditTitle(list){
        this.data.forEach(el=>{
            if(el.id == list.id){
            el.title = list.title;
            el.edit = false;
            }
        });
        },
        inicioEditItem(idList, item){
        this.data.forEach(el=>{
            if(el.id == idList){
            el.allItems.forEach(it=>{
                if(it.id == item.id){
                it.edit = true;
                }
            })
            }
        });    
        },
        fimEditItem(idList, item){
        this.data.forEach(el => {
            if(el.id == idList){
            el.allItems.forEach(it =>{
                if(it.id == item.id){
                it.text = item.text;
                it.edit = false;
                }
            });
            }        
        });
        },
        async deleteItem(idList, item){

        this.data.forEach(el=>{
            if(el.id == idList){
            let index = el.allItems.findIndex(obj=>obj.id == item.id);
            el.allItems.splice(index, 1);
            }
        });
        },
        async mudarRisco(dados){
            let item = dados.p2;
            let idList = dados.p1;
            item.state = !item.state;
            
            this.data.forEach(el => {
                if(el.id == idList){
                el.allItems.forEach(it =>{
                    if(it.id == item.id){
                    it.state = !it.state;
                    }
                });
                }        
            });

            const listToUpdate = this.data.find(list=>list.id === idList);
            
            const dataJson = JSON.stringify({allItems: listToUpdate.allItems});
            const req = await fetch(`https://makeyourburger-xxqo.onrender.com/allTodoList/${idList}`,{
                method: "PATCH",
                headers: { "Content-Type" : "application/Json" },
                body: dataJson
            })
            const res = await req.json();
            console.log("teste")
            console.log(res);
            this.getAllLists();
        },

        teste(a){alert(a?'editar':'deletar')},

        async getAllLists(){
        let req = await fetch("https://makeyourburger-xxqo.onrender.com/allTodoList");
        let data = await req.json();

        data.forEach(el=>{
            el.edit = false;
            el.allItems.forEach(it=>{
            it.edit = false;
            })
        });
        this.data = data;
        console.log(data);
        }
    },
    mounted(){
        this.getAllLists();
    }
}
</script>

<style scoped>
  #arrayToDo{
    max-width: 80%;
    margin: 100px auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
  }
  .cardsPadrao{
    background: #D9D9D9;
    color: #222;
    overflow-y: scroll;
    height: 300px;
  }
  .kitTitleList{
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }
  .feedbackSucess{
    background: rgb(124, 226, 124);
  }
  .headerCard{
    display: flex;
    justify-content: space-between;
    align-content: center;
    align-items: center;
  }
  .titleCard{
    font-size: 25px;
    font-weight: bold;
  }
  .labelPadrao{
    color: #222!important;
    font-weight: 600;
  }
  .riscado{
    text-decoration: line-through;
    color: #807f7f !important;
  }
  .item{
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-bottom: 10px;
  }
  .v-checkbox{
    margin-right: 10px;
  }
  .displayNone{
    display: none;
  }
  .maeBtnSalvar{
    width: 100%;
    display: flex;
    justify-content: space-around;
    margin-bottom: 15px;
  }
  .btnEndCard{
    width: 40%;
  }
  .excluir{
    color: #D9D9D9;
    background: #222;
  }
  .excluir:hover{
    background: rgb(136, 2, 2);
  }
  .salvar:hover{
    background: #2222223d;
  }
  
</style>
