<template>
  <v-container class="d-flex justify-center align-center fill-height">
    <v-row>
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title class="text-h5">
            Cargar Imagen Aleatoria
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="cargarImagen">
              <v-icon left>mdi-refresh</v-icon>
              Cargar Otra Imagen
            </v-btn>
          </v-card-title>

          <v-card-text class="d-flex justify-center">
            <!-- Mensaje de error si hay problemas -->
            <v-alert v-if="error" type="error" dismissible class="mb-3">
              {{ error }}
            </v-alert>

            <!-- Progress circular mientras se carga la imagen -->
            <v-progress-circular
              v-if="cargando"
              indeterminate
              color="primary"
              size="64"
            ></v-progress-circular>

            <!-- Imagen cargada -->
            <img
              v-if="!cargando && !error"
              :src="imagenUrl"
              alt="Imagen Aleatoria"
              class="elevation-2"
              width="200"
              height="300"
            />
          </v-card-text>

          <!-- Progress linear mientras se carga la imagen -->
          <v-progress-linear
            v-if="cargando"
            indeterminate
            color="primary"
            class="mb-3"
          ></v-progress-linear>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const imagenUrl = ref('');
    const cargando = ref(false);
    const error = ref('');

    // Función para cargar la imagen
    const cargarImagen = async () => {
      cargando.value = true;
      error.value = ''; // Limpiar cualquier error previo

      try {
        // Generar nueva URL de imagen aleatoria
        const nuevaImagenUrl = `https://picsum.photos/200/300?random=${new Date().getTime()}`;

        // Crear un nuevo objeto de imagen para asegurar que se cargue antes de actualizar
        const img = new Image();
        img.src = nuevaImagenUrl;

        // Esperar hasta que la imagen se haya cargado correctamente
        img.onload = () => {
          imagenUrl.value = nuevaImagenUrl;
          cargando.value = false;
        };

        // Manejar error de carga de imagen
        img.onerror = () => {
          manejarError();
        };
      } catch (e) {
        manejarError();
      }
    };

    // Función para manejar errores de carga
    const manejarError = () => {
      error.value = 'Error al cargar la imagen. Inténtalo nuevamente.';
      cargando.value = false;
    };

    // Cargar una imagen al montar el componente
    cargarImagen();

    return {
      imagenUrl,
      cargando,
      error,
      cargarImagen,
      manejarError,
    };
  },
};
</script>

