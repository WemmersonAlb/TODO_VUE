<script>
export default {
  data(){
    return{
      check1: true,
      label1: 'teste',
      riscadoOuNao: 'riscado'
    }
  },
  methods: {
    mudarRisco(){
      this.riscadoOuNao = this.riscadoOuNao == 'riscado' ? 'nao' : 'riscado';
    },
    teste(a){alert(a?'editar':'deletar')}
  },
}
</script>

<template>
  <div id="arrayToDo">
    <v-card max-width="300px" class="cardsPadrao">
      <v-template>
        <v-card-item>
          <h1 class="titleCard">Lista de Compras</h1>
        </v-card-item>
        <v-card-item>
          <div class="item">
            <v-checkbox v-model="check1"  @change="mudarRisco" hide-details >
              <template v-slot:label>
                <span :class="[riscadoOuNao, 'labelPadrao']">{{ label1 }}</span>
              </template>
            </v-checkbox>
            <v-icon icon="mdi-pencil" color="grey-darken-4" @click="teste(true)"></v-icon>
            <v-icon icon="mdi-delete" color="grey-darken-4" @click="teste(false)"></v-icon>
          </div>
          <div class="item">
            <v-checkbox v-model="check1"  @change="mudarRisco" hide-details>
              <template v-slot:label>
                <span :class="[riscadoOuNao, 'labelPadrao']">{{ label1 }}</span>
              </template>
            </v-checkbox>
          </div>
          <div class="item">
            <v-checkbox v-model="check1"  @change="mudarRisco" hide-details>
              <template v-slot:label>
                <span :class="[riscadoOuNao, 'labelPadrao']">{{ label1 }}</span>
              </template>
            </v-checkbox>
          </div>
          <div class="item">
            <v-checkbox v-model="check1"  @change="mudarRisco" hide-details>
              <template v-slot:label>
                <span :class="[riscadoOuNao, 'labelPadrao']">{{ label1 }}</span>
              </template>
            </v-checkbox>
          </div>

        </v-card-item>
      </v-template>
    </v-card>
  </div>
</template>
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
</style>
