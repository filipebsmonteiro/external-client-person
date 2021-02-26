<template>
  <form-wizard
    class="container"
    back-button-text="Anterior"
    next-button-text="Próximo"
    finish-button-text="Salvar"
    color="#f9b112"
    error-color="#e74a3b"
    @on-complete="() => {alert('COMPLETE')}"
  >
    <template #title>
      <img src="~/assets/img/veusinho.png" width="100" alt="">
    </template>
    <tab-content title="Dados Pessoais" :before-change="validateFirstTab">
      <form-group-float-label v-model="model.cpf" label="CPF" input-name="cpf" mandatory />
      <form-group-float-label v-model="model.name" label="Nome Completo" input-name="name" mandatory />
    </tab-content>
    <tab-content title="Contato" :before-change="validateThirdTab">
      <form-group-float-label v-model="model.email" label="Email" input-name="email" mandatory />
      <form-group-float-label v-model="model.phone" label="Telefone" input-name="phone" mandatory />
      <form-group-float-label
        v-model="model.CEP"
        label="CEP"
        input-name="CEP"
        mandatory
      />
    </tab-content>
    <tab-content title="Informações adicionais" :before-change="validateSecondTab">
      <h3>Confirme seus dados</h3>
      <form-group-float-label v-model="model.gender" label="Sexo" input-name="gender" mandatory />
      <form-group-float-label
        v-model="model.birthday"
        label="Data de Nascimento"
        input-name="birthday"
        input-type="date"
        mandatory
      />
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
        name: null,
        email: null,
        cpf: null,
        phone: null,
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
        const errorFields = Object.keys(fieldsToValidate).map((field) => {
          console.log(field)
          console.log(this.model[field])
          console.log(!this.model[field])
          if (!this.model[field]) {
            this.$snotify.error(`Campo ${fieldsToValidate[field]} obrigatório!`)
          }
          return null
        })

        if (errorFields.length > 0) {
          this.reject('Campos Inválidos')
        }

        resolve(true)
      })
    },
    validateFirstTab () {
      // eslint-disable-next-line no-console
      // console.log(this.validate('name', 'cpf'))
      return this.validate({
        name: 'Nome',
        cpf: 'CPF'
      })
    },
    validateSecondTab () {
      // return this.validate('gender', 'birthday')
      // return this.validate('email', 'phone', 'CEP')
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
</style>
