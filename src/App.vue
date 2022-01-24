<template>
<div style="background-color:black;padding:1em;" class="padding:0,0,0,0">
  <img alt="Vue logo" src="https://uslbk2chvod1qzfba2bdeol1-wpengine.netdna-ssl.com/wp-content/uploads/2021/06/fsl-logo.png" />
  <h1 style="color:white;">Tech Radar Maker</h1>
</div>
<button :click="saveRadar">Save Radar</button>
  <div class="myform">
    <json-forms
      :data="data"
      :renderers="renderers"
      :schema="schema"
      :uischema="uischema"
      @change="onChange"
    />
  </div>
  <div>
    {{data}}
  </div>
    <div><a href="https://radar.thoughtworks.com/">Paste contents of 'technologies' array here.</a></div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { JsonForms, JsonFormsChangeEvent } from "@jsonforms/vue";
import {
  defaultStyles,
  mergeStyles,
  vanillaRenderers,
} from "@jsonforms/vue-vanilla";

// mergeStyles combines all classes from both styles definitions into one
const myStyles = mergeStyles(defaultStyles, { control: { label: "mylabel" } });

const renderers = [
  ...vanillaRenderers,
  // here you can add custom renderers
];

const schema = {
  properties: {
    technologies:{
      type:"array",
      items:{
        type:"object",
        properties:{
          name: {
            title:"Technology Name",
            type: "string",
            minLength: 1,
          },
          ring: {
            type:"string",
            enum: ["Assess", "Trial", "Adopt", "Hold"]
          },
          quadrant: {
            type:"string",
            enum: ["Platform", "Library/Framework", "Tool", "Hold"]
          },
          isNew: {
            type: "boolean",
          },
          description: {
            type: "string",
            maximum: 5,
          },
        },
      },
    },
  },
};

const uischema = {
  "type": "VerticalLayout",
  "elements": [
    {
      "type": "Control",
      "scope": "#/properties/technologies",
      "options": {
        "elementLabelProp": "name",
        "detail": {
          "type": "VerticalLayout",
          "elements": [
            {
          type: "Control",
          scope: "#/properties/name",
          },
          {
            type: "Control",
            scope: "#/properties/ring",
          },
          {
            type: "Control",
            scope: "#/properties/quadrant",
          },
          {
            type: "Control",
            scope: "#/properties/isNew",
          },
          {
            type: "Control",
            scope: "#/properties/description",
          },
            ]
          }
      }
    }
  ]
}

export default defineComponent({
  name: "App",
  components: {
    JsonForms,
  },
  data() {
    return {
      // freeze renderers for performance gains
      renderers: Object.freeze(renderers),
      data: {
        technologies:[
          {
            name:"test"
          }
        ]
      },
      schema,
      uischema,
    };
  },
  methods: {
    onChange(event: JsonFormsChangeEvent) {
      this.data = event.data;
    },
    saveRadar() {
    // not implemented
    }
  },
  provide() {
    return {
      styles: myStyles,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 120px;
  margin-right: 120px;
}

.mylabel {
  color: darkslategrey;
}

.vertical-layout {
  margin-left: 10px;
  margin-right: 10px;
}

.myform {
  width: 640px;
  margin: 0 auto;
}

.text-area {
  min-height: 80px;
}
</style>
