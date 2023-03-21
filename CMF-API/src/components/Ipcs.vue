<template>
  <v-card width="300px">
    <v-table density="compact" v-if="ipcs.length != 0">
      <thead>
        <tr>
          <th class="text-left">Mes</th>
          <th class="text-left">Valor</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ipc in ipcs" :key="ipc.mes">
          <td>{{ ipc.mes }}</td>
          <td>
            <div v-if="ipc.IPC > 0" style="color: green">
              <v-icon size="small" color="green" icon="mdi-arrow-up"></v-icon>
              + {{ ipc.IPC }} %
            </div>

            <div v-else-if="ipc.IPC < 0" style="color: red">
              <v-icon size="small" color="red" icon="mdi-arrow-down"></v-icon>
              - {{ ipc.IPC * -1 }} %
            </div>

            <div v-else style="color: gray">
              <v-icon size="small" color="grey" icon="mdi-arrow-right"></v-icon>
              {{ ipc.IPC }} %
            </div>
          </td>
        </tr>
      </tbody>
    </v-table>
  </v-card>
</template>

<script>
export default {
  name: "Ipcs",
  props: {
    ipcs: {
      type: Array,
      required: true,
    },
  },
};
</script>
