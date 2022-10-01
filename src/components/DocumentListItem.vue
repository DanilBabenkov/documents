<template>
  <ListItemLayout
    @remove="$emit('remove', item)"
    class="hide-item"
  >
    <div class="me-3 fs-08">
      {{ item.name }}
    </div>
    <div
      v-if="item.colors"
      class="me-2"
    >
      <Dots :colors="item.colors" />
    </div>
    <div
      v-if="item.required"
      class="me-3 fs-07 text-magenta d-flex align-items-center"
    >
      Обязательный
    </div>
    <div class="me-3 fs-07 text-grey d-flex flex-fill">
      {{ item.desc }}
    </div>
  </ListItemLayout>
</template>

<script>
import Dots from '../components/Dots'
import ListItemLayout from '../components/ListItemLayout'

export default {
  name: 'DocumentListItem',
  components: { ListItemLayout, Dots },
  props: {
    item: {
      type: Object,
      default: () => ({})
    }
  },
}
</script>
