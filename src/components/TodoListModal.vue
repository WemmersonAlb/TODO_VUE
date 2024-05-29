<template>
    <div>
        <v-card class="cardsPadrao">
            <v-container>
              <v-card-item>
                <v-card-title>
                  <div class="headerCard">
                    <div class="kitTitleList">
                      <h1 class="titleCard" v-show="!list.edit">{{list.title}}</h1>
                      <v-text-field v-model="list.title" v-show="list.edit" @keydown.enter="fimEditTitle(list)" @blur="fimEditTitle(list)"></v-text-field>
                      <v-icon icon="mdi-pencil" color="grey-darken-4" @click="inicioEditTitle(list.id)"></v-icon>
                    </div>
                    <v-icon icon="mdi-close" @click="fecharModal(list.id)" slot="prepend-icon" color="grey-darken-4"></v-icon>
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
                <v-text-field v-model="list.novoItemNome" v-show="list.novoItemTextShow" @keydown.enter="fimCriarItem(list.id)" @blur="fimCriarItem(list.id)"></v-text-field>
                <v-icon color="grey-darken-4" icon="mdi-plus" @click="criarItem(list.id)" v-show="!list.novoItemTextShow"></v-icon>
              </v-card-item>
              <v-card-actions class="maeBtnSalvar">
                <v-btn variant="outlined" class="btnEndCard salvar" elevation="4" @click="salvarList(list.id)">Salvar</v-btn>
                <v-btn class="btnEndCard excluir" elevation="4" @click="deleteList(list.id)">Excluir</v-btn>
              </v-card-actions>
            </v-container>
          </v-card>
    </div>
</template>
<script>
export default {
    name:'TodoListModal',
    props:{
        list:Object
    },
    methods:{
        fimEditTitle(list){
            this.$emit('fim-edit-title', list);
        },
        inicioEditTitle(idList){
            this.$emit('inicio-edit-title', idList);
        },
        teste(a){
            this.$emit('teste', a);
        },
        mudarRisco(idList, item){
            this.$emit('mudar-risco', {p1: idList, p2: item});
        },
        fimEditItem(idList, item){
            this.$emit('fim-edit-item', {p1: idList, p2: item});
        },
        inicioEditItem(idList, item){
            this.$emit('inicio-edit-item', {p1: idList, p2: item});
        },
        deleteItem(idList, item){
            this.$emit('delete-item', {p1: idList, p2: item});
        },
        fimCriarItem(idList){
            this.$emit('fim-criar-item', idList);
        },
        criarItem(idList){
            this.$emit('criar-item', idList);
        },
        salvarList(idList){
            this.$emit('salvar-list', idList);
        },
        deleteList(idList){
            this.$emit('delete-list', idList);
        },
        fecharModal(idList){
            this.$emit('fechar-modal', idList);
        }       

    }
}
</script>
<style scoped>

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
  ::-webkit-scrollbar {
    width: 7px;
  }
  
  ::-webkit-scrollbar-track {
    box-shadow: inset 0 0 5px grey; 
    border-radius: 10px;
    background: #D9D9D9;
  }
   
  ::-webkit-scrollbar-thumb {
    background: #222; 
    border-radius: 10px;
  }
  /* Exibição padrão */
  .cardsPadrao{
    background: #D9D9D9;
    color: #222;
    overflow-y: scroll;
    height: 400px;
    width: 350px;
    box-shadow: 2px 10px 5px rgba(0, 0, 0, 0.63);
  }
  /* Telas pequenas e mobile */
  @media (max-width:700px){
    .cardsPadrao{
      height: 300px;
      width: 280px;
    }
    .labelPadrao{
      font-size: 14px;
    }
  }
</style>