<script>
  import { isNaN } from '../utils'
  import {defineComponent} from 'vue';

  function stringifyValue(value) {
    if (typeof value === 'string') return value
    // istanbul ignore else
    if (value != null && !isNaN(value)) return JSON.stringify(value)
    // istanbul ignore next
    return ''
  }

  export default defineComponent({
    name: 'vue-treeselect--hidden-fields',
    inject: [ 'instance' ],
    functional: true,

    render(context) {
      const instance = context.instance

      if (!instance) return null;
      if (!("name" in instance)) return null;

      if (!instance.name || instance.disabled || !instance.hasValue) return null

      let stringifiedValues = instance.internalValue.map(stringifyValue)

      if (instance.multiple && instance.joinValues) stringifiedValues = [
        stringifiedValues.join(instance.delimiter),
      ]

      return stringifiedValues.map((stringifiedValue, i) => (
        <input type="hidden"
          name={instance.name}
          value={stringifiedValue}
          key={'hidden-field-' + i}
        />
      ))
    },
  })
</script>
