<template>
  <div class="container d-flex flex-column align-items-center mt-3">
    <img src="~/assets/img/veusinho.png" width="200" alt="">
    <h2 class="d-flex justify-content-center text-center mt-5 mb-5 text-gray">
      Bem-vindo ao <br>
      Veus Auto-Atendimento
    </h2>
    <div class="mt-3 w-100">
      <NuxtLink to="/usuario/Cadastro" class="btn btn-outline-warning btn-block">
        Cadastrar
      </NuxtLink>
<!--      <canvas id="canvas" />-->
<!--      <span id="text" />-->
<!--      <img id="image" alt="">-->

      <qrcode-vue class="qr-code" :size="500" :value="'https://github.com/l-ll/qrcode-vue'" />

      <!--<NuxtLink to="/usuario/Documento" class="btn btn-outline-primary btn-block">-->
      <!--Ler Dados-->
      <!--</NuxtLink>-->
    </div>
  </div>
</template>

<script>
import qrcodeVue from 'qrcode-vue'

export default {
  name: 'Index',
  components: { qrcodeVue },
  mounted () {
    // QRCode.toCanvas(document.getElementById('canvas'), 'https://www.youtube.com/watch?v=UcV711pXbhM&ab_channel=WendellCarvalho')

    // QRCode.toString(window.location.href, function (err, string) {
    //   if (err) { throw err }
    //   document.getElementById('text').innerHTML = string
    // })

    // eslint-disable-next-line no-console
    // console.log(
    //   brandedQRCode.generate({
    //     text: 'https://www.google.com',
    //     path: '/assets/img/veusinho.png'
    //   })
    // )

  }
}
</script>

<style lang="scss" scoped>
::v-deep.qr-code {
  padding: 20px;
  border: #f9b112 3px solid;
  border-radius: 10px;
  text-align: center;

  img {
    width: 75%;
    height:auto;
  }
  canvas {
    width: 75%;
  }
}
</style>
