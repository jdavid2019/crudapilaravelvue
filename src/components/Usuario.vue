<template>
   <tr>
      <td>{{usuario.id}}</td>
      <td>{{usuario.fname}} {{usuario.lname}}</td>
      <td>{{usuario.email}}</td>
      <td>
        <button class="mini ui green button" @click="aEditar">Editar</button>
        <button class="mini ui red button" @click="aEliminar">Eliminar</button>
      </td>
    </tr>
</template>

<script>
export default {
name: "Usuario",
props: {
    usuario:{
      type : Object
    }
},
methods:{
  aEliminar(){
   // window.console.log("usuario eliminado " +  this.usuario.id);
   this.$emit("aEliminar", this.usuario.id);
  },
   aEditar(){
  // window.console.log("usuario a editar número " +  this.usuario.id);
   this.$emit("aEditar", this.usuario);
  }
}
};
</script>

<style>

</style>