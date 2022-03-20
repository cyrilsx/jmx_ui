<template>

  <section>
    <input v-model="jmxServer.url" placeholder="url to jolokia"/>
    <input type="button" @click="retrieveMBeans" value="Save Information" class="btn btn-block"/>
  </section>

  <section :key="mbean.id" v-for="mbean in mbeans" id="{{ mbean.id }}">
    <h2>{{ mbean.id }}</h2>
    <div :key="bean.name" v-for="bean in mbean.value">
    <h3>{{ bean.name }}: {{ bean.desc }}</h3>
    <mbean-attribute-list v-if="bean.attr" :attr="bean.attr" :id="mbean.id + ':' + bean.name"/>
    <mbean-operation-list v-if="bean.op" :op="bean.op" :id="mbean.id + ':' + bean.name"/>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
import _ from 'lodash';
import MbeanAttributeList from "@/components/MbeanAttributeList";
import MbeanOperationList from "@/components/MbeanOperationList";

export default {
  name: "MbeanList",
  components: {MbeanOperationList, MbeanAttributeList},
  data() {
    return {
      jmxServer: {
        url: 'http:localhost:8080/actuator/jolokia'
      },
      mbeans: []
    }

  },
  methods: {
    retrieveMBeans: function () {
      axios.get('http://localhost:8080/actuator/jolokia/list')
          .then((response) => {
            this.mbeans = this.mapToList(response.data.value);
          }).catch((error) => {
        console.debug(error);
        });
    },
    mapToList: function (rawResult) {
      console.info("raw" + rawResult);
      return _.transform(rawResult, function (result, value, key) {
        let flatValue = _.transform(value, function (result, value, key) {
          result.push({
            name: key,
            desc: value.desc,
            op: value.op,
            attr: value.attr,
            class: value.class
          })
        }, []);
        result.push({
          id: key,
          value: flatValue
        });
      }, []);
    }
  }
}
</script>

<style scoped>

</style>