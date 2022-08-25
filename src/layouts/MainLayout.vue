<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="openThaDrawer"
        />

        <q-toolbar-title>
          EkoHub
        </q-toolbar-title>

        <div>what???</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="room in allRooms"
          :key="room.name"
          v-bind="room"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'
import EssentialLink from 'components/EssentialLink.vue'
import sdk, {EventType} from 'matrix-js-sdk';
//import 'matrix-js-sdk/lib/@types/event.js';

const client = sdk.createClient({
    baseUrl: "https://matrix.org",
    accessToken: "syt_b2JzZXJ2ZXJ6ZXJv_qbsSNMdGRZpXMsNvXJeX_3OODDh",
    userId: "@observerzero:matrix.org"
});

client.startClient();

function printARoom(item, index, arr){
  //console.log(arr[index])
  console.log(arr[index].name)
  console.log(arr[index].isSpaceRoom())
  const isSpace = arr[index].isSpaceRoom()
  if (isSpace === true) {
    const thatRoom = arr[index].currentState.getStateEvents(EventType.SpaceChild)
    console.log(thatRoom)
  }
}

async function getThaRooms() {
  const rooms = await client.getRooms();
  rooms.forEach(printARoom)
  return rooms
};
const allRooms = getThaRooms()


const linksList = [
  {
    title: "Quasar Dev",
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev'
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  }
]

export default defineComponent({
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  setup: function () {
    const leftDrawerOpen = ref(false)

    return {
      essentialLinks: linksList,
      leftDrawerOpen,
      allRooms: allRooms,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      async doThaRooms() {
        await getThaRooms()
      },
      async openThaDrawer() {
        getThaRooms()
      }
    }
  }
})
</script>
