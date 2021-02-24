<template>
  <div class="container d-flex flex-column justify-content-center">
    <Logo />
    <h1 class="d-flex justify-content-center">
      Cadastre-se
    </h1>
    <div>
      <NuxtLink to="/usuario/Cadastro" class="btn btn-outline-primary btn-block">
        Cadastrar
      </NuxtLink>

      <NuxtLink to="/usuario/Documento" class="btn btn-outline-primary btn-block">
        Ler Dados
      </NuxtLink>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>

</style>
