<template>
  <q-page class="flex-fluid flex-center">
    <!-- tabla dinamica  -->
    <q-table
       title="Ventas por Cliente"
       :rows="ventasclientes"
       :columns="columns"
       row-key="name"
        :filter="filter"
        :table-header-style="{ fontSize: 'small' }"
     >
     <template v-slot:body-cell-actions="props">
         <q-td :props="props">
           <q-btn dense round flat color="red" @click="displayDeleteDialog(props)" icon="delete"></q-btn>
           <q-btn dense round flat color="primary" @click="displayEditDialog(props)" icon="edit"></q-btn>
         </q-td>
       </template>
       <template v-slot:top-right>
                <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
                  <template v-slot:append>
                    <q-icon name="search" />
                  </template>
                </q-input>
              </template>

     </q-table>
        <q-separator/>
     <q-card class="text-center text-h6 q-mt-md q-pa-md">
       <q-card-title>
         Total consolidado
       </q-card-title>
       <q-card-section>
         <q-icon name="monetization_on"/>
         {{totalVentas}}
       </q-card-section>

   </q-card>
  </q-page>
</template>

<script>
import { api } from 'boot/axios'
import { defineComponent } from 'vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'ventasCliente',
  data() {
   return {
     ventasclientes: [{codigoVenta: "1111", cedulaCliente: "222222", cedulaUsuario: "655857", ivaVenta: "3800", totalVenta: "20000",
                       valorVenta: "23800"},
                      {codigoVenta: "1112", cedulaCliente: "1111111", cedulaUsuario: "655857", ivaVenta: "950", totalVenta: "5000",
                       valorVenta: "5950"}], // aqui se colocan los clientes que usara la tabla dinamica
     eliminar: [],// se coloca el cliente a eliminar
     confirm: ref(false),
       confirmEdit: ref(false),
       filter: '',// se usa para tener lo que el usuario busca en el seach de la tabla
       totalVentas: ref(29750),
     columns: [
       {name: "codigoVenta", label: "Codigo Venta", field: "codigoVenta", sortable: true},
        {name: "cedulaCliente", label: "Cedula Cliente", field: "cedulaCliente"},
        {name: "cedulaUsuario", label: "Cedula Usuario", field: "cedulaUsuario"},
        {name: "ivaVenta", label: "IVA Venta", field: "ivaVenta"},
        {name: "totalVenta", label: "Total sin IVA", field: "totalVenta"},
        {name: "valorVenta", label: "Total Venta", field: "valorVenta"},
      //  {name: 'actions', label: 'Acciones', field: '', align: 'center'},
     ]
   }
},
methods:{
  loadUsers(){ // trae los clientes de la api
    api.get("/listarVentasUsuario")
    .then(response => (this.ventasclientes = response.data
          ))
          .catch(e => {
            console.log(e);
          });
          api.get("/sumarVentas")
          .then(response => (this.totalVentas = response.data
                ))
                .catch(e => {
                  console.log(e);
                });
  }
},
created(){// ejecuta codigo en una fase temprana de inicialización de la pagina
  this.loadUsers(); // llama el metodo que carga la tabla de información
}
})
</script>
