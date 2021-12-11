<template>
  <q-page class="flex-fluid flex-center">
    <q-table

      style=" width:100% !important;"
           indicator-color=""
           title="Productos"
           :rows="productos"
           :columns="columns"
           row-key="codigo_producto"
           :filter="filter"
         >
        <template v-slot:top-right >    <!--  aqui en este template se define todas las propiedades de el buscador de la tabla -->
                <q-input borderless dense debounce="300" v-model="filter" placeholder="Search">
                  <template v-slot:append>
                    <q-icon name="search" />
                  </template>
                </q-input>

        </template>
        <template v-slot:body-cell-actions="props" >
          <q-td :props="props" >
            <q-btn dense round flat color="red" @click="displayDeleteDialog(props)" icon="delete"></q-btn>
            <q-btn dense round flat color="primary" @click="displayEditDialog(props)" icon="edit"></q-btn>
          </q-td>
        </template>


    </q-table>                <!-- fin tabla dinamica  -->
 <!-- dialog de confirmación para eliminación -->
     <q-dialog v-model="confirm" persistent>
     <q-card>
       <q-card-section class="row items-center">
         <q-avatar icon="delete" color="primary" text-color="white" />
         <span class="q-ml-sm">¿Desea eliminar este producto? </span>
       </q-card-section>

       <q-card-actions align="right">
         <q-btn flat label="Cancel" color="primary" v-close-popup />
         <q-btn flat label="Eliminar producto" @click="deleteRow()" color="primary" v-close-popup />
       </q-card-actions>
     </q-card>
   </q-dialog>
    <!-- fin dialog de confirmación para eliminación -->
    <!-- dialog de edición -->
   <q-dialog v-model="confirmEdit" persistent>
   <q-card>
     <q-card-section class="row items-center">
       <q-avatar icon="edit" color="primary" text-color="white" />
       <span class="q-ml-sm text-h6">Editar Producto </span>
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
                  <q-input v-model="form.codigo_producto" disable label="Codigo Producto" />
              </div>
              <div class="col q-px-sm">
                  <q-input v-model="form.nitproveedor" label="Nit Proveedor" />
              </div>
              <div class="col q-px-sm">
                  <q-input  v-model="form.ivacompra" label="Iva Compra" />
              </div>
    </div>
    <div class="row">

              <div class="col q-px-sm">
                  <q-input v-model="form.nombre_producto" label="Nombre Producto" />
              </div>
              <div class="col q-px-sm">
                  <q-input v-model="form.precio_compra" label="Precio Compra" />
              </div>
              <div class="col q-px-sm">
                  <q-input v-model="form.precio_venta" label="Precio Venta" />
              </div>
    </div>
    <q-separator />
    </q-form>
     </q-card-section>
     <q-card-actions align="right">
       <q-btn flat label="Cancel" color="primary" v-close-popup />
       <q-btn flat label="Guardar Cambios" @click="onSubmit()" color="primary" v-close-popup />
     </q-card-actions>
   </q-card>
 </q-dialog>
 <!-- fin dialog de edición -->
     <q-separator/>
    </q-page>
</template>

