<template>
  <div class="md-example-child md-example-child-picker md-example-child-picker-1">
    <md-picker
      ref="picker"
      :data="pickerData"
      :cols="3"
      :default-index="pickerDefaultIndex"
      :default-value="pickerDefaultValue"
      is-view
      is-cascade
      @initialed="onPickerInitialed"
      @change="onPickerConfirm"
    ></md-picker>
  </div>
</template>

<script>import {Picker, Dialog} from 'mand-mobile'
import district from 'mand-mobile/components/picker/demo/data/district'

export default {
  name: 'picker-demo',
  /* DELETE */
  title:
    '联动数据 <a href="javascript:window.PickerTrigger3()">getColumnValue(0)</a><a href="javascript:window.PickerTrigger4()">getColumnIndex(0)</a><a href="javascript:window.PickerTrigger5()">changeDefaultValue</a>',
  titleEnUS:
    'Cascade <a href="javascript:window.PickerTrigger3()">getColumnValue(0)</a><a href="javascript:window.PickerTrigger4()">getColumnIndex(0)</a><a href="javascript:window.PickerTrigger5()">changeDefaultValue</a>',
  describe: '默认选中3, 2, 1项',
  describeEnUS: '"3, 2, 1" item selected by default',
  /* DELETE */
  components: {
    [Picker.name]: Picker,
  },
  data() {
    return {
      pickerData: [],
      pickerDefaultIndex: [],
      pickerDefaultValue: [],
      pickerValue: '',
    }
  },
  mounted() {
    this.pickerData = district
    this.pickerDefaultIndex = [3, 2, 1]

    window.PickerTrigger3 = () => {
      this.getColumnValue('picker', 0)
    }
    window.PickerTrigger4 = () => {
      this.getColumnIndex('picker', 0)
    }
    window.PickerTrigger5 = () => {
      this.pickerDefaultIndex = []
      this.pickerDefaultValue = ['110000', '110100', '110101']
      setTimeout(() => {
        this.$refs.picker.refresh()
      }, 0)
    }
  },
  methods: {
    onPickerInitialed() {
      const columnValues = this.$refs.picker.getColumnValues()
      let value = ''
      columnValues.forEach(item => {
        value += `${item.label}-`
      })
      console.log(`[Mand Mobile] Picker getColumnValues: ${value.substr(0, value.length - 1)}`)
    },
    onPickerConfirm(columnIndex, itemIndex, value) {
      if (value) {
        this.pickerValue = value.text
      }
    },
    getColumnValue(picker, index) {
      const value = this.$refs.picker.getColumnValue(index)
      delete value.children
      Dialog.alert({
        content: JSON.stringify(value),
      })
    },
    getColumnIndex(picker, index) {
      const value = this.$refs.picker.getColumnIndex(index)
      Dialog.alert({
        content: `<pre>${JSON.stringify(value)}</pre>`,
      })
    },
  },
}
</script>
