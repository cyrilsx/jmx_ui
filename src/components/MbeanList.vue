<template>


  <div style="position: relative;">
    <va-app-bar>
      <va-input class="ma-1 fixed-500px" v-model="jmxServer.url" placeholder="url to jolokia" />
      <va-button icon="search" @click="retrieveMBeans" color="#fff" flat :rounded="false" />
      <va-spacer />
      <va-button  color="#fff" flat :rounded="false">Login</va-button>
    </va-app-bar>
  </div>


  <section :key="mbean.id" v-for="mbean in mbeans" id="{{ mbean.id }}">
  <va-collapse :header="mbean.id" v-model="collaspe[mbean.index]">
    <h2>{{ mbean.id }}</h2>
    <div :key="bean.name" v-for="bean in mbean.value">
    <h3>{{ bean.name }}: {{ bean.desc }}</h3>
    <mbean-attribute-list v-if="bean.attr" :attr="bean.attr" :id="mbean.id + ':' + bean.name"/>
    <mbean-operation-list v-if="bean.op" :op="bean.op" :id="mbean.id + ':' + bean.name"/>
    </div>
  </va-collapse>
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
      collaspe: [],
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
      let index = 0;
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
          index: index++,
          id: key,
          value: flatValue
        });
      }, []);
    }
  }
}
</script>

<style scoped>
.fixed-500px {
  width: 500px;
}
</style>