<script>
import { api } from 'boot/axios' // importación de axios el cual se instancia y se usa desde boot/axios
import { defineComponent } from 'vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'listarClientes',
  data() {
   return {
     productos: [], // aqui se colocan los usuarios que usara la tabla dinamica
     filter: '',
     eliminar: [],// se coloca el clientes a eliminar
     confirm: ref(false),
     confirmEdit: ref(false),
     columns: [
       {name: "codigo_producto", label: "Codigo Producto", field: "codigo_producto", sortable: true},
       {name: "nitproveedor", label: "Nit Proveedor", field: "nitproveedor"},
       {name: "ivacompra", label: "Iva Compra", field: "ivacompra"},
       {name: "nombre_producto", label: "Nombre Producto", field: "nombre_producto"},
       {name: "precio_compra", label: "Precio Compra", field: "precio_compra"},
       {name: "precio_venta", label: "Precio Venta", field: "precio_venta"},
       {name: 'actions', label: 'Acciones', field: '', align: 'center'},
     ],
     form:{
       codigo_producto : "",  // propiedades reactivas
       nitproveedor:"",
       ivacompra : "",
       nombre_producto : "",
       precio_compra : "",
       precio_venta : "",
     }

    }
},
methods:{
  async loadUsers(){// trae los usuarios de la api
   await  api.get("/productos") // llamada a la api por medio de axios que arriba nombramos como "api"
    .then(response => (this.productos = response.data.productos // la respuesta de la api se guarda en la propiedad usuarios definida en la data
          ))
          .catch(e => {// se controlan errores
            console.log(e);
          });
  },

    displayDeleteDialog(props){// muestra o culpa el dialog tambien pasa información del objeto
      this.confirm = true  // si se ha llamado esta funcion se envia true a la propiedad,  esta propiedad inicia como false y cuando cambia a true muestra el dialog de eliminación
      this.eliminar = props  // al momento de llamar esta función recibe los datos de ese registro y lo pasamos a la propiedad eliminar que esta en data para usarlo. osea aqui viene el usuario con todos sus datos
    },
    deleteRow() { // elimina usuario
      const index = this.clientes.indexOf(this.eliminar.row);  // cogemos el indice del registro al que se le dio eliminar y se le pasa a indexOf con lo cual no da el usuario con ese indice y se guarda en la variable.
      api.delete("/productos/" + this.eliminar.row._id)
         .then(response => {
           this.triggerPositive (response.data, 'primary'); // se llama la funcion de notify para mostrar una notificación
         })
         .catch(e => {
           console.log(e);
         });
      //this.loadUsers();
      this.clientes.splice(index, 1);// borra cliente de la tabla ya que se elimino tambien del back pero la tabla no lo sabe
    },

    displayEditDialog(props){// muestra o culpa el dialog tambien pasa información del objeto
      this.confirmEdit = true // si se ha llamado esta funcion se envia true a la propiedad,  esta propiedad inicia como false y cuando cambia a true muestra el dialog de edicion
      this.confirm = props  // al momento de llamar esta función recibe los datos de ese registro y lo pasamos a la propiedad confirm que esta en data para usarlo. asi podemos recolocar los datos del usuario en el formulario de editar
      this.form.codigo_producto = this.confirm.row.codigo_producto  // a la propiedad cedulaUsuario que se creo en la data se le pasa la cedulaUsuario que se guardo en el confirm al momento de darle al boton de editar de ese registro
      this.form.ivacompra = this.confirm.row.ivacompra
      this.form.nitproveedor = this.confirm.row.nitproveedor
      this.form.nombre_producto = this.confirm.row.nombre_producto
      this.form.precio_compra = this.confirm.row.precio_compra
      this.form.precio_venta = this.confirm.row.precio_venta

    },
    async onSubmit(n){ // evento que se usa desde el formulario de edicion
     await api.put('/clientes/'+this.confirm.row._id, this.form).then(response => {
        console.log(response)
        this.triggerPositive ("Producto editado", 'primary') //llamada a notify para mostrar notificacion en caso de succes
      }).catch(e => {
        console.log(e);
        this.triggerPositive ("No fue posible completar la operación!", 'negative') //llamada a notify para mostrar notificacion en caso de error
      });
      await this.loadUsers(); // se llama de nuevo la funcion que trae los usuarios para llenar la tabla asi se ve el cambio
    },
    triggerPositive (mensaje, color) { // funcion de notify
      this.$q.notify({
        color: color,
        message: mensaje,
        position: 'bottom-right',
      })
    },


  },
 created(){// ejecuta codigo en una fase temprana de inicialización de la pagina
  this.loadUsers();// llama el metodo que carga la tabla de información
}
})
</script>
