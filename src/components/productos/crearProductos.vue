<template>
  <q-page class="flex-fluid flex-center">
    <q-form
     @submit="onSubmit"
     @reset="onReset"
     class="q-gutter-md"
     ref="myForm"
   >
    <div class="row">
           <div class="col q-px-sm">
               <q-input v-model="form.codigo_producto" label="Codigo Producto" :rules="[ val => val && val.length > 0 || 'Ingrese Codigo Producto.']"/>
           </div>
           <div class="col q-px-sm">
               <q-input  v-model="form.nitproveedor" label="Nit Proveedor" :rules="[ val => val && val.length > 0 || 'Ingrese el Nit Proveedor.']"/>
           </div>
           <div class="col q-px-sm">
                   <q-input v-model="form.ivacompra"
                    :dense="dense"
                    :rules="[ val => val && val.length > 0 || 'Ingrese Iva Compra.']"
                    >
        <template v-slot:append>
          <q-avatar size="16px" square>
            <img src="../../assets/porcentaje.png" style="width:80px">
          </q-avatar>
        </template>
      </q-input>
           </div>

 </div>
 <div class="row">

           <div class="col q-px-sm">
               <q-input v-model="form.nombre_producto" label="Nombre Producto" :rules="[ val => val && val.length > 0 || 'Ingrese Nombre Producto.']"/>
           </div>
           <div class="col q-px-sm">
               <q-input v-model="form.precio_compra" @change="calcularPrecioVenta" label="Precio Compra" :rules="[ val => val && val.length > 0 || 'Precio Compra.']"/>
           </div>
           <div class="col q-px-sm">
               <q-input v-model="form.precio_venta" readonly label="Precio Venta" />
           </div>
 </div>
 <q-separator />
        <div class="row q-mt-md q-mr-sm text-center" style="margin-top: 16px !important; margin-bottom: 16px !important">
            <div class="col q-px-sm">
                <q-btn color="primary"  type="submit" label="Crear Producto" />
            </div>
</div>
    </q-form>
  </q-page>
</template>

<script>
import { api } from 'boot/axios' //llamada a axios
import { defineComponent } from 'vue';
export default defineComponent({
  name: 'crearClientes',
  data() { // datos reactivos
    return {
      form:{
        codigo_producto : "",
        nitproveedor : "",
        ivacompra : "",
        nombre_producto : "",
        precio_compra : "",
        precio_venta: ''
      }
   }
},
    methods:{

onSubmit(n){
    api.post('/productos',
     this.form)
       .then(response => {
                  console.log(response)
                  this.triggerPositive ("Producto creado correctamente", 'primary') // llamada a la funcion que maneja las notificaciones por medio de notify
              }).catch(e => {
                 console.log(e);
                   this.triggerPositive ("No fue posible completar la operación!", 'negative')// llamada a la funcion que maneja las notificaciones por medio de notify
              });
        },
  triggerPositive (mensaje, color) { // funcion de notify
     this.$q.notify({
        color: color,
        message: mensaje,
         position: 'bottom-right',
      })
    },
    },
    computed:{
      calcularPrecioVenta(){
        this.form.precio_venta = ((this.form.ivacompra/100)+1)*this.form.precio_compra
      }
}

  })

</script>
