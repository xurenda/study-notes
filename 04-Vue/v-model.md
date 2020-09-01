# v-model

## Vue2

在自定义组件上使用 `v-model`，多双向数据绑定

`A.vue`

```vue
<template>
  <div>
    <my-input v-model="val" :val2.sync="val2" />
    <p>{{ val }}</p>
  </div>
</template>

<script>
import MyInput from "MyInput.vue";

export default {
  components: { MyInput },
  data() {
    return {
      val: '',
      val2: ''
    }
  }
}
</script>
```

`MyInput.vue`

```vue
<template>
  <div>
    <input type="text" :value="value" @input="onInput" />
    <input type="text" :value="val2" @input="onVal2Input" />
  </div>
</template>

<script>
export default {
  name: 'my-input',
  // 固定为 "value"
  props: ['value', 'val2'],
  methods: {
    onInput(e) {
      // 固定为 "input"
      this.$emit('input', e.target.value)
    },
    onVal2Input(e) {
      // 固定为 "update:${val}"
      this.$emit('update:val2', e.target.value)
    }
  }
}
</script>
```

## Vue3

单双向绑定

```vue
<my-input v-model="val" />

props: ['modelValue']

$emit('update:modelValue', ...)
```

多双向绑定

```vue
<my-input v-model:val1="val" v-model:val2="val2" />

props: ['val1', 'val2']

$emit('update:val1', ...)
$emit('update:val2', ...)
```

