<template>
  <v-simple-table height="300px">
    <template v-slot:default>
      <thead>
      <tr>
        <th class="text-left">
          ID
        </th>
        <th class="text-left">
          CARID
        </th>
        <th class="text-left">
          INSTOCK
        </th>
        <th class="text-left">
          HP
        </th>
        <th class="text-left">
          PRICE
        </th>
        <th class="text-left">
          COLOR
        </th>
        <th class="text-left">
          EDIT
        </th>
      </tr>
      </thead>
      <tbody>
      <tr
        v-for="item in desserts"
        :key="item.ID"
      >
        <td>{{ item.ID }}</td>
        <td>{{ item.CARID }}</td>
        <td>{{ item.INSTOCK }}</td>
        <td>{{ item.HP }}</td>
        <td>{{ item.PRICE }}</td>
        <td>
          {{ item.COLOR }}

          <div :style="{'background-color' : item.COLOR, width : '40px', height: '15px'}"></div>
        </td>
        <td>
          <v-btn color="info" small @click="editItem(item)">Edit</v-btn>
        </td>
      </tr>
      </tbody>
      <!--  dialog  -->
      <v-dialog
        v-model="modal"
        width="auto"
        class="dialog"
      >
        <v-card>
          <v-form ref="informationForm" @submit.prevent="save" autocomplete="off">
            <div class="dialog__header">
              <div class="dialog__header--title d-flex justify-center pa-4">Editing</div>
            </div>

            <div class="dialog__body">
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.ID"
                      label="ID"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.CARID"
                      label="CARID"
                      readonly
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-checkbox
                      v-model="editedItem.INSTOCK"
                      :label="`INSTOCK: ${editedItem.INSTOCK}`"
                    ></v-checkbox>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.HP"
                      :value="editedItem.HP"
                      label="PRICE"
                      type="number"
                      :rules="rules"
                      persistent-hint
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.PRICE"
                      label="PRICE"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.COLOR"
                      label="COLOR"
                      readonly
                    ></v-text-field>

                    <v-radio-group
                      v-model="editedItem.COLOR"
                    >
                      <v-radio label="Blue" color="info" value="Blue"></v-radio>
                      <v-radio label="yellow" color="yellow" value="Yellow"></v-radio>
                      <v-radio label="green" color="green" value="Green"></v-radio>
                      <v-radio label="red" color="red" value="Red"></v-radio>
                    </v-radio-group>
                  </v-col>
                </v-row>
              </v-container>
            </div>

            <div class="dialog__footer pa-4">
              <div class="w-100">
                <div class="d-flex justify-space-around">
                  <v-btn type="submit" color="success" :width="336">Save</v-btn>

                  <v-btn color="primary" :width="336" @click="close">Close Dialog</v-btn>
                </div>
              </div>
            </div>
          </v-form>
        </v-card>
      </v-dialog>
      <!-- dialog -->
    </template>
  </v-simple-table>
</template>

<script>
export default {
  data () {
    return {
      modal: false,
      ex7: 'primary',
      rules: [
        v => !!v || 'Required',
        v => v >= 100 || 'Loan should be above 50',
        v => v <= 550 || 'Max should not be above 150'
      ],
      desserts: [
        {
          ID: '1',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'red'
        },
        {
          ID: '2',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'false',
          HP: 140,
          PRICE: '30.000,00 €',
          COLOR: 'Blue'
        },
        {
          ID: '3',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'false',
          HP: 140,
          PRICE: '30.000,00 €',
          COLOR: 'Blue'
        },
        {
          ID: '4',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'Red'
        },
        {
          ID: '5',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'Red'
        },
        {
          ID: '6',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'yellow'
        }
      ],
      editedIndex: -1,
      editedItem: {
        ID: '',
        CARID: 0,
        INSTOCK: 0,
        HP: 0,
        PRICE: 0,
        COLOR: []
      },
      defaultItem: {
        ID: '',
        CARID: 0,
        INSTOCK: 0,
        HP: 0,
        PRICE: 0,
        COLOR: []
      }
    }
  },
  created () {
    this.initialize()
  },
  methods: {
    initialize () {
      this.desserts = [
        {
          ID: '1',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'red'
        },
        {
          ID: '2',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'false',
          HP: 140,
          PRICE: '30.000,00 €',
          COLOR: 'Blue'
        },
        {
          ID: '3',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'false',
          HP: 140,
          PRICE: '30.000,00 €',
          COLOR: 'yellow'
        },
        {
          ID: '4',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'Red'
        },
        {
          ID: '5',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'Blue'
        },
        {
          ID: '6',
          CARID: 'HJKFSHJK42525fsdfs',
          INSTOCK: 'true',
          HP: 250,
          PRICE: '50.000,00 €',
          COLOR: 'yellow'
        }
      ]
    },
    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.modal = true
    },
    close () {
      this.modal = false
    },
    save () {
      if (this.$refs.informationForm.validate()) {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        }
        this.close()
      }
    }
  }
}
</script>
