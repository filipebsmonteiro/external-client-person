<template>
  <form class="container form-steps" @submit="evt => evt.preventDefault()">
    <div class="steps">
      <div
        v-for="(step, index) in steps"
        v-show="index === stepsControl.index"
        :key="index"
        class="step"
      >
        <form-group-float-label
          v-model="step.model"
          :label="step.label"
          :input-name="step.name"
          :input-type="step.type ? step.type : 'text'"
          :mandatory="step.mandatory ? step.mandatory: false"
          @keyup="evt => { if (step.model){buttonBottom.disabled = false} }"
          @change="evt => {step.model = evt; if (step.model){buttonBottom.disabled = false} }"
        />
        {{ step.model }}
      </div>
      <b-button
        class="b-radius-0 mb-2 p-3"
        :type="buttonBottom.type"
        :variant="buttonBottom.variant"
        :disabled="buttonBottom.disabled"
        block
        @click="goToStep(stepsControl.index+1)"
      >
        {{ buttonBottom.text }}
      </b-button>
    </div>
    {{ stepsControl.index }}

  </form>
</template>

<script>
export default {
  name: 'FormSteps',
  props: {
    steps: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      stepsControl: {
        index: 0
      },
      buttonBottom: {
        type: 'button',
        variant: 'primary',
        text: 'Próximo',
        disabled: false
      }
    }
  },
  methods: {
    goToStep (step) {
      const steps = this.$el.querySelectorAll('.form-steps > .steps > .step')
      const qtd = Object.keys(steps).length - 1

      // Verify the last
      if (this.stepsControl.index === qtd) {
        alert('UEPAAAA')
        return
      }

      // Verify if next is the Last
      if (step === qtd) {
        this.buttonBottom.type = 'submit'
        this.buttonBottom.text = 'Salvar'
        this.buttonBottom.variant = 'success'
      }

      this.stepsControl.index = step
      this.buttonBottom.disabled = true
    }
  }
}
</script>

<style lang="scss" scoped>
.label-float {
  margin-top: 40px;
  margin-bottom: 40px;
}

.form-steps {
  min-height: 100vh;
  display: flex;
  flex-direction: column;

  .steps {
    height: 100%;
    .step {
      ::v-deep.label-float {
        flex: 1 1 auto;
      }
    }
    //justify-content: space-between;
  }
}
</style>
