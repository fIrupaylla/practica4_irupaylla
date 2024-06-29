<template>
  <div class="nasa-list">
    <div class="nasa-grid">
      <!-- Aqui empieza mi contenedor -->
      <div class="nasa-image" v-for="row in imagesNasa" :key="row.date">
        <!-- Aqui empieza mi q-card -->
        <q-card class="nasa-card" v-if="row.media_type === 'image'">
          <q-card-section class="image-container">
            <q-img :src="row.url" alt="" class="image-content"></q-img>
          </q-card-section>
          <q-card-section>
            <div class="text-h6 font-weight-bold">
              <q-card-title class="custom-title">{{ row.title }}</q-card-title>
              <div class="text-h5">Tomada el {{ row.date }}</div>
            </div>
          </q-card-section>
        </q-card>
        <!-- Aqui acaba para el IF -->
      </div>
      <!-- Aqui acaba mi contenedor -->
    </div>
  </div>
</template>

<style>
.nasa-list {
  background-color: #f5f5f5; /* Opcional: color de fondo */
  border-radius: 8px; /* Opcional: bordes redondeados */
}
.nasa-image {
  margin-bottom: 20px;
}

.nasa-card {
  width: 100%;
  height: 300px;
  display: flex;
  flex-direction: column;
}

.image-container {
  height: 70%;
  overflow: hidden;
}

.image-content {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.custom-title {
  color: #666; /* Tono rojo */
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bold;
  text-transform: uppercase;
  font-size: 14px;
}
.text-h5 {
  color: #555; /* Tono rojo */
  font-family: Arial, Helvetica, sans-serif;
  font-size: 12px;
  font-style: italic;
}

.nasa-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
}
</style>

<script>
export default {
  name: "NasaList",
  components: {},
  data() {
    return {
      imagesNasa: [],
    };
  },
  mounted() {
    this.loadImages();
  },
  methods: {
    loadImages() {
      var URL = "/planetary/apod";
      var token = "a21IcLJk5Wkn97X9OreijgBkwnsyveXrEEWXeHXF";

      var startDate = localStorage.getItem("inicioDate");
      var endDate = localStorage.getItem("finDate");

      if (!startDate) {
        startDate = this.getCurrentDate();
        localStorage.setItem("inicioDate", startDate.value);
      }
      if (!endDate) {
        endDate = this.getCurrentDate();
        localStorage.setItem("finDate", endDate.value);
      }
      this.$api
        .get(URL, {
          params: {
            api_key: token,
            start_date: startDate,
            end_date: endDate,
          },
        })
        .then((response) => {
          this.imagesNasa = response.data;
          console.log(JSON.stringify(response));
        })
        .catch((error) => {
          console.log("Ocurrio un Error: ");
        });
    },

    getCurrentDate() {
      const today = new Date();
      const year = today.getFullYear();
      const month = String(today.getMonth() + 1).padStart(2, "0");
      const day = String(today.getDate()).padStart(2, "0");
      return `${year}-${month}-${day}`;
    },
  },
};
</script>
