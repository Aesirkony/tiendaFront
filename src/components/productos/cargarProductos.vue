<template>
  <q-page class="flex flex-center">
    <div class="q-gutter-md row justify-center items-center">
        <q-file
         bg-color="grey"
          v-model="fileCsv"
          filled
          label="Seleccione archivo csv"
          accept=".csv"
           counter
          @rejected="onRejected"
        >
        <template v-slot:prepend>
          <q-icon name="attach_file" />
        </template>
        <template v-if="fileCsv" v-slot:append>
          <q-icon name="cancel" @click.stop.prevent="fileCsv = null" class="cursor-pointer" />
        </template>

      </q-file>
<q-btn color="primary" @click="csvtojson"  label="Cargar Archivo" />

      </div>
  </q-page>
</template>

<script>
import { api } from 'boot/axios'
import { defineComponent } from 'vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'listarProveedor',
  data() {
   return {
     fileCsv: ref(null),
      result: ref(null),
   }
},
methods:{
  onRejected (rejectedEntries) {
      this.$q.notify({
          type: 'negative',
          message: `${rejectedEntries.length} file(s) did not pass validation constraints`
        })
      },
csvtojson(){
  var reader = new FileReader();
             var vm = this;
             reader.onload = (e) => {
             vm.result = reader.result;
             }
             reader.readAsText(this.fileCsv);
             console.log(this.fileCsv);
}
}
})
</script>
