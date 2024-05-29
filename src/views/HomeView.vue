<template>
  <div id="arrayToDo">
      <v-card width="280px" class="cardsCriacao">
        <v-icon icon="mdi-playlist-plus" color="D9D9D9" @click="criarList" class="criarList"></v-icon>
        <p @click="criarList" class="criarList">Criar nova Lista</p>
      </v-card>
      <TodoListPadrao 
        :list="list" 
        @mudar-risco="mudarRisco" 
        @mostrar-modal="mostrarModal"
        v-for="(list, id) in reversedItems()" 
        :key="id"
        class="scrollBar"/>    
      

      <div id="fade"></div>
        

        <MessageFeedback 
          v-show="titleMsg"
          :msg="msg" 
          :tipoStatus="tipoStatus" 
          :titleMsg="titleMsg"/>
        
        <TodoListModal 
          v-show="list.showModal"
          :list="list" 
          v-for="list in data" 
          :key="list.id"
          @fechar-modal="fecharModal"
          @delete-list="deleteList"
          @salvar-list="salvarList"
          @criar-item="criarItem"
          @fim-criar-item="fimCriarItem"
          @delete-item="deleteItem"
          @inicio-edit-item="inicioEditItem"
          @fim-edit-item="fimEditItem"
          @mudar-risco="mudarRiscoModal"
          @teste="teste"
          @inicio-edit-title="inicioEditTitle"
          @fim-edit-title="fimEditTitle"
          class="modal scrollBar"/>

    
  </div>
</template>
<script>
import MessageFeedback from '../components/MessageFeedback.vue';
import TodoListModal from '../components/TodoListModal.vue';
import TodoListPadrao from '../components/TodoListPadrao.vue';

export default {
  components:{
      TodoListPadrao,
      TodoListModal,
      MessageFeedback
  },
  data(){
      return{
        msg: '',
        titleMsg:'',
        tipoStatus:'',
        riscadoOuNao: 'riscado',
        data: null
      }
  },
  methods: {
      reversedItems() {
        return this.data ? this.data.slice().reverse(): [];
      },
      criarItem(idList){
        this.data.forEach(el=>{
          el.novoItemTextShow = el.id == idList;
        });
      },
      fimCriarItem(idList){
        this.data.forEach(el=>{
          if(el.id == idList && el.novoItemTextShow){
            el.novoItemTextShow = false;
            const novoItem = {
              id: `${el.allItems.length+1}`,
              text: el.novoItemNome,
              state: false
            };
            el.allItems.push(novoItem);
          }
        });
      },
      mostrarModal(idList){
        this.data.forEach(el=>{
          if(el.id == idList){
            el.showModal = true;
          }
        });
        const fade = document.querySelector('#fade');
        fade.style.display="flex";
      },
      fecharModal(idList, code){
        this.data.forEach(el=>{
          if(el.id == idList){
            el.showModal = false;
          }
        });
        const fade = document.querySelector('#fade');
        fade.style.display="none";
        if(code == 0){
          this.titleMsg = 'Lista deletada';
          this.tipoStatus = 'delete';
          setTimeout(()=>this.titleMsg='', 2000);
        }else{
          this.titleMsg='Lista fechada';
          this.msg='Nenhuma edição foi salva';
          this.tipoStatus='update';
          setTimeout(()=>this.titleMsg='', 2000);
        }

      },
      async salvarList(idList){
        this.fecharModal(idList);
        this.data.forEach(el=>{
            delete el.edit;
            delete el.showModal;
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

        this.titleMsg='Mudanças aplicadas com sucesso';
        this.tipoStatus='update';
        setTimeout(()=>this.titleMsg='', 2000);
      },
      async deleteList(idList){
        this.fecharModal(idList, 0);
        const req = await fetch(`https://makeyourburger-xxqo.onrender.com/allTodoList/${idList}`,{
            method: "DELETE"
        })
        const res = await req.json();
        console.log(res);
        this.getAllLists();

        
      },
      async criarList(){
        const novaLista = {
          title:'Nova lista',
          allItems:[{
            text:'Novo item',
            state:false
          }]
        }
        const dataJson = JSON.stringify(novaLista);
        const req = await fetch("https://makeyourburger-xxqo.onrender.com/allTodoList",{
              method: "POST",
              headers: { "Content-Type" : "application/Json" },
              body: dataJson
        });
        const res = req.json();
        console.log(res);
        await this.getAllLists();
        this.mostrarModal(this.data[this.data.length-1].id);
        
        this.titleMsg='Nova lista criada!';
        this.tipoStatus='create';
        setTimeout(()=>this.titleMsg='', 2000);
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
      inicioEditItem(dados){
        let idList = dados.p1;
        let item = dados.p2;
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
      fimEditItem(dados){
        let idList = dados.p1;
        let item = dados.p2;
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
      async deleteItem(dados){
        let idList = dados.p1;
        let item = dados.p2;
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
      async mudarRiscoModal(dados){
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
      },
      teste(a){alert(a?'editar':'deletar')},

      async getAllLists(){
      let req = await fetch("https://makeyourburger-xxqo.onrender.com/allTodoList");
      let data = await req.json();

      data.forEach(el=>{
          el.edit = false;
          el.showModal = false;
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
  gap: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
  z-index:1;
}
#rowReverse{
  display: flex;
  flex-direction: row-reverse;
  justify-content: space-around;
  width: 100%;
  flex-wrap: wrap-reverse;

}
#fade{
  display:none;
  background:  #0000007c;
  background-size: cover;
  z-index:2;
  position:fixed;
  left:0;
  top:0;
  width:100%;
  height:100%;
}
.modal{
  display:flex;
  margin:150px auto 0 auto;
  flex-direction:column;
  z-index:3;
  position:fixed;
  padding:1rem;
  top: 0;
}
p{
  margin-left: 10px;
}
.criarList{
  cursor: pointer;
}
.cardsCriacao{
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
  background: transparent;
  border: 2px solid #D9D9D9;
  color: #D9D9D9;
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

/*Scroll bar*/
::-webkit-scrollbar {
  width: 5px;  }

::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey; 
  border-radius: 10px;
  background: #D9D9D9;
}
 
::-webkit-scrollbar-thumb {
  background: #222; 
  border-radius: 10px;
}
/* Pequenas telas */
@media (max-width: 900px){
  #arrayToDo{
    margin-top: 40px;
  }
  .modal{
    top: 0;
    margin-top: 150px;
  }
}
</style>
