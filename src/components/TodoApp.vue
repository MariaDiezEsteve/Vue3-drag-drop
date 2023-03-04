<template>
    <nav>
      <ul>
        <li><a href="#" @click.prevent="handleNewBoard">Create board</a></li> <!-- Para crear un tablero -->
      </ul>
    </nav>
  
    <div class="boards-container">
      <div class="boards">
        <div class="board" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent v-for ="board in boards" :key="board.id"> 
            <div>{{ board.name }}</div>
            <InputNew @on-new-item="(text) => handleNewItem(text, board)"/> <!-- Aquí aplicamos el emit creado en InputNew con:  @on-new-item -->
          <div class="items">
            <div class="item" draggable="true" @dragstart="startDrag($event, board, item)"  v-for="item in board.items" :key="item.id"> <!-- La propiedad draggable existe en todos los elementos HTML y sirve para poder moverlos de un sitio a otro -->
                <div>{{ item.title}}</div>
            </div>
          </div>  
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { reactive } from "vue";
  import InputNew from './InputNew.vue'
  /*Variables reactivas:
  Las variables reactivas se llaman así porque son capaces de refrescar la vista de los componente cuando éstas cambian.
  Imagina que en la vista (en el HTML) pintas una variable que contiene un string. Si en cualquier punto del componente actualizas esa variable, 
  la vista se actualizará automáticamente sin tener que hacerlo a mano. */ 
  let boards = reactive( [
    {
      id:  crypto.randomUUID(),
      name: "Tablero 1",
      items: [
        {
          id:  crypto.randomUUID(), /* El randomUUID()método de la Cryptointerfaz se usa para generar un UUID v4 usando un generador de números aleatorios criptográficamente seguro. EJ:  "36b8f84d-df4e-4d49-b662-bcde71a8764f"*/
          title:'Feature de archivos',
        },
        {
          id:  crypto.randomUUID(), /* El randomUUID()método de la Cryptointerfaz se usa para generar un UUID v4 usando un generador de números aleatorios criptográficamente seguro. EJ:  "36b8f84d-df4e-4d49-b662-bcde71a8764f"*/
          title:'Feature de archivos',
        },
      ],  
    },
    {
      id:  crypto.randomUUID(),
      name: "Tablero 2",
      items: [
        {
          id:  crypto.randomUUID(), /* El randomUUID()método de la Cryptointerfaz se usa para generar un UUID v4 usando un generador de números aleatorios criptográficamente seguro. EJ:  "36b8f84d-df4e-4d49-b662-bcde71a8764f"*/
          title:'Mandar reporte',
        },
        {
          id:  crypto.randomUUID(), /* El randomUUID()método de la Cryptointerfaz se usa para generar un UUID v4 usando un generador de números aleatorios criptográficamente seguro. EJ:  "36b8f84d-df4e-4d49-b662-bcde71a8764f"*/
          title:'Code review',
        },
      ],  
    },
  ]);
  
  /* CREAR UN NUEVO INPUT DENTRO DE UNA TARJETA */
  
  function handleNewItem(text, board){ /* Pasamos el texto y la referencia de su tablero */ 
    console.log(text.value, board.id, board.name)
    board.items.push({
      id: crypto.randomUUID(),
      title: text.value,
    });
  }
  
  /* CREAR UN NUEVO TABLERO */
  function handleNewBoard(){
    const name = prompt("Name of the board");
    if(name !== " "){
      boards.push({
        id:  crypto.randomUUID(),
        name: name,
        items: []
      })
    }
  }
  
  /* Para mover las etiquetas de un tablero a otro */
  function startDrag(evt, board, item){
      evt.dataTransfer.setData("text/plain", JSON.stringify({ boardId: board.id, itemId: item.id}))
  }
  
  function onDrop(evt, dest){
    const { boardId, itemId  } = JSON.parse(evt.dataTransfer.getData("text/plain"))
    const originBoard = boards.find(item => item.id === boardId) /* Tengo el tablero de origen */
    const originItem = originBoard.items.find(item => item.id === itemId) /* Tengo la tarjeta del origen*/
  
    dest.items.push({ ... originItem })
    originBoard.items = originBoard.items.filter(item => item !== originItem)
  }
  
  </script>
  
  <style scoped>
  
  .boards {
    display: flex;
    gap: 10px;
  }
  
  .board{
    background: #efefef;
    padding: 10px;
  }
  
  .items {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }
  
  .item{
    background: white ;
    padding: 10px;
    box-sizing: boder-box;
  }
  
  </style>
  