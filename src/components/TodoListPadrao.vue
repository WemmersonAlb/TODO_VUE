<template>
    <div>
        <v-card width="280px" class="cardsPadrao" >
            <v-container>
              <v-card-item>
                <v-card-title @click="mostrarModal(list.id)">
                      <h1 class="titleCard">{{list.title}}</h1>                      
                </v-card-title>
              </v-card-item>
              <v-card-item>
                <div class="item" v-for="item in list.allItems" :key="item.id">
                  <v-checkbox  @change="mudarRisco(list.id, item)" hide-details v-model="item.state" >
                    <template v-slot:label>
                      <span :class="[item.state? 'riscado': '', 'labelPadrao']">{{ item.text }}</span>
                    </template>
                  </v-checkbox>
                </div>
              </v-card-item>
            </v-container>
          </v-card>
    </div>
</template>
<script>
export default {
    name:'TodoListPadrao',
    props:{
        list: Object,
    },
    methods: {
        mudarRisco(idList, item){
            this.$emit('mudar-risco', {p1: idList, p2: item});
        },
        mostrarModal(idList){
          this.$emit('mostrar-modal', idList);
        }
    },
}
</script>
<style scoped>
.v-card-title{
  cursor: pointer;
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
  /*Pequenas telas e mobile*/
  @media (max-width:700px){
    .labelPadrao{
      font-size: 14px;
    }
  }
</style>