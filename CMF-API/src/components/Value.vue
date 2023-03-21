<template>
  <v-card>
    <v-card-title>{{ title }}</v-card-title>
    <v-card-subtitle>
      {{ value.fecha.split("T")[0]}}</v-card-subtitle>
    <v-card-text>
      {{ value.valor }} UF 
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "Values",
  props: {
    value : {
      type: Object,
      required: true
    },
    title : {
      type: String,
      required: true
    }
  },
  computed: {
    maximo() {
      return Math.max.apply(
        Math,
        this.values.map(function (o) {
          return o.valor;
        })
      );
    },
  },
};
</script>