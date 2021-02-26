<template>
  <form-wizard
    class="container"
    back-button-text="Anterior"
    next-button-text="Próximo"
    finish-button-text="Enviar"
    color="#f9b112"
    error-color="#e74a3b"
    @on-complete="() => {alert('COMPLETE')}"
  >
    <template #title>
      <div class="d-flex jusfity-content-between align-items-center">
        <img src="~/assets/img/veusinho.png" alt="" class="logo">
        <h2 class="flex-fill text-gray">
          Cadastro
        </h2>
        <NuxtLink to="/" class="btn btn-outline-warning">
          <b-icon icon="arrow-left" />
        </NuxtLink>
      </div>
    </template>

    <tab-content title="Verificação" :before-change="validateFirstTab">
      <form-group-float-label v-model="model.cpf" label="CPF" input-name="cpf" mandatory />
    </tab-content>
    <tab-content title="Dados Pessoais" :before-change="validateSecondTab">
      <form-group-float-label v-model="model.name" label="Nome Completo" input-name="name" mandatory />
      <form-group-float-label v-model="model.email" label="Email" input-name="email" mandatory />
      <form-group-float-label v-model="model.phone" label="Telefone" input-name="phone" mandatory />
      <form-group-float-label v-model="model.gender" label="Sexo" input-name="gender" mandatory />
      <form-group-float-label
        v-model="model.birthday"
        label="Data de Nascimento"
        input-name="birthday"
        input-type="date"
        mandatory
      />
      <form-group-float-label v-model="model.CEP" label="CEP" input-name="CEP" mandatory />
    </tab-content>
    <tab-content title="Documentos" :before-change="validateThirdTab">
      <b-form-group label="Pedido(s) de exame">
        <b-form-file
          placeholder="Escolha arquivo ou Solte aqui"
          drop-placeholder="Solte aqui"
          accept="image/x-png,image/gif,image/jpeg"
        />
      </b-form-group>
      <b-form-group label="Carteirinha do plano de saúde" class="mt-5">
        <b-form-file
          placeholder="Escolha arquivo ou Solte aqui"
          drop-placeholder="Solte aqui"
          accept="image/x-png,image/gif,image/jpeg"
        />
      </b-form-group>
      <b-form-group label="Identidade" class="mt-5">
        <b-form-file
          placeholder="Escolha arquivo ou Solte aqui"
          drop-placeholder="Solte aqui"
          accept="image/x-png,image/gif,image/jpeg"
        />
      </b-form-group>
    </tab-content>
  </form-wizard>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Cadastro',
  data () {
    return {
      model: {
        cpf: null,
        name: null,
        email: null,
        phone: null,
        gender: null,
        birthday: null,
        CEP: null
      }
    }
  },
  mounted () {
    // eslint-disable-next-line no-console
    console.log('this.$el.clientWidth', this.$el.clientWidth)
  },
  methods: {
    validate (fieldsToValidate) {
      return new Promise((resolve, reject) => {
        const errorFields = Object.keys(fieldsToValidate).filter(field => !this.model[field])

        if (errorFields.length > 0) {
          errorFields.map(field => this.$snotify.error(`Campo ${fieldsToValidate[field]} obrigatório!`))
          this.reject('Campos Inválidos')
        }

        resolve(true)
      })
    },
    validateFirstTab () {
      return this.validate({ cpf: 'CPF' })
    },
    validateSecondTab () {
      return this.validate({
        CEP: 'CEP',
        birthday: 'Data de Nascimento',
        gender: 'Sexo',
        phone: 'Telefone',
        email: 'Email',
        name: 'Nome'
      })
    },
    validateThirdTab () {
      // return this.validate('phone', 'password')
    },
    procuraCep () {
      // return
      // Nova variável "cep" somente com dígitos.
      // eslint-disable-next-line no-unreachable
      const cep = this.model.CEP.replace(/\D/g, '')

      // Verifica se campo cep possui valor informado.
      if (cep.length > 7) {
        // Expressão regular para validar o CEP.
        const validacep = /^[0-9]{8}$/

        // Valida o formato do CEP.
        if (validacep.test(cep)) {
          // Preenche os campos com "..." enquanto consulta webservice.
          this.model.Logradouro = '...'
          this.model.Bairro = '...'
          this.model.Cidade = '...'

          // Consulta o webservice viacep.com.br/
          axios.get('//viacep.com.br/ws/' + cep + '/json/?callback=').then((dados) => {
            if (!dados.erro) {
              const end = dados.data
              // Atualiza os campos com os valores da consulta.
              this.model.Logradouro = `${end.logradouro} ${end.localidade} ${end.complemento}`
              this.model.Bairro = end.bairro
              this.model.Cidade = end.cidade
            }
          }).catch(() => {
            this.model.Logradouro = ''
            this.model.Bairro = ''
            this.model.Cidade = ''
          })
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.label-float{
  margin-top: 40px;
  margin-bottom: 40px;
}

@keyframes slideInFromLeft {
  0% {
    transform: translateX(160%) scale(2) translateY(25%);// translate(+50%);
  }
  100% {
    transform: translateX(0);// translate(+25%, +25%) scale(1);
  }
}

::v-deep.logo {
  width: 50px;
  animation: 1s ease-out 0s 1 slideInFromLeft;
}
</style>
