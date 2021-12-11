<template>
  <q-page class="flex-fluid flex-center">
    <div class="q-gutter-y-md" >
      <q-card>
          <!-- botones que dan función al tab -->
        <q-tabs
          v-model="tab"
          dense
          class="text-grey"
          active-color="primary"
          indicator-color="primary"
          align="justify"
          narrow-indicator
        >
          <q-tab name="listar" color="primary" label="Listar Producto" />
          <q-tab name="crear" label="Crear Producto" />
          <q-tab name="cargar" label="Cargar Producto" />

        </q-tabs>

        <q-separator />
        <!-- contenido que se muestra en el tab, se define que se muestra basandose en los name de los q-tab-panel pasandose a los v-model de los
        q-tab-panels y q-tabs  -->
        <q-tab-panels v-model="tab" animated >
          <q-tab-panel name="listar"  style="padding: 0px !important">
            <div class="text-h6"></div>
           <listarProductos />  <!-- componente llamado desde la carpeta components -->
          </q-tab-panel>
          <q-tab-panel name="crear" style="padding: 0px !important">
            <crearProductos />
          </q-tab-panel>
          <q-tab-panel name="cargar" style="padding: 0px !important">
            <cargarProductos />
          </q-tab-panel>
        </q-tab-panels>
      </q-card>


    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { ref } from 'vue'
import listarProductos from 'components/productos/listarProductos.vue' // forma de traer los componentes y librerias externas
  import crearProductos from 'components/productos/crearProductos.vue'
  import cargarProductos from 'components/productos/cargarProductos.vue'

export default defineComponent({
  name: 'Productos',
  components: {
    listarProductos,  // se define si las importaciones son componentes para asi poder usarlos en el template
    crearProductos,
    cargarProductos
  },
  data(){// zona para crear propiedades reactivas
    return{
       tab: ref('listar')//se inicializa en este caso en el name del q-tab-panel que se quiere mostrar al cargar la pagina
    }
  },
  mounted() {
   if (this.$store.state.usuario === false) {
       this.$router.push({ path: `/` });
           }
 }
})
</script>
