<template>
    <div>
        <v-card max-width="300px" class="cardsPadrao" >
            <v-template>
              <v-card-item>
                <v-card-title>
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
            </v-template>
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
        }
    },
}
</script>
<style scoped>
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
</style>