<template>
  <span>
    <span v-for="arg in this.args" :key="arg.name">
      {{arg.name}}
    <input type="string"/>
<!--    <input v-if="arg.type=== 'boolean'" type="checkbox">-->
<!--    <input v-if="arg.type === 'int'" type="number">-->
<!--    <input v-if="arg.type === 'long'" type="number">-->
    </span>
    <small>{{ this.desc }}</small>
    <button>call</button>
  </span>
</template>

<script>
// import axios from "axios";

export default {
  name: "MbeanOperation",
  props: {
    args: Array,
    ret: String,
    desc: String,
    readUrl: String
  },
  data() {
    return {
      current: ''
    }
  },
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