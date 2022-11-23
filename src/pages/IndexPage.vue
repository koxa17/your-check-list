<template>
  <q-page class="q-pt-md q-pr-sm q-pl-sm">
    <list-question/>
  </q-page>
</template>

<script>
import {computed, defineComponent, onMounted, ref} from 'vue'
import {getDataFromLocalStorage} from "assets/tools/script";
import {fasMinus} from '@quasar/extras/fontawesome-v6'
import ListQuestion from "components/ListQuestions.vue";

export default defineComponent({
  name: 'IndexPage',
  components: {ListQuestion},
  setup() {

    onMounted(() => {
      let authorLS = getDataFromLocalStorage("author")
      let showTimersLS = getDataFromLocalStorage("showTimers")

      if (authorLS) {
        author.value = authorLS
      }

      if (showTimersLS) {
        timers.value = showTimersLS
      }
    })

    const author = ref("Кристина Матусевич")
    const timers = ref(false)
    const currentDate = computed(() => new Date().toLocaleDateString())


    return {
      author,
      timers,
      currentDate,
    }

  }
})
</script>
<style lang="scss">


</style>
