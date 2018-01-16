<template>
  <draggable :element="'tbody'" :list="data" :options="draggableO">
    <template v-if="data.length">
      <template v-for="(item, index) in data">
        <tr :key="index">
          <td v-if="shouldRenderSelection">
            <multi-select :selection="selection" :row="item" />
          </td>
          <td v-for="col in columns" :class="col.tdClass" :style="col.tdStyle">
            <!-- <td> component (tdComp) -->
            <component
              v-if="col.tdComp"
              :is="forDynCompIs(col.tdComp)"
              :row="item"
              :field="col.field"
              :value="item[col.field]"
              :index="index"
              v-bind="$props">
            </component>
            <template v-else>
              {{ item[col.field] }}
            </template>
          </td>
        </tr>
      </template>
    </template>
    <tr v-else-if="!leftFixed && !rightFixed">
      <td :colspan="colLen" class="text-center text-muted">
        ( {{ $i18nForDatatable('No Data') }} )
      </td>
    </tr>
  </draggable>
</template>
<script>
import MultiSelect from './MultiSelect.vue'
import props from '../_mixins/props'
import shouldRenderSelection from '../_mixins/shouldRenderSelection'
import draggable from 'vuedraggable'

export default {
  name: 'TableBody',
  components: { MultiSelect, draggable },
  mixins: [props, shouldRenderSelection],
  computed: {
    colLen () {
      return this.columns.length + !!this.selection
    },
    draggableO() {
        return {
            disabled: true, //disable by default
            ...this.draggableOptions
        }
    }
  }
}
</script>
