<template>
  <q-page class="flex-fluid flex-center">
    <!-- tabla dinamica  -->
    <q-table
       title="Listado de Clientes"
       :rows="clientes"
       :columns="columns"
       row-key="name"
        :filter="filter"
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
     <!-- dialog de confirmación para eliminación -->
     <q-dialog v-model="confirm" persistent>
     <q-card>
       <q-card-section class="row items-center">
         <q-avatar icon="delete" color="primary" text-color="white" />
         <span class="q-ml-sm">¿Desea eliminar este cliente? </span>
       </q-card-section>

       <q-card-actions align="right">
         <q-btn flat label="Cancel" color="primary" v-close-popup />
         <q-btn flat label="Eliminar cliente" @click="deleteRow()" color="primary" v-close-popup />
       </q-card-actions>
     </q-card>
   </q-dialog>
    <!-- fin dialog de confirmación para eliminación -->
    <!-- dialog de edición -->
   <q-dialog v-model="confirmEdit" persistent>
   <q-card>
     <q-card-section class="row items-center">
       <q-avatar icon="edit" color="primary" text-color="white" />
       <span class="q-ml-sm text-h6">Editar Cliente </span>
     </q-card-section>
     <q-card-section class="row items-center">
       <q-form
        @submit="onSubmit"
        @reset="onReset"
        class="q-gutter-md"
        ref="myForm"
      >
       <div class="row">
              <div class="col q-px-sm">
                  <q-input v-model="cedulaCliente" label="Cedula" />
              </div>
              <div class="col q-px-sm">
                  <q-input  v-model="direccionCliente" label="Dirección" />
              </div>
    </div>
    <div class="row">
              <div class="col q-px-sm">
                  <q-input v-model="emailCliente" label="Correo" />
              </div>
              <div class="col q-px-sm">
                  <q-input v-model="nombreCliente" label="Nombre" />
              </div>
              <div class="col q-px-sm">
                  <q-input v-model="telefonoCliente" label="Telefono" />
              </div>
    </div>
    <q-separator />
    </q-form>
     </q-card-section>
     <q-card-actions align="right">
       <q-btn flat label="Cancel" color="primary" v-close-popup />
       <q-btn flat label="Guardar Cambios" @click="deleteRow()" color="primary" v-close-popup />
     </q-card-actions>
   </q-card>
 </q-dialog>
 <!-- fin dialog de edición -->
     <q-separator/>
  </q-page>
</template>

<script>
import { api } from 'boot/axios'
import { defineComponent } from 'vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'listarClientes',
  data() {
   return {
     clientes: [], // aqui se colocan los clientes que usara la tabla dinamica
     eliminar: [],// se coloca el cliente a eliminar
     confirm: ref(false),
       confirmEdit: ref(false),
       filter: '',// se usa para tener lo que el usuario busca en el seach de la tabla
     columns: [
       {name: "cedula_cliente", label: "Cedula", field: "cedula_cliente", sortable: true},
        {name: "direccion_cliente", label: "Dirección", field: "direccion_cliente"},
        {name: "email_cliente", label: "Correo", field: "email_cliente"},
        {name: "nombre_cliente", label: "Nombre", field: "nombre_cliente"},
        {name: "telefono_cliente", label: "Telefono", field: "telefono_cliente"},
     ]
   }
},
methods:{
  loadUsers(){ // trae los clientes de la api
    api.get("/clientes")
    .then(response => (this.clientes = response.data.clientes
          ))
          .catch(e => {
            console.log(e);
          });
  },
  displayDeleteDialog(props){ // muestra o culpa el dialog tambien pasa información del objeto
    this.confirm = true
    this.eliminar = props
  },
  displayEditDialog(props){// muestra o culpa el dialog tambien pasa información del objeto
    this.confirmEdit = true
    this.eliminar = props
  },
  deleteRow() { // elimina cliente
    const index = this.clientes.indexOf(this.eliminar.row);
      api.delete("/eliminarClientes?cedula=" + this.eliminar.row.cedulaCliente)
          .then(response => response.data)
          .catch(e => {
            console.log(e);
          });
      //this.loadUsers();
      this.clientes.splice(index, 1);// borra cliente de la tabla ya que se elimino tambien del back pero la tabla no lo sabe
    },
},
created(){// ejecuta codigo en una fase temprana de inicialización de la pagina
  this.loadUsers(); // llama el metodo que carga la tabla de información
}
})
</script>
