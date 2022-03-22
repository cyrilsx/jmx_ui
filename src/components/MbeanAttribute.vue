<template>
  <td>
    {{ this.desc }}:
  </td>
  <td>
    <input v-if="type === 'java.lang.String'" type="string" :disabled="!this.rw" v-model="current"/>
    <input v-if="type === 'boolean'" type="checkbox" :disabled="!this.rw" v-model="current">
    <input v-if="type === 'int'" type="number" :disabled="!this.rw" v-model="current">
    <input v-if="type === 'long'" type="number" :disabled="!this.rw" v-model="current">
  </td>
</template>

<script>
import axios from "axios";

export default {
  name: "MbeanAttribute",
  props: {
    rw: Boolean,
    type: String,
    desc: String,
    readUrl: String
  },
  data() {
    return {
      current: ''
    }
  },
  mounted() {
    axios.get(this.readUrl).then((result) => {
      console.info("res " + result.data)
      this.current = result.data.value
    });
  }

}
</script>

<style scoped>

</style>