<template>
  <va-card color="#172226">
    <va-card-title>Operation</va-card-title>
    <va-card-content>
      <div class="va-table-responsive">
        <table v-if="this.op" class="va-table">
          <thead>
          <td>Name</td>
          <td>Arguments</td>
          <td></td>
          </thead>
          <tr v-for="op in this.operations" :key="op">
            <td>{{ op.name }}</td>
            <mbean-operation :mbean="op.mbeanId" :name="op.name" :args="op.args" :desc="op.desc" :writeUrl=op.writeUrl />
          </tr>
        </table>
      </div>
    </va-card-content>
  </va-card>
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
        mbeanId: currentId,
        readUrl: "http://localhost:8080/actuator/jolokia/read/" + currentId + "/" + key,
        writeUrl: "http://localhost:8080/actuator/jolokia/exec/" + (currentId).toString('base64')
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