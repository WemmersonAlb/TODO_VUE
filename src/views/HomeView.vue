
<template>
  <div id="arrayToDo">
    <v-card max-width="300px" class="cardsPadrao" v-for="list in data" :key="list.id">
      <v-template>
        <v-card-item>
          <v-card-title>
            <div class="headerCard">
              <div class="kitTitleList">
                <h1 class="titleCard" v-show="!list.edit">{{list.title}}</h1>
                <v-text-field v-model="list.title" v-show="list.edit" @keydown.enter="fimEditTitle(list)" @blur="fimEditTitle(list)"></v-text-field>
                <v-icon icon="mdi-pencil" color="grey-darken-4" @click="inicioEditTitle(list.id)"></v-icon>
              </div>
              <v-icon icon="mdi-close" @click="teste(a)" slot="prepend-icon" color="grey-darken-4"></v-icon>
            </div>
          </v-card-title>
        </v-card-item>
        <v-card-item>
          <div class="item" v-for="item in list.allItems" :key="item.id">
            <v-checkbox  @change="mudarRisco(list.id, item)" hide-details v-model="item.state" v-show="!item.edit">
              <template v-slot:label>
                <span :class="[item.state? 'riscado': '', 'labelPadrao']">{{ item.text }}</span>
              </template>
            </v-checkbox>
            <v-text-field v-model="item.text" v-show="item.edit" @keydown.enter="fimEditItem(list.id, item)" @blur="fimEditItem(list.id, item)"></v-text-field>
            <v-icon icon="mdi-pencil" color="grey-darken-4" @click="inicioEditItem(list.id, item)"></v-icon>
            <v-icon icon="mdi-delete" color="grey-darken-4" @click="deleteItem(list.id, item)"></v-icon>
          </div>
        </v-card-item>
        <v-card-actions class="maeBtnSalvar">
          <v-btn variant="outlined" class="btnEndCard salvar" elevation="4" @click="salvarList(list.id)">Salvar</v-btn>
          <v-btn class="btnEndCard excluir" elevation="4" @click="deleteList(list.id)">Excluir</v-btn>
        </v-card-actions>
      </v-template>
    </v-card>
  </div>
</template>
<script>
export default {
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
    async mudarRisco(idList, item){
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
