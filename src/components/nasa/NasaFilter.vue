<template>
  <div class="nasa-filter q-pa-md">
    <q-form @submit.prevent="handleSearch" class="q-gutter-md">
      <div class="q-gutter-sm q-mb-md">
        <!-- Campo para Fecha de Inicio -->
        <q-input
          v-model="inicioDate"
          label="Fecha de Inicio"
          mask="####-##-##"
          placeholder="AAAA-MM-DD"
          filled
          :rules="[
            (val) =>
              (val && val.length === 10) || 'Fecha de inicio es obligatoria',
          ]"
          hint="Formato: YYYY-MM-DD"
          @blur="validateDate(inicioDate)"
        >
          <template v-slot:append>
            <q-icon name="calendar_today" @click="openDatePicker('start')" />
          </template>
          <q-popup-proxy
            v-if="datePickerType === 'start'"
            cover
            transition-show="jump-down"
            transition-hide="jump-up"
          >
            <q-date
              v-model="inicioDate"
              mask="YYYY-MM-DD"
              locale="es"
              :min-date="minDate"
              :max-date="maxDate"
              @input="validateDate(inicioDate)"
              @click="closeDatePicker"
            />
          </q-popup-proxy>
        </q-input>

        <!-- Campo para Fecha de Fin -->
        <q-input
          v-model="finDate"
          label="Fecha de Fin"
          mask="####-##-##"
          placeholder="AAAA-MM-DD"
          filled
          :rules="[
            (val) =>
              (val && val.length === 10) || 'Fecha de fin es obligatoria',
          ]"
          hint="Formato: YYYY-MM-DD"
          @blur="validateDate(finDate)"
        >
          <template v-slot:append>
            <q-icon name="calendar_today" @click="openDatePicker('end')" />
          </template>
          <q-popup-proxy
            v-if="datePickerType === 'end'"
            cover
            transition-show="jump-down"
            transition-hide="jump-up"
          >
            <q-date
              v-model="finDate"
              mask="YYYY-MM-DD"
              locale="es"
              :min-date="minDate"
              :max-date="maxDate"
              @input="validateDate(finDate)"
              @click="closeDatePicker"
            />
          </q-popup-proxy>
        </q-input>
        <q-btn
          type="submit"
          label="Consultar"
          color="primary"
          :loading="loading"
          :disable="!inicioDate || !finDate"
        />
      </div>
    </q-form>

    <q-dialog v-model="dialogOpen" persistent>
      <q-card>
        <q-card-section>
          <div class="text-h6">Búsqueda realizada</div>
          <p>Fecha de Inicio: {{ inicioDate }}</p>
          <p>Fecha de Fin: {{ finDate }}</p>
        </q-card-section>
        <q-card-actions>
          <q-btn flat label="Cerrar" @click="dialogOpen = false" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "DateRangeForm",
  setup() {
    const inicioDate = ref("");
    const finDate = ref("");
    const datePickerType = ref("");
    const loading = ref(false);
    const dialogOpen = ref(false);

    const minDate = ref("1900-01-01");
    const maxDate = ref("2100-12-31");

    const openDatePicker = (type) => {
      datePickerType.value = type;
    };

    const closeDatePicker = () => {
      datePickerType.value = "";
    };

    const validateDate = (date) => {
      if (!date.match(/^\d{4}-\d{2}-\d{2}$/)) {
        date = "";
      }
    };

    const handleSearch = () => {
      if (inicioDate.value > finDate.value) {
        alert("La fecha de fin no puede ser anterior a la fecha de inicio.");
        return;
      }

      loading.value = true;
      console.log(inicioDate.value);
      console.log(finDate.value);

      localStorage.setItem("inicioDate", inicioDate.value);
      localStorage.setItem("finDate", finDate.value);

      setTimeout(() => {
        loading.value = false;
        window.location.reload();
      }, 1000); // Simulando una búsqueda con un retraso de 1 segundo
    };

    return {
      finDate,
      inicioDate,
      minDate,
      maxDate,
      datePickerType,
      loading,
      dialogOpen,
      openDatePicker,
      closeDatePicker,
      validateDate,
      handleSearch,
    };
  },
});
</script>

<style scoped>
.nasa-filter {
  width: 300px; /* Asegúrate de dar un ancho fijo */
  background-color: #f5f5f5; /* Opcional: color de fondo */
  border-radius: 8px; /* Opcional: bordes redondeados */
}
</style>
