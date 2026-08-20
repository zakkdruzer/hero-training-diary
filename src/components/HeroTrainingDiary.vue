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
                  <button v-for="nivel in [1, 2, 3, 4, 5]" :key="nivel" type="button" class="btn"
                    :class="energia === nivel ? 'btn-warning' : 'btn-outline-light'" @click="energia = nivel">
                    {{ nivel }}
                  </button>
                </div>
              </div>

              <!-- Nota -->
              <div class="mb-3">
                <label for="nota" class="form-label diary-label">
                  Nota del entrenamiento (opcional)
                </label>
                <textarea id="nota" v-model="nota" class="form-control diary-input" rows="3"
                  placeholder="Ej: Entrenamiento de resistencia, 30 min de cardio..."></textarea>
              </div>

              <!-- Botón agregar -->
              <div class="d-grid">
                <button type="submit" class="btn btn-warning diary-btn">
                  Agregar registro
                </button>
              </div>
            </form>

            <!-- Estado vacío -->
            <div v-if="registros.length === 0" class="text-center diary-empty">
              <p class="diary-status">
                No hay registros aún. Agrega tu primer día de entrenamiento.
              </p>
            </div>

            <!-- Lista de registros -->
            <div v-else>
              <!-- Resumen -->
              <div class="card diary-resumen-card mb-3">
                <div class="card-body">
                  <h5 class="diary-resumen-title">Resumen de entrenamiento</h5>
                  <div class="row g-2">
                    <div class="col-4">
                      <span class="diary-resumen-label">Promedio:</span>
                      <p class="diary-resumen-value">{{ promedioEnergia }}</p>
                    </div>
                    <div class="col-4">
                      <span class="diary-resumen-label">Mejor día:</span>
                      <p class="diary-resumen-value">
                        {{ diaMejor ? diaMejor.energia : '-' }}
                      </p>
                    </div>
                    <div class="col-4">
                      <span class="diary-resumen-label">Peor día:</span>
                      <p class="diary-resumen-value">
                        {{ diaPeor ? diaPeor.energia : '-' }}
                      </p>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Lista de registros -->
              <div class="list-group">
                <div v-for="registro in registrosOrdenados" :key="registro.id" class="list-group-item diary-list-item">
                  <div class="d-flex justify-content-between align-items-center">
                    <div>
                      <h6 class="mb-1 diary-item-energia">
                        Energía: {{ registro.energia }} / 5
                      </h6>
                      <small class="diary-item-fecha">
                        {{ new Date(registro.fecha).toLocaleDateString() }}
                      </small>
                      <p class="mb-0 diary-item-nota">
                        {{ registro.nota || 'Sin nota' }}
                      </p>
                    </div>
                    <button class="btn btn-sm btn-outline-danger" @click="eliminarRegistro(registro.id)">
                      Eliminar
                    </button>
                  </div>
                </div>
              </div>
            </div>
            <!-- Botón borrar todo -->
            <div class="mt-3">
              <button class="btn btn-danger w-100 diary-btn-delete" @click="borrarTodo">
                Borrar todos los registros
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted, computed } from 'vue'

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

/**
 * Watch deep para guardar los registros en localStorage.
 * Se ejecuta cada vez que el array o sus elementos cambian.
 */
watch(
  registros,
  (nuevoValor) => {
    localStorage.setItem('hero-training-registros', JSON.stringify(nuevoValor))
  },
  { deep: true }
)

watch(
  registros,
  (nuevoValor) => {
    localStorage.setItem('hero-training-registros', JSON.stringify(nuevoValor))
  },
  { deep: true }
)

/**
 * Carga los registros guardados en localStorage al montar el componente.
 */
onMounted(() => {
  const guardados = localStorage.getItem('hero-training-registros')
  if (guardados) {
    try {
      registros.value = JSON.parse(guardados)
    } catch (e) {
      // Si hay un error al parsear, empezamos con array vacío
      registros.value = []
    }
  }
})

/**
 * Calcula el promedio de energía de todos los registros.
 */
const promedioEnergia = computed(() => {
  if (registros.value.length === 0) return 0
  const suma = registros.value.reduce((acc, r) => acc + r.energia, 0)
  return (suma / registros.value.length).toFixed(1)
})

/**
 * Obtiene el registro con la energía más alta.
 */
const diaMejor = computed(() => {
  if (registros.value.length === 0) return null
  return registros.value.reduce((mejor, actual) =>
    actual.energia > mejor.energia ? actual : mejor
  )
})

/**
 * Obtiene el registro con la energía más baja.
 */
const diaPeor = computed(() => {
  if (registros.value.length === 0) return null
  return registros.value.reduce((peor, actual) =>
    actual.energia < peor.energia ? actual : peor
  )
})

/**
 * Retorna los registros ordenados del más reciente al más antiguo.
 */
const registrosOrdenados = computed(() => {
  return [...registros.value].sort((a, b) => new Date(b.fecha) - new Date(a.fecha))
})

/**
 * Elimina un registro por su id.
 */
function eliminarRegistro(id) {
  registros.value = registros.value.filter(r => r.id !== id)
}

/**
 * Borra todos los registros con confirmación.
 */
function borrarTodo() {
  if (confirm('¿Estás seguro de que quieres borrar todos los registros?')) {
    registros.value = []
  }
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

.diary-resumen-card {
  background: rgba(17, 24, 39, 0.6);
  border: 1px solid #374151;
  border-radius: 12px;
}

.diary-resumen-title {
  color: #facc15;
  font-size: 1rem;
  font-weight: 700;
  margin-bottom: 0.75rem;
}

.diary-resumen-label {
  color: #fde047;
  font-size: 0.8rem;
  display: block;
}

.diary-resumen-value {
  color: #facc15;
  font-weight: 700;
  font-size: 1.1rem;
  margin: 0;
}

.diary-list-item {
  background: rgba(31, 41, 55, 0.5);
  border-color: #374151;
  color: #e5e7eb;
}

.diary-item-energia {
  color: #facc15;
  font-weight: 700;
}

.diary-item-fecha {
  color: #9ca3af;
  font-size: 0.8rem;
}

.diary-item-nota {
  color: #d1d5db;
  font-size: 0.9rem;
}

.diary-btn-delete {
  background: rgba(239, 68, 68, 0.2);
  border: 1px solid rgba(239, 68, 68, 0.5);
  color: #fca5a5;
}

.diary-btn-delete:hover {
  background: rgba(239, 68, 68, 0.4);
}
</style>