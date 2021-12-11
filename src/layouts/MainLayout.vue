<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          v-if="this.$route.path !== '/'"
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Tienda Grupo 6
        </q-toolbar-title>

        <div>
          <q-btn
            v-if="this.$route.path !== '/'"
            flat
            dense
            round
            icon="logout"
            aria-label=""
            @click="salir"
          />
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-if="this.$route.path !== '/'"
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
          class="text-center" style="color: #366382
       !important; font-size: 16px !important; letter-spacing: 1px !important"
        >
          Menu
        </q-item-label>
        <EssentialLink
                 v-for="link in essentialLinks"
                 :key="link.title"
                 v-bind="link"
               />

      </q-list>
    </q-drawer>
    <q-page-container>
      <!-- vistas que seran dinamicas -->
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink.vue'
// rutas que se visualizaran en el menu
const linksList = [
  {
    title: 'Usuarios',
    caption: 'Usuarios',
    icon: 'person',
    link: '/usuarios'
  },
    {
      title: 'Clientes',
      caption: 'Clientes',
      icon: 'face',
      link: '/clientes'
    },
    {
      title: 'Productos',
      caption: 'Productos',
      icon: 'local_shipping',
      link: '/productos'
    },
    {
      title: 'Ventas',
      caption: 'ventas',
      icon: 'shopping_cart',
      link: '/ventas'
    },
    {
      title: 'Reportes',
      caption: 'reportes',
      icon: 'summarize',
      link: '/reportes'
    },
    {
      title: 'Consolidado',
      caption: 'consolidado',
      icon: 'leaderboard',
      link: '/consolidados'
    },

    // {
    //   title: 'Productos',
    //   caption: 'Productos',
    //   icon: 'inventory_2',
    //   link: '/productos'
    // },
    // {
    //   title: 'Ventas',
    //   caption: 'ventas',
    //   icon: 'shopping_cart',
    //   link: '/ventas'
    // },
    // {
    //   title: 'Reportes',
    //   caption: 'reportes',
    //   icon: 'summarize',
    //   link: '/reportes'
    // },
  ];
import { defineComponent, ref } from 'vue'
export default defineComponent({
  //nombre del componente
  name: 'MainLayout',
  components: {
    EssentialLink,
  },
  setup () {
    const leftDrawerOpen = ref(false)
    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  },
  methods:{
    salir(){
      this.$router.push({ path: `/` });
      this.$store.commit("noAutenticado");
    }
  },
  mounted() {
   if (this.$store.state.usuario === false) {
       this.$router.push({ path: `/` });
           }
 }
})
</script>
