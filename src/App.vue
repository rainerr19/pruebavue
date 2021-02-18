<template>
  <div id="app" class="container-fluid">
      <div class="container-fluid mt-2">
        <div class="row justify-content-center">
            <div class="col-md-8">
              <registros @actualizar="update"/>
              <tablaboleta ref="tabla" />

            </div>
        </div>
      </div>
  </div>
</template>

<script>
import tablaboleta from './components/tablaBoleta.vue'
import registros from './components/registros.vue'

export default {
  name: 'App',
  components: {
    tablaboleta,registros
  }, methods:{
    // evento emitido por evento hijo y hermano de tabla
    update()
      {
        // usar method del hijo usando ref
       this.$refs.tabla.mostrar();
      }
    }
}
</script>

