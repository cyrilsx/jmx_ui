<template>
  <va-card color="#172226">
    <va-card-title>Attribute</va-card-title>
    <va-card-content>
      <div class="va-table-responsive">
        <table v-if="this.attr" class="va-table">
          <thead>
          <td>Name</td>
          <td>Input</td>
          <td>Value</td>
          </thead>
          <tr v-for="attribute in this.attributes" :key="attribute">
            <td>{{ attribute.name }}</td>
            <mbean-attribute :type="attribute.type" :rw="attribute.rw" :desc="attribute.desc"
                             :readUrl=attribute.readUrl />
          </tr>
        </table>
      </div>
    </va-card-content>
  </va-card>
</template>

<script>
import MbeanAttribute from "@/components/MbeanAttribute";
import _ from "lodash";

export default {
  name: "MbeanAttributeList",
  props: ["attr", "id"],
  components: {MbeanAttribute},
  data() {
    return {
      attributes: [],
      current: ''
    }

  },
  mounted() {
    let currentId = this.id;
    this.attributes = _.transform(this.attr, function (result, value, key) {
      result.push({
        name: key,
        rw: value.rw,
        type: value.type,
        desc: value.desc,
        readUrl: "http://localhost:8080/actuator/jolokia/read/" + currentId + "/" + key
      })
    }, []);
    // this.current = axios.get('http://localhost:8080/actuator/jolokia/read/' + this.id + '/' + )
  }
}
</script>

<style scoped>
</style>