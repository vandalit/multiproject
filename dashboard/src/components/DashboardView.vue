<script setup>
</script>


<template>
  <div class="home">

    <!-- Aquí añadiremos un componente que liste las regiones turísticas -->
  </div>
  <div class="container my-5">
        <h1 class="text-center mb-5">Mis Proyectos</h1>
        <div class="row">
            <div class="col-md-4 mb-4">
                <div class="card h-100">
                    <img src="https://placehold.it/400x300" class="card-img-top" alt="Proyecto 1">
                    <div class="card-body">
                        <h5 class="card-title">Proyecto 1</h5>
                        <p class="card-text">Una breve descripción del primer proyecto y sus características principales.</p>
                        <a href="https://multiprojectfirebase-main.web.app/" class="btn btn-primary"><i class="fas fa-info-circle me-2"></i>Más Información</a>
                    </div>
                </div>
            </div>
            <div class="col-md-4 mb-4">
                <div class="card h-100">
                    <img src="https://placehold.it/400x300" class="card-img-top" alt="Proyecto 2">
                    <div class="card-body">
                        <h5 class="card-title">Proyecto 2</h5>
                        <p class="card-text">Detalles sobre el segundo proyecto y lo que lo hace especial.</p>
                        <a href="https://multiprojectfirebase-second.web.app/" class="btn btn-primary"><i class="fas fa-code me-2"></i>Ver Código</a>
                    </div>
                </div>
            </div>
            <div class="col-md-4 mb-4">
                <div class="card h-100">
                    <img src="https://placehold.it/400x300" class="card-img-top" alt="Proyecto 3">
                    <div class="card-body">
                        <h5 class="card-title">Proyecto 3</h5>
                        <p class="card-text">Explicación del tercer proyecto y sus logros destacados.</p>
                        <a href="https://multiprojectfirebase-third.web.app/" class="btn btn-primary"><i class="fas fa-external-link-alt me-2"></i>Visitar Sitio</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "HomeView",
};
</script>

<style scoped>
.home {
  padding: 20px;
  text-align: center;
}
</style>