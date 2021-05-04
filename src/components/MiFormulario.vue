<template>
  <div class="my-form">
    <form class="ui form">
      <div class="fields">
        <div class="four wide field">
          <label>Nombre</label>
          <input
            type="text"
            name="fname"
            placeholder="Primer nombre"
            @change="handleChange"
            :value="form.fname"
          />
        </div>
        <div class="four wide field">
          <label>Apellido</label>
          <input
            type="text"
            name="lname"
            placeholder="Apellido"
            @change="handleChange"
            :value="form.lname"
          />
        </div>
        <div class="six wide field">
          <label>Email</label>
          <input
            type="email"
            name="email"
            placeholder="gmail"
            @change="handleChange"
            :value="form.email"
          />
        </div>
        <div class="two wide field">
          <button
            :class="btnClass"
            @click="onFormSubmit"
            v-text="btnName"
          ></button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  //paso 0 creo el nombre
  name: "MiFormulario",
  data() {
    return {
      btnName: "Guardar",
      btnClass: "ui primary button submit-button",
    };
  },
  props: {
    form: {
      type: Object,
    },
  },
  methods: {
    //manejar el cambio
    handleChange(event) {
      const { name, value } = event.target;
      let form = this.form;
      form[name] = value;
      this.form = form;
    },
    onFormSubmit(e) {
      // prevenir el enviado
      e.preventDefault();

      //validar formulario
      if (this.validarFormulario()) {
        // window.console.log("listo para enviar");
        this.$emit("onFormSubmit", this.form);

        //cambiar el nomnbre del boton
        this.btnName = "Guardar";
        this.btnClass = "ui primary button submit-button";
        // agrego metodo de limpiar
        this.limpiarCampos();
      }
    },

    validarFormulario() {
      //nombre
      if (document.getElementsByName("fname")[0].value === "") {
        //alert("colocar un nombre por favor");
        //   this.$swal('Debes colocar un nombre por favor');
        this.$swal({
          // position: 'bottom-end',
          icon: "error",
          title: "Debes colocar un nombre por favor",
          showConfirmButton: false,
          timer: 1500,
        });

        return false;
      }
      //apellido
      if (document.getElementsByName("lname")[0].value === "") {
        this.$swal({
          // position: 'bottom-end',
          icon: "warning",
          title: "Debes colocar un apellido por favor",
          showConfirmButton: false,
          timer: 1500,
        });
        return false;
      }
      //email
      if (document.getElementsByName("email")[0].value === "") {
        this.$swal({
          // position: 'bottom-end',
          icon: "warning",
          title: "Debes colocar un email por favor",
          showConfirmButton: false,
          timer: 1500,
        });
        return false;
      }

      return true;
    },
    limpiarCampos() {
      //limpiar la data
      this.form.fname = "";
      this.form.lname = "";
      this.form.email = "";
      this.form.isEdit = false;

      //limpiar los campos
      document.querySelector(".form").reset();
    },
  },
  updated() {
    if (this.form.isEdit) {
      this.btnName = "Actualizar";
      this.btnClass = "ui orange button submit-button";
    }
  },
};
</script>

<style>
</style>