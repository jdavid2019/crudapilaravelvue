<!--aqui creo el encabezado de mi tabla-->
<template>
  <div class="customer-list">
    <div class="data">
        <table class="ui celled table">
            <thead>
                <tr>
                    <th style="width:50px; text-align:center;">#</th>
                    <th>Nombre</th>
                    <th>Email</th>
                    <th style="width:188px;">Acciones</th>
                </tr>
            </thead>
            <tbody>
              <Usuario v-for="usuario in usuarios" :key="usuario.id" 
              :usuario="usuario"
              @aEliminar="aEliminar"
              @aEditar="aEditar"
              />
            </tbody>
        </table>
    </div>
  </div> 
</template>

<script>
import Usuario from "./Usuario";
export default {
name : "UsuarioLista"
,
components : {
    Usuario
},
props :{
    //esto ya esta pero aqui como es un archivo nuevo lo paso como array
    usuarios :{
        type : Array
    }
},
methods:{
    aEliminar(id){
    //  window.console.log("lista eliminada " + id);
    this.$emit("aEliminar",id);
    },
    aEditar(data){
   //  window.console.log("lista editada " + data);
    this.$emit("aEditar",data);
    }
}
}
</script>

<style>

</style>