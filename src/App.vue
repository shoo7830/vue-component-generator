<template lang="pug">
  #app
    div.header
    //- Left (Generator)
    div.row
      div.generator.col-md-6
        h1 Vue Component Generator
        div.options
          div.card
            label.label-header Template
            select(v-model="newComponent.selectedTemplate")
              option(v-for="option in options.template" v-bind:value="option") {{option}}
          div.card
            label.label-header Script
            select(v-model="newComponent.selectedScript")
              option(v-for="option in options.scripts" v-bind:value="option") {{option}}
          div.card
            label.label-header Style
            select(v-model="newComponent.selectedStyle")
              option(v-for="option in options.styles" v-bind:value="option") {{option}}
          div.card
            label.label-header Scoped Style
              br
              input(type="checkbox" v-model="newComponent.selectedScoped")
              span.checkable on
        div.component-form
          div.card
            label.label-header Component Name
            input(type="text", v-model="newComponent.name")
          div.card
            button(v-on:click="addNewProp") Add Prop
            div(v-for="(prop, index) in newComponent.props")
              label.label-header Props
                input(type="text", placeholder="props" v-model="prop.name")
              button.pseudo(v-on:click="removeProp(index)") Remove
          div.card
            button(v-on:click="addNewData") Add Data
            div(v-for="(data, index) in newComponent.data")
              label.label-header Data
                input(type="text", v-model="data.name")
              button.pseudo(v-on:click="removeData(index)") Remove
          div.card
            button(v-on:click="addNewWatch") Add Watch
            div(v-for="(watch, index) in newComponent.watches")
              label.label-header Watch
                input(type="text", v-model="watch.name")
              button.pseudo(v-on:click="removeWatch(index)") Remove
          div.card
            button(v-on:click="addNewComputed") Add Computed
            div(v-for="(computed, index) in newComponent.computed")
              label.label-header Computed
                input(type="text", v-model="computed.name")
              button.pseudo(v-on:click="removeComputed(index)") Remove
          div.card
            button(v-on:click="addNewMethod") Add Method
            div(v-for="(method, index) in newComponent.methods")
              label.label-header Method
                input(type="text", v-model="method.name")
              button.pseudo(v-on:click="removeMethod(index)") Remove
          //- LifeCycle Hook
          div.card LifeCycle Hooks
                label.label-header
                  input(type="checkbox" value="beforeCreate", v-model="newComponent.lifecycleHooks")
                  span.checkable beforeCreate
                label.label-header
                  input(type="checkbox" value="created", v-model="newComponent.lifecycleHooks")
                  span.checkable created
                label.label-header
                  input(type="checkbox" value="beforeMount", v-model="newComponent.lifecycleHooks")
                  span.checkable beforeMount
                label.label-header
                  input(type="checkbox" value="mounted", v-model="newComponent.lifecycleHooks")
                  span.checkable mounted
                label.label-header
                  input(type="checkbox" value="beforeUpdate", v-model="newComponent.lifecycleHooks")
                  span.checkable beforeUpdate
                label.label-header
                  input(type="checkbox" value="updated", v-model="newComponent.lifecycleHooks")
                  span.checkable updated
                label.label-header
                  input(type="checkbox" value="beforeDestroy", v-model="newComponent.lifecycleHooks")
                  span.checkable beforeDestroy
                label.label-header
                  input(type="checkbox" value="destroyed", v-model="newComponent.lifecycleHooks")
                  span.checkable destroyed
        //- Right (Result)
      codemirror.col-md-6(v-bind:code="compiledComponent", v-bind:options="editorOption")
</template>

<script>
// function htmlEscape (str) {
//   return str.replace(/&/g, '&amp;') // first!
//             .replace(/>/g, '&gt;')
//             .replace(/</g, '&lt;')
//             .replace(/"/g, '&quot;')
//             .replace(/'/g, '&#39;')
//             .replace(/`/g, '&#96;')
// }
//
// function html (literalSections, ...substs) {
//   // Use raw literal sections: we don’t want
//   // backslashes (\n etc.) to be interpreted
//   let raw = literalSections.raw
//
//   let result = ''
//
//   substs.forEach((subst, i) => {
//     // Retrieve the literal section preceding
//     // the current substitution
//     let lit = raw[i]
//
//     // In the example, map() returns an array:
//     // If substitution is an array (and not a string),
//     // we turn it into a string
//     if (Array.isArray(subst)) {
//       subst = subst.join('')
//     }
//
//     // If the substitution is preceded by a dollar sign,
//     // we escape special characters in it
//     if (lit.endsWith('$')) {
//       subst = htmlEscape(subst)
//       lit = lit.slice(0, -1)
//     }
//     result += lit
//     result += subst
//   })
//   // Take care of last literal section
//   // (Never fails, because an empty template string
//   // produces one literal section, an empty string)
//   result += raw[raw.length - 1] // (A)
//
//   return result
// }
import Beautify from 'js-beautify'

