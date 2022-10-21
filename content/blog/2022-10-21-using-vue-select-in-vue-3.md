+++
title = "Using Vue Select in Vue 3"
[taxonomies]
 tags = [ "programming", "frontend", "vue" ]
+++

Just a reminder for me to use [vue select](https://github.com/sagalbot/vue-select) for Vue 3 (version ^4.0.0-beta.3).

```html
<v-select
  :options="[{ hello: 'world' }]"
  :getOptionLabel="(e: any) => e.world"
  @update:modelValue="handleChangeUser"
  :value="{ hello: 'world' }"
/>
```

since docs is still scarce.
