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
    <template v-slot:title>
      <img src="~/assets/img/veusinho.png" width="100" />
    </template>
    <tab-content title="Dados Pessoais" :before-change="validateFirstTab">
      <form-group-float-label v-model="model.name" label="Nome Completo" input-name="name" mandatory />
      <form-group-float-label v-model="model.cpf" label="CPF" input-name="cpf" mandatory />
    </tab-content>
    <tab-content title="Informações adicionais" :before-change="validateSecondTab">
      <form-group-float-label v-model="model.gender" label="Sexo" input-name="gender" mandatory />
      <form-group-float-label
        v-model="model.birthday"
        label="Data de Nascimento"
        input-name="birthday"
        input-type="date"
        mandatory
      />
    </tab-content>
    <tab-content title="Contato" :before-change="validateThirdTab">
      <form-group-float-label v-model="model.phone" label="Telefone" input-name="phone" mandatory />
      <form-group-float-label
        v-model="model.password"
        label="Senha"
        input-name="password"
        input-type="password"
        mandatory
      />
    </tab-content>
  </form-wizard>
</template>

<script>
export default {
  name: 'Cadastro',
  data () {
    return {
      model: {
        name: null,
        cpf: null,
        phone: null,
        birthday: null,
        gender: null,
        password: null
      }
    }
  },
  methods: {
    validate (firstField, secondField) {
      return new Promise((resolve, reject) => {
        if (this.model[firstField] && this.model[secondField]) {
          resolve(true)
        } else {
          this.reject('DEU RUIM')
        }
      })
    },
    validateFirstTab () {
      return this.validate('name', 'cpf')
    },
    validateSecondTab () {
      return this.validate('gender', 'birthday')
    },
    validateThirdTab () {
      return this.validate('phone', 'password')
    }
  },
  mounted () {
    // eslint-disable-next-line no-console
    console.log('this.$el.clientWidth', this.$el.clientWidth)
  }
}
</script>

<style lang="scss" scoped>
.label-float{
  margin-top: 40px;
  margin-bottom: 40px;
}
</style>
