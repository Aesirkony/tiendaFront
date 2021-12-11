<template>
  <q-page class="flex flex-center bg-dark">
       <q-form  class="q-gutter-md"
       @submit="onSubmit"
        style="  position: absolute; top: 15vh">
      <q-card class="q-pa-lg" square style="width: 300:px;" color="grey">
        <q-card-title class="text-h5">
        Login
      </q-card-title>
        <q-card-section>

               <q-input square filled  v-model="form.usuario" type="text" label="Usuario"
                 :rules="[ val => val && val.length > 0 || 'Ingrese Usuario.']"
                />
                <q-input square filled v-model="form.contrasena" :type="isPwd ? 'password' : 'text'" label="Contraseña"
                   :rules="[ val => val && val.length > 0 || 'Ingrese contraseña.']"
                >
         <template v-slot:append>
           <q-icon
             :name="isPwd ? 'visibility_off' : 'visibility'"
             class="cursor-pointer"
             @click="isPwd = !isPwd"
           />
         </template>
       </q-input>

      </q-card-section>
      <q-card-actions class="q-px-md">
            <q-btn unelevated type="submit" color="primary" size="lg" class="full-width" label="Login" />
      </q-card-actions>
      <div class="q-px-md">

        <q-select v-model="model" :options="options" label="sucursal" />
      </div>

    </q-card>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { ref } from 'vue'
import { api } from 'boot/axios'
export default defineComponent({
  name: 'PageIndex',
  data(){
    return{
      form:{
        usuario:ref(''),
        contrasena:ref('')
      },
      isPwd: ref(true),
      model: "",
      options: [
        'Cali', 'Medellin', 'Twitter', 'Bogotá'
      ]
    }
  },
  methods:{
    onSubmit(){
      api.post('/auth/login', this.form).then(response => {
        this.$router.push({path: `/usuarios`})
        this.$store.commit('autenticado')
      }).catch(e => {
        this.triggerPositive ("Información de usuario incorrecta", 'negative');
      })

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
