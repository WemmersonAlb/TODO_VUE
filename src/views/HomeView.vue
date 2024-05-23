
<template>
  <div id="arrayToDo">
    <v-card max-width="300px" class="cardsPadrao" v-for="list in data" :key="list.id">
      <v-template>
        <v-card-item>
          <h1 class="titleCard">{{list.title}}</h1>
        </v-card-item>
        <v-card-item>
          <div class="item" v-for="item in list.allItems" :key="item.id">
            <v-checkbox  @change="mudarRisco(list.id, item)" hide-details v-model="item.state" v-show="!item.edit">
              <template v-slot:label>
                <span :class="[item.state? 'riscado': '', 'labelPadrao']">{{ item.text }}</span>
              </template>
            </v-checkbox>
            <v-text-field v-model="item.text" v-show="item.edit"></v-text-field>
            <v-icon icon="mdi-pencil" color="grey-darken-4" @click="editItem(list.id, item)"></v-icon>
            <v-icon icon="mdi-delete" color="grey-darken-4" @click="deleteItem(list.id, item)"></v-icon>
          </div>
        </v-card-item>
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
      data: null,
      items:[]
    }
  },
  methods: {
    editItem(idList, item){
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
    async deleteItem(idList, item){

      this.data.forEach(el=>{
        if(el.id == idList){
          let index = el.allItems.findIndex(obj=>obj.id == item.id);
          el.allItems.splice(index, 1);
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
        el.allItems.forEach(it=>{
          it.edit = false;
        })
      });

      this.data = data;
      console.log(data)
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
</style>
