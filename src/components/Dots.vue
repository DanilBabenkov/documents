<template>
  <div class="d-flex align-items-center">
    <template v-for="(color, index) in colors">
      <div
        :key="`color_${index}`"
        style="margin-right: .4rem"
        class="d-flex align-items-center"
      >
        <circle-icon :fill="color" />
      </div>
    </template>
  </div>
</template>

<script>
import CircleIcon from '../components/icons/CircleIcon'

export default {

  name: 'Dots',
  components: { CircleIcon },
  props: {
    colors: {
      type: Array,
      default: () => ([])
    }
  }
}
</script>
