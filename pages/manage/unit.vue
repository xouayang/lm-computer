<template >
  <div>
    <div class="mt-5 mb-3 pb-2 ml-2 font-weight-bold">ຈັດການຂໍ້ມູນຫົວໜ່ວຍ</div>
    <v-card>
      <!-- search button------------------------------- -->
      <v-row class="d-flex align-center col-12">
        <v-col cols="12" md="10" sm="12">
          <v-card-title>
            <v-text-field
              v-model="searchTerm"
              prepend-inner-icon="mdi-magnify"
              label="ຄົ້ນຫາ"
              outlined
              hide-details
              dense
              small
              color="#9155FD"
            />
          </v-card-title>
        </v-col>
        <v-col cols="12" md="2" sm="3" class="d-flex justify-end">
          <v-btn
            style="width: 100"
            color="success"
            @click="showAddDialog = !showAddDialog"
            ><span style="color: white">ເພີ່ມຂໍ້ມູນ</span>
            <v-icon color="white">mdi-plus-outline</v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-data-table
        :headers="headers"
        :items="unitData.rows"
        :items-per-page="5"
        color="#9155FD"
        :search="searchTerm"
      >
        <template #[`item.actions`]="{ item }">
          <v-tooltip top color="error">
            <template #activator="{ on, attrs }">
              <v-btn icon v-bind="attrs" v-on="on" @click="showDelete(item)">
                <v-icon color="error"
                  >mdi-trash-can-outline</v-icon
                >
              </v-btn>
            </template>
            <span>ລຶບ</span>
          </v-tooltip>
          <v-tooltip top color="success">
            <template #activator="{ on, attrs }">
              <v-btn icon v-bind="attrs" v-on="on" @click="showEdit(item)">
                <v-icon color="primary"
                  >mdi-pencil-outline</v-icon
                >
              </v-btn>
            </template>
            <span>ເເກ້ໄຂ</span>
          </v-tooltip>
        </template>
        <template #[`item.created_at`]="{ item }">
          {{ $moment(item.created_at).format('DD-MM-YYYY') }}
        </template>
      </v-data-table>
    </v-card>
    <v-dialog v-model="showDailog" width="540" activator="parent" persistent>
      <v-card>
        <v-toolbar dark color="success">
          <v-card-title>ທ່ານເເນ່ໃຈບໍ ?</v-card-title>
          <v-spacer></v-spacer>
          <v-btn icon @click="showDailog = !showDailog">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-divider></v-divider>
        <div class="mt-2 col-12">
          <div>
            <v-card-title class="mb-2">
              ຫົວໜ່ວຍ : {{ showDataDelete.name }}
            </v-card-title>
          </div>
        </div>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="error"
            width="100"
            @click="deleteData(showDataDelete.id)"
          >
            <div class="text--white">ລຶບ</div>
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-row>
      <v-dialog
        v-model="dialog"
        width="600"
        transition="dialog-bottom-transition"
        persistent
      >
        <v-card>
          <v-toolbar dark color="success">
            <div>ແກ້ໄຂຂໍ້ມູນຫົວໜ່ວຍ</div>
            <v-spacer></v-spacer>
            <v-btn icon dark @click="dialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-divider></v-divider>
          <v-col cols="12">
            <v-text-field
              v-model="unitUpdate.name"
              outlined
              dense
              hide-details="auto"
              label="ຫົວໜ່ວຍ"
              color="success"
            />
          </v-col>
          <v-spacer></v-spacer>
          <div class="d-flex justify-end pa-4">
            <v-btn
              color="success"
              width="100"
              class="white--text"
              @click="updateUnit(showDataUpdate.id)"
              >ບັນທືກ</v-btn
            >
          </div>
        </v-card>
      </v-dialog>
    </v-row>
    <v-row>
      <v-dialog
        v-model="showAddDialog"
        width="600"
        transition="dialog-bottom-transition"
        persistent
      >
        <v-card>
          <v-toolbar dark color="success">
            <div>ເພີ່ມຂໍ້ມູນຫົວໜ່ວຍ</div>
            <v-spacer></v-spacer>
            <v-btn icon dark @click="showAddDialog = !showAddDialog">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-divider></v-divider>
          <v-col cols="12">
            <v-text-field
              v-model="unit.name"
              outlined
              dense
              hide-details="auto"
              label="ຊື່ຫົວໜ່ວຍ"
              color="success"
            />
          </v-col>
          <v-spacer></v-spacer>
          <div class="d-flex justify-end pa-4">
            <v-btn
              color="success"
              width="100"
              class="white--text"
              @click="addData"
              >ບັນທືກ</v-btn
            >
          </div>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>
<script>
export default {
  name: 'ManageUnit',
  data() {
    return {
      searchTerm: '',
      showDailog: false,
      dialog: false,
      showAddDialog: false,
      unit: {
        name: '',
      },
      unitUpdate: {
        name: '',
      },
      showDataDelete: '',
      showDataUpdate: '',
      headers: [
        { text: 'ລະຫັດ', value: 'id' },
        { text: 'ຫົວໜ່ວຍ', value: 'name' },
        { text: 'ວັນທີ່ບັນທຶກ', value: 'created_at' },
        { text: 'actions', value: 'actions' },
      ],
    }
  },
  computed: {
    unitData() {
      return this.$store.state.unit.UnitData
    },
  },
  async mounted() {
    await this.$store.dispatch('unit/getAll')
  },
  methods: {
    async addData() {
      await this.$store.dispatch('unit/createUnit', { ...this.unit })
      this.unit.name = ''
      await this.$store.dispatch('unit/getAll')
      this.showAddDialog = false
    },
    showDelete(data) {
      this.showDataDelete = data
      this.showDailog = true
    },
    async deleteData(id) {
      await this.$store.dispatch('unit/deleteData', id)
      await this.$store.dispatch('unit/getAll')
      this.showDailog = false
    },
    showEdit(data) {
      this.showDataUpdate = data
      if(this.showDataUpdate){
        this.unitUpdate.name = this.showDataUpdate.name
      }
      this.dialog = true
    },
    async updateUnit(id) {
      const data = this.unitUpdate;
      await this.$store.dispatch('unit/updateData', {data,id})
      await this.$store.dispatch('unit/getAll')
      this.dialog = false
    },
  },
}
</script> 