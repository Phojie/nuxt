<script setup lang="ts">
const state = useState(() => shallowRef({} as Record<string, any>))

if (process.server) {
  const r = ref('')
  state.value.ref = r
  state.value.shallowReactive = shallowReactive({ nested: { ref: r } })
  state.value.shallowRef = shallowRef(false)
  state.value.reactive = reactive({ ref: r })
  state.value.error = createError({ message: 'error' })
  state.value.date = new Date()
}
</script>

<template>
  <div>
    <pre>{{ state }}</pre>
    Date: {{ state.date instanceof Date }} <br>
    Error: {{ isNuxtError(state.error) }} <hr>
    Shallow reactive: {{ isReactive(state.shallowReactive) && isShallow(state.shallowReactive) }} <br>
    Shallow ref: {{ isShallow(state.shallowRef) }} <br>
    Reactive: {{ isReactive(state.reactive) }} <br>
    Ref: {{ isRef(state.ref) }} <hr>
    Recursive objects: {{ state.ref === state.shallowReactive.nested.ref }} <br>
  </div>
</template>