// let codemirror
export default {
  name: 'app',
  mounted () {

  },
  data: function () {
    return {
      options: {
        template: ['html', 'pug'],
        styles: ['css', 'scss', 'sass', 'stylus'],
        scripts: ['javascript', 'coffeescript'],
        scopedStyle: [true, false]
      },
      newComponent: {
        name: '',
        props: [],
        data: [],
        watches: [],
        computed: [],
        methods: [],
        lifecycleHooks: [],
        selectedTemplate: 'html',
        selectedStyle: 'css',
        selectedScript: 'javascript',
        selectedScoped: false
      },
      editorOption: {
        tabSize: 2,
        mode: 'text/javascript',
        theme: 'solarized light',
        lineNumbers: true,
        line: true,
        keyMap: 'sublime',
        extraKeys: { 'Ctrl': 'autocomplete' },
        foldGutter: true,
        gutters: ['CodeMirror-linenumbers', 'CodeMirror-foldgutter'],
        styleSelectedText: true,
        highlightSelectionMatches: { showToken: /\w/, annotateScrollbar: true },
        viewportMargin: Infinity
      }
    }
  },
  methods: {
    addNewProp () {
      this.newComponent.props.push({name: ''})
    },
    removeProp (index) {
      this.newComponent.props.splice(index, 1)
    },
    addNewData () {
      this.newComponent.data.push({name: ''})
    },
    removeData (index) {
      this.newComponent.data.splice(index, 1)
    },
    addNewWatch () {
      this.newComponent.watches.push({name: ''})
    },
    removeWatch (index) {
      this.newComponent.watches.splice(index, 1)
    },
    addNewComputed () {
      this.newComponent.computed.push({name: ''})
    },
    removeComputed (index) {
      this.newComponent.computed.splice(index, 1)
    },
    addNewMethod () {
      this.newComponent.methods.push({name: ''})
    },
    removeMethod (index) {
      this.newComponent.methods.splice(index, 1)
    }
  },
  watch: {
    // compiledComponent: {
    //   handler (newVal, oldVal) {
    //     var dom = document.querySelector('.code-mirror')
    //     if (!codemirror) {
    //       codemirror = CodeMirror.fromTextArea(dom, {
    //         lineNumbers: true,
    //         mode: 'vue',
    //         height: '600px',
    //         styleActiveLine: true,
    //         matchBrackets: true,
    //         theme: 'base16-dark'
    //       })
    //     }
    //     codemirror.setValue(newVal)
    //   },
    //   deep: true
    // }
  },
  computed: {
    compiledComponent: function () {
      const component = JSON.parse(JSON.stringify(this.newComponent))
      const isHTML = component.selectedTemplate === 'html'
      const templateLanguage = isHTML ? '' : ` lang='${component.selectedTemplate}'`
      const styleLanguage = component.selectedStyle === 'css' ? '' : ` lang='${component.selectedStyle}'`
      const styleScoped = component.selectedScoped ? ' scoped' : ''
      const scriptLanguage = component.selectedScript === 'javascript' ? '' : ` lang='${component.selectedScript}'`
      let scriptBody = 'export default {'
      scriptBody += 'name: ' + "'" + component.name + "',"
      // Props
      if (component.props && component.props.length > 0) {
        let scriptProps = component.props.map(prop => `'${prop.name}'`).join(',')
        scriptBody += `props: [${scriptProps}],`
      }
      // Check Data
      if (component.data && component.data.length > 0) {
        let scriptData = component.data.map(data => data.name + ": ''").join(',')
        scriptBody += `data () {return {${scriptData}}},`
      }
      // LifeCycle Hooks
      if (component.lifecycleHooks && component.lifecycleHooks.length > 0) {
        scriptBody += component.lifecycleHooks.map(hook => `${hook}() {},`).join(' ')
      }
      // Watches
      if (component.watches && component.watches.length > 0) {
        let scriptWatches = component.watches.map(watch => `${watch.name} (newVal, oldVal){}`).join(',')
        scriptBody += `watch: { ${scriptWatches} },`
      }
      // Computed
      if (component.computed && component.computed.length > 0) {
        let scriptComputed = component.computed.map(computed => `${computed.name} () {}`).join(',')
        scriptBody += `computed: { ${scriptComputed} },`
      }
      // Methods
      if (component.methods && component.methods.length > 0) {
        let scriptMethods = component.methods.map(method => `${method.name} () {}`).join(',')
        scriptBody += `methods: { ${scriptMethods} },`
      }
      scriptBody = scriptBody.trim().slice(0, -1)
      scriptBody += '}'
      scriptBody = Beautify(scriptBody, {
        'indent_size': 2,
        'indent_char': ' ',
        'other': ' ',
        'indent_level': 0,
        'indent_with_tabs': false,
        'preserve_newlines': true,
        'max_preserve_newlines': 2,
        'jslint_happy': true,
        'indent_handlebars': true,
        'object': {}
      })
      let dummyText = '<!-- This Component is generated by Vue Component Generator - ChangJoo Park. --> \n'
      let templateBody = isHTML ? '\n\t<div>\n\n\t</div>\n' : '\n\tdiv\n'
      let templateResult = '<template' + templateLanguage + '>' + templateBody + '</template>\n\n'
      let scriptResult = '<script' + scriptLanguage + '>\n' + scriptBody + '\n</scri' + 'pt>\n\n'
      let styleResult = '<style' + styleLanguage + styleScoped + '>\n\n' + '</style>'
      const compiledComponent = dummyText + templateResult + scriptResult + styleResult
      return compiledComponent
    }
  }
}
</script>

<style>
.generator {
  padding: 30px;
  padding-top: 10px;
}
.CodeMirror {
  font-family: 'Source Code Pro', 'Monaco','menlo', monospace !important;
  width: 50% !important;
  height: 100vh !important;
}
.card {
  padding: 5px;
  margin-bottom: 10px;
}
.label-header {
  font-size: 20px;
  margin-bottom: 10px;
}
</style>
