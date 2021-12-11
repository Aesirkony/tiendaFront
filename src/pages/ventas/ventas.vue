<template>
  <q-page class="flex flex-center">
    <q-card class="cent q-ma-md q-pa-md" style="margin-top: 30px;" >
      <q-card-title class="text-h6">
          Cliente
      </q-card-title>
         <q-card-section>
           <div class="row">
             {{cliente}}
              <div class="col-7 q-px-md">
           <q-input bottom-slots v-model="client" label="Cedula Cliente"  counterd>
       <template v-slot:before>
         <q-icon name="person" />
       </template>

       <template v-slot:append>
         <q-icon v-if="client !== ''" name="close" @click="client = ''" class="cursor-pointer" />
         <q-icon name="search" />
       </template>
     </q-input>
        </div>
        <div class="flex flex-center col  ">
           <q-btn color="primary" @click="loadClient()" class="full-width" label="Buscar" />

  </div>
  <div class="flex flex-center col ">
     <q-btn color="secondary" @click="clearClient()" class="full-width" label="Limpiar" />
</div>
   </div>
         </q-card-section>
         <q-card-section>
           <div class="row">
              <div class="col">
                <q-table
      title="Cliente seleccionado"
      :rows="cliente"
      :columns="columns"
      row-key="name"
    />
        </div>
   </div>
         </q-card-section>
       </q-card>
       <!-- prueba -->

       <q-card class="q-ma-md q-pa-md cent" >
         <q-card-title class="text-h6">
         </q-card-title>
            <q-card-section >
       <q-list bordered class="rounded-borders" v-for="(p, index) in compra" key="index">
            <q-item-label header>Productos seleccionados</q-item-label>
            <q-item >
              <q-item-section avatar top>
                <q-icon name="dashboard" color="black" size="34px" />
              </q-item-section>
              <q-item-section top class="col-2 gt-sm">
                <q-item-label lines="1" class="q-mt-sm"> Codigo: {{p.codigo_producto}} - Producto: {{p.nombre_producto}}</q-item-label>
                <q-item-label lines="1">
                  <span class="text-weight-medium"> Precio Venta:{{p.precio_venta}} </span>

                </q-item-label>
              </q-item-section>
              <q-item-section top>
                <q-item-label lines="1">
                  <span class="text-grey-8"> Precio Compra: {{p.precio_compra}} </span>
                </q-item-label>
                <q-item-label caption lines="1">
                Iva Producto: {{p.ivacompra}}
                </q-item-label>
                <q-item-label caption lines="1">
                 Nit Proveedor:{{p.nitproveedor}}
                </q-item-label>
                <q-item-label lines="1" class="q-mt-xs text-body2 text-weight-bold text-primary text-uppercase">
                  <!-- <span class="cursor-pointer">Open in GitHub</span> -->
                </q-item-label>
              </q-item-section>
              <q-item-section top>
                <div class="text-grey-8 q-gutter-xs">
                <q-input v-model="cantidad[index]"
                 @change="numberPostive(index)"
                 type="number"
                value="1"
                 min="1" />
                </div>
              </q-item-section>
              <q-item-section top>
                <div class="text-grey-8 q-gutter-xs col-2 gt-sm" >
                  <q-item-label lines="1" class="q-mt-xs text-body2 text-weight-bold text-center text-uppercase">
                  <span> $ {{cantidad[index]*p.precio_venta}}</span>
                  </q-item-label>

                </div>
              </q-item-section>
              <q-item-section top side>
                <div class="text-grey-8 q-gutter-xs">
                  <q-btn class="gt-xs" size="12px" flat dense round icon="delete" @click="deleteItem(index)" />
                  <!-- <q-btn class="gt-xs" size="12px" flat dense round icon="done" /> -->
                  <!-- <q-btn size="12px" flat dense round icon="more_vert" /> -->
                </div>
              </q-item-section>
            </q-item>

          </q-list>
          <div class="text-h6 text-weight-bold text-center text-uppercase q-ma-sm" >
            Total iva:$ {{totalIva}} - Total Compra sin Iva:$ {{totalCompra}}  - Total Compra: ${{totalCompraIva}} - consecutivo {{consecutivo}}
          </div>
          <div class="  text-center text-uppercase q-ma-sm" >
          <q-btn color="primary" class="full-width" @click="generaCompra()" label="Aceptar Compra" />
          </div>
        </q-card-section>
