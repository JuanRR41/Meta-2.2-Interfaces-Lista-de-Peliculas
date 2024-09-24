<template>
  <v-card>
    <v-layout>
      <v-app-bar
        color="primary"
        prominent
        theme="dark"
        permanent
      >
        <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
        <v-toolbar-title>My files</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-app-bar>

      <v-navigation-drawer
        v-model="drawer"
        :location="$vuetify.display.mobile ? 'bottom' : undefined"
        temporary
        theme="dark"
        permanent
      >
        <v-list>
          <v-list-item
            v-for="(item, index) in items"
            :key="index"
            @click="navigateTo(item.ruta)"
          >
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-main>
        <v-card-text>
          <router-view />
        </v-card-text>
      </v-main>
    </v-layout>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    drawer: false,
    items: [
      {
        title: 'Main',
        icon: 'mdi-home',  // Añadir el nombre del icono
        ruta: '/',
      },
      {
        title: 'Peliculas',
        icon: 'mdi-film',  // Añadir el nombre del icono
        ruta: '/ListaPeliculas',
      },
      {
        title: 'Tarjeta',
        icon: 'mdi-card',  // Añadir el nombre del icono
        ruta: '/paginaTarjeta',
      },
    ],
  }),
  methods: {
    navigateTo(ruta) {
      this.$router.push(ruta);
      this.drawer = false; // Cierra el drawer después de la navegación
    },
  },
};
</script>
