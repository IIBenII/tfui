<template>
<div>
    <div class="text-subtitle-2 mb-2">With markup</div>

    <v-expansion-panels>
      <v-expansion-panel
        v-for="i in RessourceCreate"
        :key="i"
      >
        <v-expansion-panel-title>
          {{i.address}}
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>

    <div class="text-subtitle-2 mb-2">With markup</div>

      <v-data-table
        :headers="headers"
        :items="RessourceCreate"
        :items-per-page="5"
        class="elevation-1"
      ></v-data-table>


    
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
  methods: {
    importJson: function () {
      const files = document.getElementById('selectFiles').files;
      const fr = new FileReader();
      // const RessourceCreate = [];
      const RessourceDelete = [];
      const RessourceOther = [];
        fr.onload = e => {
          const resource_changes = JSON.parse(e.target.result).resource_changes;

          resource_changes.forEach(element => {
            if (element.change.actions[0] != "no-op") {
              element.change.actions.forEach(action => {
                if (action == "create") {
                  this.RessourceCreate.push(element)
                }
                if (action == "delete") {
                  RessourceDelete.push(element)
                }

                
                  RessourceOther.push(element)
                
              });
              // result.push(element)
            }
                      
          });

          const RessourceCreateFmt = JSON.stringify(this.RessourceCreate, null, 2);
          const RessourceDeleteFmt = JSON.stringify(RessourceDelete, null, 2);
          // const RessourceOtherFmt = JSON.stringify(RessourceOther, null, 2);
          document.getElementById('RessourceCreate').innerHTML = RessourceCreateFmt;
          document.getElementById('RessourceDelete').innerHTML = RessourceDeleteFmt;
          // document.getElementById('RessourceOther').innerHTML = RessourceOtherFmt;
        }
        fr.readAsText(files.item(0));
    }
  },

  data: () => ({
    RessourceCreate : [],
    headers: [
          {
            text: 'address',
            align: 'start',
            sortable: false,
            value: 'address',
          },
          { text: 'type', value: 'type' },
          { text: 'name', value: 'name' },
        ],
    ecosystem: [
      {
        text: 'vuetify-loader',
        href: 'https://github.com/vuetifyjs/vuetify-loader',
      },
      {
        text: 'github',
        href: 'https://github.com/vuetifyjs/vuetify',
      },
      {
        text: 'awesome-vuetify',
        href: 'https://github.com/vuetifyjs/awesome-vuetify',
      },
    ],
    importantLinks: [
      {
        text: 'Chat',
        href: 'https://community.vuetifyjs.com',
      },
      {
        text: 'Made with Vuetify',
        href: 'https://madewithvuejs.com/vuetify',
      },
      {
        text: 'Twitter',
        href: 'https://twitter.com/vuetifyjs',
      },
      {
        text: 'Articles',
        href: 'https://medium.com/vuetify',
      },
    ],
    whatsNext: [
      {
        text: 'Explore components',
        href: 'https://vuetifyjs.com',
      },
      {
        text: 'Roadmap',
        href: 'https://vuetifyjs.com/introduction/roadmap/',
      },
      {
        text: 'Frequently Asked Questions',
        href: 'https://vuetifyjs.com/getting-started/frequently-asked-questions',
      },
    ],
  }),
}
</script>