</q-card>
  <!-- muestra productos y filtrado -->
<q-card class="q-ma-md q-pa-lg cent" >
         <q-card-title class="text-h6">
           Productos
         </q-card-title>
         <q-card-section>
           <div class="row">
              <div class="col-12 q-px-md">
           <q-input bottom-slots v-model="product" label="Codigo producto"  counterd>
       <template v-slot:before>
         <q-icon name="inventory_2" />
       </template>
       <template v-slot:append>
         <q-icon v-if="product !== ''" name="close" @click="product = ''" class="cursor-pointer" />
         <q-icon name="search" />
       </template>
     </q-input>
        </div>
        <div class="flex flex-center col  ">
           <!-- <q-btn color="primary" @click="loadProduct()" class="full-width" label="Agregar" /> -->
  </div>
  <div class="flex flex-center col ">
     <!-- <q-btn color="secondary" @click="clearProduct()" class="full-width" label="Limpiar" /> -->
</div>
   </div>
         </q-card-section>
         <q-card-section>
           <!-- <q-card class="q-ma-md q-pa-lg" v-for="p in producto">
             <q-card-title class="text-h6">
             {{p.codigoProducto}}
             </q-card-title>
             <q-card-section>
               <q-separator/>
              <q-icon name="style" />Iva Producto: {{p.ivaCompra}} Nit Proveedor:{{p.nitProveedor}} NombreProducto:{{p.nombreProducto}} Precio Compra: {{p.precioCompra}} Precio Venta:{{p.precioVenta}}
             </q-card-section>
           </q-card> -->
           <div class="row justify-center">
             <div class="col-xs-12 col-sm-5 col-md-3 q-ma-md" v-for="p in items">
           <q-card class="my-card "  >
     <q-img src="https://random.imagecdn.app/500/150" />

     <q-card-section>
       <!-- <q-btn
         fab
         color="primary"
         icon="attach_money"
         class="absolute"
         style="top: 0; right: 12px; transform: translateY(-50%);"
       /> -->
        {{p.codigo_producto}} - {{p.nombre_producto}}
       <div class="row no-wrap items-center">
         <div class="col text-h6 ellipsis">
         </div>
         <div class="col-auto text-grey text-caption q-pt-md row no-wrap items-center">
            Precio de venta: <q-icon name="euro" />
         {{p.precio_venta}}
         </div>
       </div>

       <!-- <q-rating v-model="stars" :max="5" size="32px" /> -->
     </q-card-section>

     <q-card-section class="q-pt-none">
       <div class="text-subtitle1">
        precio Compra  ${{p.precio_compra}}
       </div>
       <div class="text-caption text-grey">
         Nit Proveedor:{{p.nitproveedor}}
         <q-separator />
         Iva Producto: {{p.ivacompra}}
       </div>
     </q-card-section>

     <q-separator />

     <q-card-actions>
         <q-icon name="credit_card" />

       <q-btn flat color="primary" @click="add(p)">
         Comprar
       </q-btn>
     </q-card-actions>
           </q-card>
    </div>
 </div>
         </q-card-section>
</q-card>
{{$data}}
    </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import { ref } from 'vue'
