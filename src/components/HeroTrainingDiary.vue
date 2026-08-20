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
            <!-- Aquí irá el formulario y la lista -->
            <p class="text-center diary-status">
              Sistema listo. Registra tu primer día de entrenamiento.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Por ahora no hay lógica.
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

.diary-status {
  color: #fde047;
  font-size: 0.95rem;
}
</style>