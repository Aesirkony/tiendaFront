
<template>
  <q-page class="flex-fluid flex-center">
    <div class="q-gutter-y-md">
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
          <q-tab name="listadoClientes" color="primary" label="Listado Clientes" />
          <q-tab name="ventaCliente" color="primary" label="Ventas por Cliente" />

        </q-tabs>

        <q-separator />
        <q-tab-panels v-model="tab" animated>
          <q-tab-panel   name="listadoClientes">
            <div class="text-h6"></div>
              <listarClientes class="" />
          </q-tab-panel>
          <q-tab-panel   name="ventaCliente">
            <div class="text-h6"></div>
              <ventasClientes class="" />
          </q-tab-panel>
        </q-tab-panels>
      </q-card>


    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { ref } from 'vue'
import listarClientes from 'components/reportes/listarClientes.vue'
import ventasClientes from 'components/reportes/ventasClientes.vue'
export default defineComponent({
  name: 'Productos',
  components: {
     listarClientes,
     ventasClientes
  },
  data(){
    return{
       tab: ref('listadoClientes')//se inicializa en este caso en el name del q-tab-panel que se quiere mostrar al cargar la pagina
    }
  },
  mounted() {
   if (this.$store.state.usuario === false) {
       this.$router.push({ path: `/` });
           }
 }
})
</script>
