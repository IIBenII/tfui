<template>
  <div>
    <v-card>
      <v-toolbar color="blue">
        <v-row class="mt-1">
          <v-toolbar-title>tfUI</v-toolbar-title>

          <v-spacer></v-spacer>
          <v-file-input
            label="Choose tf plan"
            id="selectFiles"
            dense
            @change="importJson"
            margin="60px"
            single-line:True
            truncate-length="50"
            accept=".json,application/json,application/JSON"
          ></v-file-input>
          <v-spacer></v-spacer>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            dense
            single-line
            hide-details
          ></v-text-field>
        </v-row>
      </v-toolbar>
    </v-card>

    <v-row v-if="RessourceCreate.length > 0">
      <v-col>
        <v-card>
          <v-card-title>
            <span style="color: green">Ressources added</span>
          </v-card-title>
          <v-data-table
            dense
            :headers="headers"
            :items="RessourceCreate"
            item-key="address"
            class="ressource-create"
            :expanded.sync="expandedCreate"
            show-expand
            :search="search"
          >
            <template v-slot:expanded-item="{ headers, item }">
              <td :colspan="headers.length">
                <pre>{{
                  JSON.stringify(item["change"]["after"], null, 2)
                }}</pre>
              </td>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>

    <v-row v-if="RessourceUpdate.length > 0">
      <v-col>
        <v-card>
          <v-card-title>
            <span style="color: orange">Ressources updated</span>
          </v-card-title>
          <v-data-table
            dense
            :headers="headers"
            :items="RessourceUpdate"
            item-key="address"
            class="ressource-update"
            :expanded.sync="expandedUpdate"
            show-expand
            :search="search"
          >
            <template v-slot:expanded-item="{ headers, item }">
              <td :colspan="headers.length">
                <span v-html="processDif(item)"></span>
              </td>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>

    <v-row v-if="RessourceDelete.length > 0">
      <v-col>
        <v-card>
          <v-card-title>
            <span style="color: red">Ressources deleted</span>
          </v-card-title>
          <v-data-table
            dense
            :headers="headers"
            :items="RessourceDelete"
            item-key="address"
            class="ressource-delete"
            :expanded.sync="expandedDelete"
            show-expand
            :search="search"
          >
            <template v-slot:expanded-item="{ headers, item }">
              <td :colspan="headers.length">
                <pre>{{
                  JSON.stringify(item["change"]["before"], null, 2)
                }}</pre>
              </td>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "MainPage",

  methods: {
    importJson: function () {
      this.RessourceCreate = [];
      this.RessourceDelete = [];
      this.RessourceUpdate = [];
      this.expandedCreate = [];
      this.expandedUpdate = [];
      this.expandedDelete = [];
      const files = document.getElementById("selectFiles").files;
      const fr = new FileReader();
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
            } 
            else if (
              element.change.actions.length == 1 &&
              element.change.actions[0] == "update"
            ) {
              this.RessourceUpdate.push(element);
            }else if (
              element.change.actions.length == 2 &&
              element.change.actions.sort().join(",") ===
                ["create", "delete"].sort().join(",")
            ) {
              this.RessourceUpdate.push(element);
            }
          }
        });
      };
      fr.readAsText(files.item(0));
    },
    processDif: function (entity) {
      const afterValues = entity["change"]["after"];
      const beforeValues = entity["change"]["before"];
      var result = {};
      for (const [key, value] of Object.entries(afterValues)) {
        if (key == "condition") {
          continue;
        } else if (value != beforeValues[key]) {
          result[key] =
            `<span style= 'color:orange'>` +
            beforeValues[key] +
            "->" +
            value +
            "</span>";
        } else {
          result[key] = value;
        }
      }
      return "<pre>" + JSON.stringify(result, null, 2) + "</pre>";
    },
  },

  data: () => ({
    search: "",
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
