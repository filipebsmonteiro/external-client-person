<template>
  <b-card :class="'border-left-' + variant">
    <div class="top">
      <slot name="top" />
    </div>
    <div class="row no-gutters align-items-center">
      <div class="col mr-2">
        <slot>
          <div :class="'text-xs font-weight-bold text-uppercase mb-1 text-' + variant">
            {{ text }}
          </div>
          <div class="h5 mb-0 font-weight-bold text-gray-800">
            {{ value }} <small>{{ valueDescription }}</small>
          </div>
        </slot>
      </div>
      <div class="col-auto">
        <slot name="icon">
          <i :class="'fas fa-' + icon + ' fa-2x text-' + variant"/>
        </slot>
      </div>
    </div>
  </b-card>
</template>

<script>
export default {
  name: 'LittleCard',
  props: {
    variant: {
      type: String,
      default: 'primary'
    },
    text: {
      type: String,
      default: null
    },
    value: {
      // eslint-disable-next-line vue/require-prop-type-constructor
      type: String | Number,
      default: null
    },
    valueDescription: {
      type: String,
      default: ''
    },
    icon: {
      type: String,
      default: 'cogs'
    }
  }
}
</script>
