<template>
  <div class="border-grey ps-3 pe-1 ">
    <div class="hide-item d-flex align-items-center justify-content-between">
      <div v-if="$slots.toggle" class="me-2">
        <slot name="toggle" />
      </div>
      <div class="d-flex align-items-center text-truncate flex-fill overlay">
        <slot />
      </div>
      <div class="d-flex align-items-center ms-4">
        <div class="me-1">
          <Button type="icon">
            <pencil-icon class="action-icon" />
          </Button>
        </div>
        <div class="me-1">
          <Button
            type="icon"
            @click="$emit('remove')"
          >
            <TrashBinIcon class="action-icon-magenta" />
          </Button>
        </div>
        <div class="handle">
          <Button type="icon">
            <UpDownArrowIcon class="action-icon" />
          </Button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PencilIcon from '../components/icons/PencilIcon'
import TrashBinIcon from '../components/icons/TrashBinIcon'
import UpDownArrowIcon from '../components/icons/UpDownArrowIcon'
import Button from '../components/Button'

export default {
  name: 'ListItemLayout',
  components: { UpDownArrowIcon, TrashBinIcon, PencilIcon, Button }
}
</script>
