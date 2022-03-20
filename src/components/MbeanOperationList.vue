<template>
  <h4>Operation</h4>
  <table v-if="this.op">
    <thead>
    <td>Name</td>
    <td>Input</td>
    </thead>
    <tr v-for="op in this.operations" :key="op">
      <td>{{ op.name }}</td>
      <td>
        <mbean-operation :args="op.args" :desc="op.desc" :readUrl=op.readUrl />
      </td>
    </tr>
  </table>
</template>

<script>
import _ from "lodash";
import MbeanOperation from "@/components/MbeanOperation";

export default {
  name: "MbeanOperationList",
  props: ["op", "id"],
  components: {MbeanOperation},
  data() {
    return {
      operations: [],
    }

  },
  mounted() {
    let currentId = this.id;
    this.operations = _.transform(this.op, function (result, value, key) {
      result.push({
        name: key,
        args: value.args,
        ret: value.ret,
        desc: value.desc,
        readUrl: "http://localhost:8080/actuator/jolokia/read/" + currentId + "/" + key
      })
    }, []);
  }
}
</script>

<style scoped>
table {
  border: 1px aliceblue;
  position: center;
  text-align: center;
}
</style>