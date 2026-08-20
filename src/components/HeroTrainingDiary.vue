<template>
  <div class="container py-4">
    <div class="row justify-content-center">
      <div class="col-md-8 col-lg-6">
        <div class="card diary-card">
          <div class="card-header text-center">
            <h1 class="diary-title">Hero Training Diary</h1>
            <p class="diary-subtitle">
              Registra tu energía después del entrenamiento
            </p>
          </div>

          <div class="card-body">
            <!-- Formulario de registro -->
            <form @submit.prevent="agregarRegistro" class="mb-4">
              <!-- Energía -->
              <div class="mb-3">
                <label class="form-label diary-label">
                  Nivel de energía (1–5)
                </label>
                <div class="d-flex gap-2">
                  <button
                    v-for="nivel in [1, 2, 3, 4, 5]"
                    :key="nivel"
                    type="button"
                    class="btn"
                    :class="energia === nivel ? 'btn-warning' : 'btn-outline-light'"
                    @click="energia = nivel"
                  >
                    {{ nivel }}
                  </button>
                </div>
              </div>

              <!-- Nota -->
              <div class="mb-3">
                <label for="nota" class="form-label diary-label">
                  Nota del entrenamiento (opcional)
                </label>
                <textarea
                  id="nota"
                  v-model="nota"
                  class="form-control diary-input"
                  rows="3"
                  placeholder="Ej: Entrenamiento de resistencia, 30 min de cardio..."
                ></textarea>
              </div>

              <!-- Botón agregar -->
              <div class="d-grid">
                <button type="submit" class="btn btn-warning diary-btn">
                  Agregar registro
                </button>
              </div>
            </form>

            <!-- Estado vacío -->
            <div
              v-if="registros.length === 0"
              class="text-center diary-empty"
            >
              <p class="diary-status">
                No hay registros aún. Agrega tu primer día de entrenamiento.
              </p>
            </div>

            <!-- Lista de registros (aún sin lógica completa) -->
            <div v-else>
              <p class="diary-status">
                Registros: {{ registros.length }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Array de registros de entrenamiento
const registros = ref([])

// Campos del formulario
const energia = ref(3)
const nota = ref('')

/**
 * Agrega un nuevo registro al diario.
 */
function agregarRegistro() {
  const registro = {
    id: Date.now(),
    fecha: new Date().toISOString(),
    energia: energia.value,
    nota: nota.value.trim()
  }

  registros.value.push(registro)

  // Limpiar formulario
  energia.value = 3
  nota.value = ''
}
</script>

<style scoped>
.diary-card {
  background: rgba(17, 24, 39, 0.8);
  border: 1px solid #374151;
  border-radius: 12px;
  box-shadow: 0 0 20px rgba(234, 179, 8, 0.2);
}

.diary-title {
  color: #facc15;
  font-weight: 800;
  letter-spacing: 1px;
  text-transform: uppercase;
  font-size: 1.6rem;
}

.diary-subtitle {
  color: #fde047;
  font-size: 0.9rem;
  margin-bottom: 0;
}

.diary-label {
  color: #fde047;
  font-weight: 600;
  font-size: 0.9rem;
}

.diary-input {
  background: rgba(31, 41, 55, 0.7);
  border: 1px solid #374151;
  color: #e5e7eb;
}

.diary-input:focus {
  background: rgba(31, 41, 55, 0.9);
  border-color: #facc15;
  color: #e5e7eb;
  box-shadow: 0 0 0 0.2rem rgba(250, 204, 21, 0.25);
}

.diary-btn {
  background: linear-gradient(90deg, #facc15, #fde047);
  border: none;
  font-weight: 600;
  color: #0b0f19;
}

.diary-btn:hover {
  background: linear-gradient(90deg, #eab308, #facc15);
}

.diary-empty {
  padding: 2rem 0;
}

.diary-status {
  color: #fde047;
  font-size: 0.95rem;
}
</style>