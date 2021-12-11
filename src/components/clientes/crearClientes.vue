<template>
<q-page>
<q-form
     @submit="onSubmit"
     @reset="onReset"
     class="q-gutter-md"
     ref="myForm"
   >
    <div class="row">
           <div class="col q-px-sm">
               <q-input v-model="form.cedula_cliente" label="Cedula"
                 :rules="[ val => val && val.length > 0 || 'Ingrese numero de cedula.']"
               />
           </div>
           <div class="col q-px-sm">
               <q-input  v-model="form.direccion_cliente" label="Dirección"
                  :rules="[ val => val && val.length > 0 || 'Ingrese Dirección.']"
                />
           </div>
           <div class="col q-px-sm">
             <q-input  v-model="form.email_cliente" label="Correo"
               :rules="[ val => val && val.length > 0 || 'Ingrese correo.']"
             />
           </div>

 </div>
 <div class="row">
           <div class="col q-px-sm">
               <q-input v-model="form.nombre_cliente" label="Nombre"
                 :rules="[ val => val && val.length > 0 || 'Ingrese nombre.']"
               />
           </div>
           <div class="col q-px-sm">
               <q-input v-model="form.telefono_cliente" label="Telefono"
                 :rules="[ val => val && val.length > 0 || 'Ingrese  Telefono.']"
                />
           </div>

 </div>
 <q-separator />
        <div class="row q-mt-md q-mr-sm text-center" style="margin-top: 16px !important; margin-bottom: 16px !important">
            <div class="col q-px-sm">
                <q-btn color="primary"  type="submit" label="Crear Cliente" />
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
        cedula_cliente : "",  // propiedades reactivas
        direccion_cliente:"",
       email_cliente : "",
       nombre_cliente : "",
       telefono_cliente : "",
      }
   }
},
    methods:{

onSubmit(n){ // funcion llamada por el formulario de creacion de usuarios
    // api.post('/usuarios?cedula='+this.cedulaUsuario+'&correo='+this.emailUsuario+'&nombre='+this.nombreUsuario+'&contrasena='+this.password+'&usuario='+this.usuario)
    api.post('/clientes',
     this.form)
       .then(response => {
                  console.log(response)
                  this.triggerPositive ("Cliente creado correctamente", 'primary') // llamada a la funcion que maneja las notificaciones por medio de notify
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
  })

</script>
