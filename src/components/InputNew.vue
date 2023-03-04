<template>
    <form action="" @submit.prevent="handleSubmit">   <!-- prevent: evita el comporatmiendo prederterminado de envío de formularios, por lo que no enviará la información directamente al servidor. Esto evitará que la página se vuelva a cargar cuando también se presione el botón Enviar. 
                                                        @submit es lo mismo que v-on:click="submit"-->
          <input type="text" v-model="text">  <!-- Esta directiva permite enlazar (bindear) una variable de Vue con un atributo específico de una etiqueta HTML. De esta forma, podemos colocar como valor de un atributo HTML el contenido que tengamos almacenado en una variable de la lógica de Javascript. -->
                                             <!-- enlazar completamente una etiqueta HTML con un objeto que tenemos en las variables de Vue. De esta forma actualizamos la etiqueta completa simplemente actualizando el  desde Javascript.  
                                                 text es el parametro que vamos a pasar a la función function handleNewItem(text) (Que esta en TodoApp) 
                                                  text será el valor que el usuario meta en el input y que se recogerá en la function handleSubmit()-->
      </form>
  
  </template>
  
  <script setup>
  import {ref, defineEmits} from "vue";
  
  const text = ref(""); /* Variable reactiva que cambia de estado/valor cuando se interactura con ella, como es un string utilizamos ref */
  const emits = defineEmits(["onNewItem"]);/*  emits: (["nuevo array de valores"]) <--- setup(props, {emit})*/
  
  function handleSubmit(){
      if(text.value  !== " "){
          // mandar a llamar a la función emit Events (@submit)
          emits("onNewItem" , text) /* le pasamos como parámetros el nombre del emit y el parametro text que es el que estamos comprobando */
          text.value = " ";
      }
  }
  
  </script>
  
  <style>
  
  </style>