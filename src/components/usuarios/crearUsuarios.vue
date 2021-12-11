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
               <q-input v-model="form.cedula" label="Cedula"
                 :rules="[ val => val && val.length > 0 || 'Ingrese numero de cedula.']"
               />
           </div>
           <div class="col q-px-sm">
               <q-input  v-model="form.correo" label="Correo"
                  :rules="[ val => val && val.length > 0 || 'Ingrese correo.']"
                />
           </div>
 </div>
 <div class="row">
           <div class="col q-px-sm">
               <q-input v-model="form.nombre" label="Nombre"
                 :rules="[ val => val && val.length > 0 || 'Ingrese nombre.']"
               />
           </div>
           <div class="col q-px-sm">
               <q-input type="password" v-model="form.contrasena" label="Contraseña"
                 :rules="[ val => val && val.length > 0 || 'Ingrese  contraseña.']"
                />
           </div>
           <div class="col q-px-sm">
               <q-input v-model="form.usuario" label="Usuario"
                  :rules="[ val => val && val.length > 0 || 'Ingrese  usuario.']"
                />
           </div>
 </div>
 <q-separator />
        <div class="row q-mt-md q-mr-sm text-center" style="margin-top: 16px !important; margin-bottom: 16px !important">
            <div class="col q-px-sm">
                <q-btn color="primary"  type="submit" label="Crear Usuario" />
            </div>
</div>
</q-form>
</q-page>
</template>


<script>
import { api } from 'boot/axios' //llamada a axios
import { defineComponent } from 'vue';
export default defineComponent({
  name: 'crearUsuarios',
  data() { // datos reactivos
    return {
      form:{
       cedula : "",  // propiedades reactivas
       correo : "",
       nombre : "",
       contrasena : "",
        usuario : ""
      }
   }
},
    methods:{

onSubmit(n){ // funcion llamada por el formulario de creacion de usuarios
    // api.post('/usuarios?cedula='+this.cedulaUsuario+'&correo='+this.emailUsuario+'&nombre='+this.nombreUsuario+'&contrasena='+this.password+'&usuario='+this.usuario)
    console.log(this.form);
    api.post('/usuarios',
     this.form)
       .then(response => {
                  console.log(response)
                  this.triggerPositive (response.data, 'primary') // llamada a la funcion que maneja las notificaciones por medio de notify
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
