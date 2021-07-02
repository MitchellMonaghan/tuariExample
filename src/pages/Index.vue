<template>
  <q-page class="row items-center justify-evenly">
    <example-component
      @click="login"
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models'
import ExampleComponent from 'components/CompositionComponent.vue'
import { defineComponent, ref, onMounted } from 'vue'
import * as tauri from '@tauri-apps/api'

type Command = {
  commandName:string
  id:number
  params: string
}

export default defineComponent({
  name: 'PageIndex',
  components: { ExampleComponent },

  setup () {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: 'ct1'
      },
      {
        id: 2,
        content: 'ct2'
      },
      {
        id: 3,
        content: 'ct3'
      },
      {
        id: 4,
        content: 'ct4'
      },
      {
        id: 5,
        content: 'ct5'
      }
    ])
    const meta = ref<Meta>({
      totalCount: 1200
    })

    const loginCommand = async (username:string, password:string) => {
      return sendCommand({ commandName: 'login', id: 1, params: JSON.stringify([username, password]) })
    }

    const sendCommand = async (command: Command) => {
      return tauri.invoke('send_command', { command: JSON.stringify(command) })
    }

    const login = async () => {
      await loginCommand('admin', 'admin')
    }

    onMounted(async () => {
      // await login()
    })

    return { todos, meta, login }
  }
})
</script>
