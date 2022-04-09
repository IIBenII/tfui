<template>
  <div>
    <v-card color="grey lighten-4" flat tile>
      <v-toolbar dense>
        <v-toolbar-title>tfUI</v-toolbar-title>

        <v-spacer></v-spacer>
        <v-file-input
          truncate-length="15"
          label="Choose tf plan"
          id="selectFiles"
        ></v-file-input>
        <v-btn id="import" v-on:click="importJson">Import The File!</v-btn>

      </v-toolbar>
    </v-card>

    <v-card v-if="RessourceCreate.length > 0">
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        dense
        :headers="headers"
        :items="RessourceCreate"
        item-key="address"
        class="ressource-create"
        :expanded.sync="expandedCreate"
        show-expand
      >
        <template v-slot:expanded-item="{ headers, item }">
          <td :colspan="headers.length">
            <pre>{{ JSON.stringify(item, null, 2) }}</pre>
          </td>
        </template>
      </v-data-table>
    </v-card>

    <v-card v-if="RessourceUpdate.length > 0">
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        dense
        :headers="headers"
        :items="RessourceUpdate"
        item-key="address"
        class="ressource-update"
        :expanded.sync="expandedUpdate"
        show-expand
      >
        <template v-slot:expanded-item="{ headers, item }">
          <td :colspan="headers.length">
            <pre>{{ JSON.stringify(item, null, 2) }}</pre>
          </td>
        </template>
      </v-data-table>
    </v-card>

    <v-card v-if="RessourceDelete.length > 0">
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        dense
        :headers="headers"
        :items="RessourceDelete"
        item-key="address"
        class="ressource-delete"
        :expanded.sync="expandedDelete"
        show-expand
      >
        <template v-slot:expanded-item="{ headers, item }">
          <td :colspan="headers.length">
            <pre>{{ JSON.stringify(item, null, 2) }}</pre>
          </td>
        </template>
      </v-data-table>
    </v-card>

    <!-- <pre id="RessourceCreate"></pre>
    <br />
    <pre id="RessourceDelete"></pre>
    <br />
    <pre id="RessourceOther"></pre>
    <br /> -->
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  methods: {
    importJson: function () {
      const files = document.getElementById("selectFiles").files;
      const fr = new FileReader();
      // const RessourceCreate = [];
      // const RessourceDelete = [];
      // const RessourceOther = [];
      fr.onload = (e) => {
        const resource_changes = JSON.parse(e.target.result).resource_changes;

        resource_changes.forEach((element) => {
          if (element.change.actions[0] != "no-op") {
            if (
              element.change.actions.length == 1 &&
              element.change.actions[0] == "create"
            ) {
              this.RessourceCreate.push(element);
            } else if (
              element.change.actions.length == 1 &&
              element.change.actions[0] == "delete"
            ) {
              this.RessourceDelete.push(element);
            } else if (
              element.change.actions.length == 2 &&
              element.change.actions.sort().join(",") ===
                ["create", "delete"].sort().join(",")
            ) {
              this.RessourceUpdate.push(element);
            }
            // element.change.actions.forEach((action) => {
            //   if (action == "create") {
            //     this.RessourceCreate.push(element);
            //   }
            //   if (action == "delete") {
            //     RessourceDelete.push(element);
            //   }

            //   RessourceOther.push(element);
            // });
            // result.push(element)
          }
        });

        // const RessourceCreateFmt = JSON.stringify(
        //   this.RessourceCreate,
        //   null,
        //   2
        // );
        // const RessourceDeleteFmt = JSON.stringify(RessourceDelete, null, 2);
        // const RessourceOtherFmt = JSON.stringify(RessourceOther, null, 2);
        // document.getElementById("RessourceCreate").innerHTML =
        //   RessourceCreateFmt;
        // document.getElementById("RessourceDelete").innerHTML =
        //   RessourceDeleteFmt;
        // document.getElementById('RessourceOther').innerHTML = RessourceOtherFmt;
      };
      fr.readAsText(files.item(0));
    },
  },

  data: () => ({
    RessourceCreate: [],
    RessourceDelete: [],
    RessourceUpdate: [],
    expandedCreate: [],
    expandedUpdate: [],
    expandedDelete: [],
    headers: [
      {
        text: "address",
        align: "start",
        sortable: false,
        value: "address",
      },
      { text: "type", value: "type" },
      { text: "name", value: "name" },
    ],
  }),
};
</script>