import { api } from 'boot/axios'
export default defineComponent({
  name: 'Ventas',
  components: {
  },
  data(){
    return{
      client:ref(''),
      cliente: ref([]),
      product: ref(''),
      producto:ref([]),
      productos:ref([]),
      compra:ref([]),
      cantidad:ref([]),
      consecutivo:ref()
    }
  },
  methods:{
    async loadClient(){

      let cedula = this.client
      await api.get("/clientes/cedula/"+cedula)
      .then(response => (this.cliente = response.data
            ))
            .catch(e => {
              console.log(e);
            });
            if(this.cliente.length === 0){
              this.triggerPositive ("No se ha encontrado el cliente!", 'negative');
            }
    },
    clearClient(){
      this.client=""
      this.cliente=[]
    },
   loadProduct(){
     api.get("/productos")
      .then(response => (this.producto = response.data.productos
            ))
            .catch(e => {
              console.log(e);
            });
    },
    clearProduct(){
      this.product=""
      this.producto=[]
    },
    triggerPositive (mensaje, color) {
       this.$q.notify({
          color: color,
          message: mensaje,
           position: 'bottom-right',
        })
      },
      add(value){
        this.compra.push(value);
        this.cantidad.push(1);
      },
      deleteItem(index){
            this.compra.splice(index, 1);
            this.cantidad.splice(index, 1);
      },
      numberPostive(index){
        if(this.cantidad[index] < 0){
        this.cantidad[index] = 0
        }
      },
      async generaCompra(){
        await  api.post('/ventas',   {
          detalle_venta: {
            cantidad_producto: 10,
            codigo_producto: 10 ,
            valor_total: 1200,
            valor_venta: 1600,
            valorIva: 70
          },
          cedula_cliente: this.client,
          codigo_venta: 4,
          ivaventa: this.totalIva,
          total_venta: this.totalCompra,
          valor_venta: this.totalCompraIva
        }).then(response => {
              console.log(response)
              this.consecutivo = response.data._id
              this.triggerPositive ("se ha guardado exitosamente la venta", 'primary')
          }).catch(e => {
             console.log(e);
               this.triggerPositive ("No fue posible completar la operación!", 'negative')
             });
       },
      registraProducto(){
        for(var key in this.compra){
          let ivaTotalProducto = this.compra[key].ivaCompra*this.cantidad[key]
          let valorTotalProducto = this.compra[key].precioVenta*this.cantidad[key]
          console.log(valorTotalProducto)
          api.post('/registrarDetalleVenta?cantidadProducto='+this.cantidad[key]+'&codigoProducto='
          +this.compra[key].codigoProducto+
          '&codigoVenta='+this.consecutivo+'&valorIva='+ivaTotalProducto+'&valorTotal='+valorTotalProducto+'&valorVenta='+this.compra[key].precioVenta).then(response => {
                console.log(response)
            }).catch(e => {
               console.log(e);
                 this.triggerPositive ("No fue posible completar la operación!", 'negative')
               });
        }
      },
  },
  created(){
    this.loadProduct();
},
computed:{
  items() {
     return this.producto.filter(item => {
       return item._id.toString().toLowerCase().includes(this.product.toLowerCase());
     });
   },
   totalCompra(){
     var compra = 0;
     var cantidad = this.cantidad;
     this.compra.forEach(function(elemento, indice){
       compra += elemento["precio_compra"]*cantidad[indice];
     })
     return compra;
   },
   totalCompraIva(){
     var compra = 0;
     var cantidad = this.cantidad;
     this.compra.forEach(function(elemento, indice){
       compra += (elemento["precio_venta"]*cantidad[indice]);
     })
     return compra;
   },
   totalIva(){
     var ivaTotal = 0;
     var cantidad = this.cantidad;
     this.compra.forEach(function(elemento, indice){
       ivaTotal += ((elemento["ivacompra"]/100)*elemento["precio_compra"])*cantidad[indice];
     })
     return ivaTotal;
   },
     getCedulaUsuario(){
       return this.$store.cedulaUsuario
   },
},
mounted() {
 if (this.$store.state.usuario === false) {
     this.$router.push({ path: `/` });
         }
    console.log(this.$store.state.cedulaUsuario)
}
})
</script>

<style>
 .cent{
   width: 100%;
 }
</style>
