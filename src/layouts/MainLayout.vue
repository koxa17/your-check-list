<template>

  <q-layout view="lHh lpR fFf">

    <q-header elevated class="bg-primary text-white flex">
      <q-toolbar shrink>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer"/>

        <q-toolbar-title class="q-px-lg q-mb-md" clickable>
          <div class="text-h4 logo flex flex-inline items-baseline">

            <div class="logo-text q-mr-sm"  >
              Check-List
            </div>

            <q-img
              src="../assets/images/logo.png"
              :no-native-menu="true"
              width="50px"
              class="logo-img q-mr-sm"
            />
          </div>
        </q-toolbar-title>

        <q-toggle
          v-model="darkMode"
          :icon="iconThemeToggle"
          color="dark"
        />

        <q-btn
          no-caps
          flat
          :dark-percentage="true"
          :label="`${version} Что нового?`"
          class="q-mr-sm"
        />

        <q-btn flat round dense icon="gamepad"/>

      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="leftDrawerOpen" side="left" elevated>

      <template v-for="(groups, index) in essentialLinks" :key="groups.group + '__' + index">
        <q-list>
          <q-item-label
            header
          >
            {{ groups.group }}
          </q-item-label>


          <EssentialLink v-for="(link) in groups.links" :key="link.title"
                         v-bind="link"
                         :title="link.title"
          />
        </q-list>

        <q-separator v-if="index !== essentialLinks.length - 1"/>
      </template>

    </q-drawer>

    <q-page-container>
      <router-view/>
    </q-page-container>

  </q-layout>
</template>

<script>
import {computed, defineComponent, ref, watch} from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import {useStore} from 'vuex'
import {fabVk, fasMoon, fasSun} from '@quasar/extras/fontawesome-v6'
import {useQuasar} from "quasar";
import {getDataFromLocalStorage, saveDataToLocalStorage} from "assets/tools/script";

const linksList = [
  {
    group: "Связь со мной",
    links: [
      {
        title: 'Телеграмм',
        caption: "@Koxa17",
        icon: 'telegram',
        color: 'primary',
        link: 'https://t.me/Koxa17'
      },
      {
        title: 'Вконтакте',
        caption: "id71204174",
        icon: fabVk,
        color: 'primary',
        link: 'https://vk.com/id71204174'
      },
      {
        title: 'Нашли ошибку?',
        caption: "Напишите мне",
        icon: 'error',
        color: 'warning',
        link: 'https://vk.com/id71204174'
      },
    ]
  },
]

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup() {
    const store = useStore()
    const leftDrawerOpen = ref(false)
    const version = store.getters.getVersionMyApp
    const $q = useQuasar()
    const darkMode = ref(false)
    const iconThemeToggle = computed(() => darkMode.value ? fasMoon : fasSun)
    $q.dark.set(darkMode.value = getDataFromLocalStorage('isDark') ? getDataFromLocalStorage('isDark') : false)


    watch(() => darkMode.value, val => {
      $q.dark.set(val)
      saveDataToLocalStorage('isDark', val)
    })

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      version,
      darkMode,
      iconThemeToggle,
    }
  }
})
</script>

<style lang="scss">


</style>
