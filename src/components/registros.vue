<template>
<div class="card my-2">
  <div class="container-fluid m-1">
   <b-alert
      :show="dismissCountDown"
      dismissible
     :variant="alertvariant"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
    > {{ mensaje }}
    </b-alert>
      <button class="btn btn-success text-white" @click="modal_comprador()">+ Agregar comprador</button> 
      <button class="btn btn-success text-white float-right" @click="modal_evento()">+ Agregar evento</button> 
        
      <b-modal ref="comprador" hide-footer >
      <template #modal-title>
        Agregar comprado
      </template>
      <form action="" v-on:submit.prevent="agregar_comprador()">
        
        <div class="form-group">
            <label class="col-form-label">Nombres cliente:</label>
            <input type="text" class="form-control" v-model="nombres" required>
        </div>
        <div class="form-group">
            <label class="col-form-label">Apellidos cliente:</label>
            <input type="text" class="form-control" v-model="apellidos" required>
        </div>
        <div class="form-group">
            <label class="col-form-label">Cédula de cliente:</label>
            <input type="number" class="form-control" v-model="cedula" required>
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
      </form>                  
    </b-modal >
    <b-modal ref="evento" hide-footer >
    <template #modal-title>
      Agregar Evento
    </template>
    <form action="" v-on:submit.prevent="agregar_evento()">
      
      <div class="form-group">
          <label class="col-form-label">Nombre de evento :</label>
          <input type="text" class="form-control" v-model="nombre_evento" required>
      </div>
      <div class="form-group">
          <label class="col-form-label">Descripción :</label>
          <input type="text" class="form-control" v-model="descrip">
      </div>
      <div class="form-group">
          <label class="col-form-label">Limite :</label>
          <input type="number" class="form-control" v-model="limite" required>
      </div>
      <div class="form-group">
          <label class="col-form-label">Fecha cierre :</label>
          <b-form-datepicker v-model="fecha_cierre" :min="min" locale="es"></b-form-datepicker>
      </div>
      <button type="submit" class="btn btn-primary">Guardar</button>
    </form>                  
  </b-modal >

  </div>

</div>
</template>

<script>
import axios from "axios";


export default {
  data(){
    const now = new Date()
      const today = new Date(now.getFullYear(), now.getMonth(), now.getDate())
      const minDate = new Date(today)
    return {
      alertvariant:"warning",
      dismissSecs: 5,
      mensaje:'',
      dismissCountDown: 0,
      cedula:'',
      apellidos:'',
      nombres:'',
      nombre_evento:'',
      descrip:'',
      limite:'',
      fecha_cierre:'',
      min:minDate
      };
  },
 methods:{
   modal_comprador(){
      this.$refs['comprador'].show();
   },
   modal_evento(){
      this.$refs['evento'].show();
   },
   clear_input(){
     this.cedula='';this.apellidos='';this.nombres='';
     this.nombre_evento='';this.descrip='';this.limite='';this.fecha_cierre='';
   },
   agregar_comprador(){
     const datos = {cedula:this.cedula,apellidos:this.apellidos,nombres:this.nombres};
            axios.post('http://localhost/pruebalaravel/public/api/boleta/create_comprador',datos).then((response) => {
              //  this.$refs['agregar'].hide()
                this.$refs['comprador'].hide();
                console.log(response.data);
      
                this.alertvariant='success';
                this.clear_input();
                this.showAlert('Comprador creado');
              
        });

          },
   agregar_evento(){
     const datos = {nombre_evento:this.nombre_evento,descrip:this.descrip,limite:this.limite,fecha_cierre:this.fecha_cierre};
            axios.post('http://localhost/pruebalaravel/public/api/boleta/create',datos).then((response) => {
              //  this.$refs['agregar'].hide()
                this.$refs['evento'].hide();
                console.log(response.data);
      
                this.alertvariant='success';
                this.clear_input();
                this.showAlert('Evento creado');
                //  this.$forceUpdate();
              this.$emit('actualizar','actualizar');
        });

          },
      countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown;
      },
      showAlert(mensaje) {
        this.mensaje = mensaje;
        this.dismissCountDown = this.dismissSecs;
      }

    }
    
  }
</script>

