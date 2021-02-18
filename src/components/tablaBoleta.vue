<template>
<div class="card">
  <div class="container-fluid">
   <b-alert
      :show="dismissCountDown"
      dismissible
     :variant="alertvariant"
      @dismissed="dismissCountDown=0"
      @dismiss-count-down="countDownChanged"
    > {{ mensaje }}
    </b-alert>
          <table class="table table-striped table-bordered">
              <thead class="thead-dark">
              <tr>
                  <th> Nombre evento </th>
                  <th> Descripción </th>
                  <th> Limite</th>
                  <th> Asignados </th>
                  <th> Fecha de cierre </th>
                  <th> Acciones </th>
              </tr>
              </thead>
              <tbody>
                  <tr v-for="boleta in boletas" :key="boleta.id">
                      <td>{{boleta.nombre_evento}}</td>

                  <td> {{boleta.descrip}} </td>
                  <td> {{boleta.limite}} </td>
                  <td> {{boleta.asignados}} </td>
                  <td> {{boleta.fecha_cierre}} </td>
                  
                  <td> <button class="btn btn-info text-white m-1"  data-toggle="modal" data-target="#agregar"
                   @click="modal_boleta(boleta.id)">Asignar boleta</button> 
                      <button class="btn btn-danger text-white m-1"
                   @click="delete_evento(boleta.id)">Eliminar Evento</button>
                   </td>
                  </tr>
              </tbody>
          </table>
         <b-modal ref="agregar" hide-footer >
          <template #modal-title>
            Asignar boleta
          </template>
          <form action="" v-on:submit.prevent="agregar(id_boleta)">
            
            <div class="form-group">
                <label for="recipient-name" class="col-form-label">Cédula de cliente:</label>
                <input type="number" class="form-control" v-model="cedula" required>
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
  // props:{actualiza:Function},
  data(){
    return {
       alertvariant:"warning",
      dismissSecs: 5,
      mensaje:'',
      dismissCountDown: 0,
      boletas:[ ],
      id_boleta:'',
      cedula:''};
  },
 methods:{
   modal_boleta(id){
      this.$refs['agregar'].show();
     this.id_boleta = id;
   },
     
   delete_evento(id_b){
            axios.delete('http://localhost/pruebalaravel/public/api/boleta/eliminar/'+id_b).then((response) => {
              //  this.$refs['agregar'].hide()
              // eliminar
              if (response.data == 'no encontrado') {
                 this.alertvariant='danger';
                  this.showAlert('boleta no encontrada');
              } else {
                console.log(response.data);
                this.alertvariant='success';
                this.showAlert('Evento Eliminado');
              }
              this.mostrar();
        });

          },
   agregar(id_b){
     const datos = {cedula:this.cedula};
            axios.put('http://localhost/pruebalaravel/public/api/boleta/asignar/'+id_b,datos).then((response) => {
              //  this.$refs['agregar'].hide()
                this.$refs['agregar'].hide();
              if (response.data === 'comprador') {
                this.alertvariant='warning';
                  this.showAlert('Cliente no registrado');
                } else if (response.data === 'boleta') {
                  this.alertvariant='danger';
                  this.showAlert('boleta no encontrada');
              } else {
                this.alertvariant='success';
                this.showAlert('boleta asignada');
                this.cedula = '';
                 this.mostrar();
              }
        });

          },
    async mostrar()
      {
          axios.get('http://localhost/pruebalaravel/public/api/boleta').then((response) => {
            this.boletas = response.data;
        });
      }, countDownChanged(dismissCountDown) {
        this.dismissCountDown = dismissCountDown;
      },
      showAlert(mensaje) {
        this.mensaje = mensaje;
        this.dismissCountDown = this.dismissSecs;
      }

    },
    created (){
      this.mostrar();
    }
    
  }
</script>

