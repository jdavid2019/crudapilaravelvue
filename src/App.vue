<template>
 <div id="app">
    <div class="ui fixed inverted menu vue-color">
      <div class="ui container">
        <a href="#" class="header item">Vue JS CRUD Laravel API</a>
      </div>
    </div>
    <div class="ui main container">
      <!--lo llamo aqui-->
      <MiFormulario :form="form" @onFormSubmit="onFormSubmit"  />
      <LoaderUsuario v-if="loader" />
      <UsuarioLista :usuarios="usuarios" @aEliminar="aEliminar" @aEditar="aEditar" />
    </div>
  </div>
</template>

<script>
//1ero importo mi ruta
import Vue from 'vue';
import VueSweetalert2 from 'vue-sweetalert2';
Vue.config.productionTip = false
Vue.use(VueSweetalert2);
import axios from "axios";
import MiFormulario from "../src/components/MiFormulario";
import UsuarioLista from "../src/components/UsuarioLista";
import LoaderUsuario from "../src/components/LoaderUsuario";

export default {
  name: 'App',
  components: {
    //2do asigno mi componente
   MiFormulario,
   UsuarioLista,
   LoaderUsuario
  },
  data(){
    return{
      url : "http://127.0.0.1:8000/api/students",
      url2: "http://127.0.0.1:8000/api/students/delete",
      url3: "http://127.0.0.1:8000/api/student",
      url4: "http://127.0.0.1:8000/api/students/update",
      usuarios : [],
      form : {
        fname: '',
        lname: '',
        email: '',
        isEdit: false
      },
      loader : false
    }
  },
  methods : {
       getUsuarios(){
         this.loader = true;
         axios.get(this.url).then(data => { 
           this.usuarios = data.data; 
           this.loader = false;
           } );
       },
       agregarUsuario(data){
         this.loader = true;
         axios.post(this.url3,{
           fname : data.fname,
           lname : data.lname,
           email : data.email
         }).then(() =>{
           this.$swal({
          icon: 'success',
          title: 'Usuario Creado',
          showConfirmButton: false,
          timer: 2800
          })
            this.getUsuarios();
         }).catch(
          // e => alert(e),
          e => 
          this.$swal('Error email duplicado , verifique',e),
           this.loader = false
           );
       },
       editarUsuario(data){
         this.loader = true;
         axios.put(`${this.url4}/${data.id}`,{
           fname : data.fname,
           lname : data.lname,
           email : data.email
         }).then(
           () => {
             this.getUsuarios();
           }
         ).catch(e => {
           alert(e);
         })
       },
       eliminarUsuario(id){
         this.loader = true;
         axios.delete(`${this.url2}/${id}`).then(
           () => {
             this.getUsuarios();
           }
         ).catch(e => {
           alert(e);
         });
       } , 
       aEliminar(id){
       //  window.console.log("eliminado " +id);
       this.eliminarUsuario(id);
       },
       aEditar(data){
      //  window.console.log("editado " +data);
      // this.eliminarUsuario(id);
      this.form = data;
      this.form.isEdit = true;
       },
        onFormSubmit(data){
      //  window.console.log("app onsubmit" , data);
      if(data.isEdit){
        //llama al metodo editar usuario
        this.editarUsuario(data);
       
      }else{
        //llama al metodo crear usuario
        this.agregarUsuario(data);
      }
 }
 },
 created(){
   this.getUsuarios();
 }
}
</script>

<style>
.vue-color {
  background-color: black !important;
}
.main.container{
  margin-top: 60px;
}

.submit-button{
  margin-top: 24px !important;
  float:right;
}

.data{
  margin-top: 15px;
}
thead tr th{
  background: #e0e0e0 !important;
}
.ui.inverted.dimmer{
  background-color: black !important ;
}
</style>
