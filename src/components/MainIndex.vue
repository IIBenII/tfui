<template>
<div>
    <div class="text-subtitle-2 mb-2">With markup</div>

    <v-expansion-panels>
      <v-expansion-panel
        v-for="i in 3"
        :key="i"
      >
        <v-expansion-panel-title>
          Item
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>

    <div class="text-subtitle-2 mt-4 mb-2">With props</div>

    <v-expansion-panels>
      <v-expansion-panel
        v-for="i in 3"
        :key="i"
        title="Item"
        text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."
      ></v-expansion-panel>
    </v-expansion-panels>
  </div>
  
  <input type="file" id="selectFiles" value="" />
  <button id="import" v-on:click="importJson">Import The File!</button>
  <pre id="RessourceCreate"></pre> <br />
  <pre id="RessourceDelete"></pre><br />
  <!-- <pre id="RessourceOther"></pre><br /> -->
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods: {
    importJson: function () {
      const files = document.getElementById('selectFiles').files;
      const fr = new FileReader();
      const RessourceCreate = [];
      const RessourceDelete = [];
      const RessourceOther = [];
        fr.onload = e => {
          const resource_changes = JSON.parse(e.target.result).resource_changes;

          resource_changes.forEach(element => {
            if (element.change.actions[0] != "no-op") {
              element.change.actions.forEach(action => {
                if (action == "create") {
                  RessourceCreate.push(element)
                }
                if (action == "delete") {
                  RessourceDelete.push(element)
                }

                
                  RessourceOther.push(element)
                
              });
              // result.push(element)
            }
                      
          });

          const RessourceCreateFmt = JSON.stringify(RessourceCreate, null, 2);
          const RessourceDeleteFmt = JSON.stringify(RessourceDelete, null, 2);
          // const RessourceOtherFmt = JSON.stringify(RessourceOther, null, 2);
          document.getElementById('RessourceCreate').innerHTML = RessourceCreateFmt;
          document.getElementById('RessourceDelete').innerHTML = RessourceDeleteFmt;
          // document.getElementById('RessourceOther').innerHTML = RessourceOtherFmt;
        }
        fr.readAsText(files.item(0));
    }
  }
}

// var example1 = new Vue({
//   el: '#example-1',
//   data: {
//     counter: 0
//   }
// })

// document.getElementById('import').onclick = () => {
//   const files = document.getElementById('selectFiles').files;
//   if (files.length <= 0) {
//     return false;
//   }

//   const fr = new FileReader();

//   fr.onload = e => {
//     const result = JSON.parse(e.target.result);
//     const formatted = JSON.stringify(result, null, 2);
//     document.getElementById('result').innerHTML = formatted;
//   }
//   fr.readAsText(files.item(0));
// };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
