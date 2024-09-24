<template>
  <v-container class="d-flex justify-center align-center fill-height">
    <v-row>
      <v-col>
        <v-card>
          <v-card-title class="text-h5">
            {{ titulo }}
            <v-spacer></v-spacer>
            <!-- Botón para agregar nueva película -->
            <v-btn color="primary" @click="abrirDialogAgregar">
              <v-icon left>mdi-plus</v-icon>
              Agregar Película
            </v-btn>
          </v-card-title>

          <v-card-text class="scroll-container">
            <v-data-table
              :headers="headers"
              :items="peliculas"
              class="elevation-1"
              :sort-by="['rating']"
              :sort-desc="[true]" 
            >
              <!-- Mostrar el nombre de la película como enlace -->
              <template v-slot:item.movie="{ item }">
                <a href="#" @click.prevent="cambiarTitulo(item.movie)">
                  {{ item.movie }}
                </a>
              </template>

              <!-- Mostrar el enlace de IMDb -->
              <template v-slot:item.imdb_url="{ item }">
                <a :href="item.imdb_url" target="_blank">Ver en IMDB</a>
              </template>

              <!-- Botones de editar y eliminar -->
              <template v-slot:item.actions="{ item, index }">
                <v-btn icon @click="editarPelicula(item)">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn icon @click="eliminarPelicula(index)">
                  <v-icon color="red">mdi-delete</v-icon>
                </v-btn>
              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Dialog para editar/agregar la película -->
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title class="headline">{{ esEdicion ? 'Editar' : 'Agregar' }} Película</v-card-title>
        <v-card-text>
          <v-form ref="form">
            <v-text-field
              v-model="peliculaEditada.movie"
              label="Nombre de la película"
              required
            ></v-text-field>
            <v-text-field
              v-model="peliculaEditada.rating"
              label="Rating"
              required
              type="number"
            ></v-text-field>
            <v-text-field
              v-model="peliculaEditada.imdb_url"
              label="Enlace IMDB"
              required
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-btn text @click="dialog = false">Cancelar</v-btn>
          <v-btn text @click="esEdicion ? guardarEdicion() : agregarPelicula()">Guardar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import { onMounted, ref } from 'vue';

export default {
  setup() {
    const peliculas = ref([]);
    const titulo = ref('Lista de Películas');
    const dialog = ref(false);
    const peliculaEditada = ref({});
    const editIndex = ref(null);
    const esEdicion = ref(false); // Determina si es edición o agregar

    // Encabezados de la tabla, agregando columna de acciones
    const headers = ref([
      { text: 'Nombre de la Película', value: 'movie' },
      { text: 'Rating', value: 'rating' },
      { text: 'Enlace', value: 'imdb_url' },
      { text: 'Acciones', value: 'actions', sortable: false }
    ]);

    // Función asíncrona para obtener los datos de la API
    const obtenerPeliculas = async () => {
      try {
        const respuesta = await fetch('https://dummyapi.online/api/movies');
        const data = await respuesta.json();
        peliculas.value = data;
        ordenarPorRating();
      } catch (error) {
        console.error('Error al obtener las películas:', error);
      }
    };

    // Ejecutar la función cuando el componente se monte
    onMounted(() => {
      obtenerPeliculas();
    });

    // Función para cambiar el título
    const cambiarTitulo = (nombrePelicula) => {
      titulo.value = nombrePelicula;
    };

    // Función para eliminar una película
    const eliminarPelicula = (index) => {
      peliculas.value.splice(index, 1);
      ordenarPorRating();
    };

    // Función para abrir el diálogo de edición con los datos de la película seleccionada
    const editarPelicula = (pelicula) => {
      esEdicion.value = true;
      peliculaEditada.value = { ...pelicula }; // Hacer una copia de la película seleccionada
      editIndex.value = peliculas.value.indexOf(pelicula);
      dialog.value = true; // Mostrar el diálogo
    };

    // Función para guardar los cambios en la película editada
    const guardarEdicion = () => {
      if (editIndex.value !== null) {
        peliculas.value[editIndex.value] = { ...peliculaEditada.value };
        ordenarPorRating();
      }
      dialog.value = false; // Cerrar el diálogo
    };

    // Función para abrir el diálogo de agregar nueva película
    const abrirDialogAgregar = () => {
      esEdicion.value = false;
      peliculaEditada.value = { movie: '', rating: '', imdb_url: '' }; // Limpiar los campos
      dialog.value = true; // Mostrar el diálogo
    };

    // Función para agregar una nueva película
    const agregarPelicula = () => {
      peliculas.value.push({ ...peliculaEditada.value });
      ordenarPorRating();
      dialog.value = false; // Cerrar el diálogo
    };

    // Función para ordenar las películas por rating de mayor a menor
    const ordenarPorRating = () => {
      peliculas.value.sort((a, b) => b.rating - a.rating);
    };

    return {
      peliculas,
      titulo,
      headers,
      dialog,
      peliculaEditada,
      esEdicion,
      cambiarTitulo,
      eliminarPelicula,
      editarPelicula,
      guardarEdicion,
      abrirDialogAgregar,
      agregarPelicula
    };
  }
};
</script>

<style scoped>
.fill-height {
  height: 100vh;
}

.scroll-container {
  max-height: 400px;
  overflow-y: auto;
}

.v-card {
  max-width: 800px;
  margin: 0 auto;
}

a {
  color: #1e90ff;
  text-decoration: none;
}

a:hover {
  color: #41ff8a;
}
</style>
