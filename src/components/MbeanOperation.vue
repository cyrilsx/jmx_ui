<template>
  <td>
    <span v-for="(arg, index) in this.args" :key="arg.name">
      {{ arg.name }}
    <input type="string" v-model="valuesArgs[index]"/>
      <!--    <input v-if="arg.type=== 'boolean'" type="checkbox">-->
      <!--    <input v-if="arg.type === 'int'" type="number">-->
      <!--    <input v-if="arg.type === 'long'" type="number">-->
    </span>
    <small>{{ this.desc }}</small>
  </td>
  <td>
    <va-button @click="call">Call</va-button>
  </td>
</template>

<script>
import axios from "axios";

export default {
  name: "MbeanOperation",
  props: {
    name: String,
    mbean: String,
    writeUrl: String,
    args: Array,
    ret: String,
    desc: String,
    readUrl: String
  },
  data() {
    return {
      current: '',
      valuesArgs: []
    }
  },
  methods: {
    call: function () {
      console.info("args: " + JSON.stringify(this.valuesArgs));
      axios.post(this.writeUrl, {
          "type":"EXEC",
          "mbean":this.mbean,
          "operation":this.name,
          "arguments":this.valuesArgs
      }).then((response) => {
        alert(JSON.stringify(response.value));
      }).catch((error) => {
        console.debug(error);
      });
    }
  }
  /*mounted() {
    axios.get(this.readUrl).then((result) => {
      console.info("res " + result.data)
      this.current = result.data.value
    });
  }*/

}
</script>

<style scoped>

</style>