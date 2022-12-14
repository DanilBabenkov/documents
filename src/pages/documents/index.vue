<template>
  <div>
    <div class="mt-4 d-flex justify-content-between">
      <div><h5>Документы</h5></div>
      <div class="d-flex">
        <Button>
          <template #icon>
            <BookmarkIcon class="btn-icon" />
          </template>
        </Button>
        <Button class="ms-2">
          <template #icon>
            <plus-icon class="btn-icon" />
          </template>
          Новый тип
        </Button>
        <Button class="ms-2">
          <template #icon>
            <plus-icon class="btn-icon" />
          </template>
          Новый документ
        </Button>
      </div>
    </div>
    <div class="mt-3 w-50">
      <SearchInput
        v-model="query"
        @clear="query = null"
      />
    </div>
    <div class="mt-3">
      <transition name="fade-in">
        <div
          v-if="isNothingFound"
          class="text-grey fs-09 mt-5"
        >
          Ничего не найдено. Попробуйте изменить параметры поиска.
        </div>
      </transition>
      <transition name="fade-in">
        <draggable
          :list="categories"
          handle=".handle"
          :group="{ name: 'section', pull: true }"
          ghost-class="ghost-item"
          chosen-class="chosen-item"
          drag-class="drag-item"
          animation="220"
          :remove-clone-on-hide="false"
          @start="onStart"
        >
          <SectionListItem
            v-for="category in computedCategories"
            :category="category"
            :query="query"
            :key="`category_${category.id}`"
            @remove="onRemoveSection"
          />
        </draggable>
      </transition>
    </div>
    <div class="mt-3">
      <SectionListItem
        v-for="category in itemsWithoutCategory"
        :category="category"
        :query="query"
        :key="`category_${category.id}`"
        no-category
        @remove="onRemoveSection"
        class="dropzone"
      />
    </div>
  </div>
</template>

<script>
import Button from '../../components/Button'
import BookmarkIcon from '../../components/icons/BookmarkIcon'
import SearchInput from '../../components/SearchInput'
import SectionListItem from '../../components/SectionListItem'
import PlusIcon from '../../components/icons/PlusIcon'
import draggable from 'vuedraggable'
import { mapState } from 'vuex'

export default {
  name: 'Documents',
  data () {
    return {
      query: null,
      dragging: false,
      itemsWithoutCategory: undefined
    }
  },
  components: {
    SectionListItem,
    SearchInput,
    Button,
    PlusIcon,
    BookmarkIcon,
    draggable
  },
  created () {
    this.getItemsWithoutCategory()
  },
  watch: {
    computedItems: 'getItemsWithoutCategory'
  },
  computed: {
    ...mapState(['items', 'categories']),
    isNothingFound () {
      return !this.computedItems.length && !this.search('categories').length
    },
    computedCategories () {
      return this.categories.map(category => {
        const items = this.computedItems.filter(item => item.categoryId === category.id)
        return {
          ...category,
          items
        }
      })
    },
    computedItems () {
      return this.search('items')
    }
  },
  methods: {
    onStart ({ clone }) {
      clone.classList.add('old-item')
    },
    onRemoveSection (removeItem) {
      const index = this.categories.findIndex(category => category.id === removeItem.id)
      if (index !== -1) {
        this.categories.splice(index, 1)
      }
    },
    getItemsWithoutCategory () {
      const items = this.computedItems.filter(item => !item.categoryId)
      this.itemsWithoutCategory = [{ items }]
    },
    search (entity) {
      if (!this.query) {
        return this[entity]
      }
      const condition = new RegExp(this.query, 'i')
      return this[entity].filter(item => condition.test(item.name))
    }
  }
}
</script>
