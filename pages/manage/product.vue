<template >
  <div>
    <div class="mt-5 mb-3 pb-2 ml-2 font-weight-bold">ຈັດການຂໍ້ມູນສິນຄ້າ</div>
    <v-card>
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
              color="success"
            />
          </v-card-title>
        </v-col>
        <v-col cols="12" md="2" sm="3" class="d-flex justify-end">
          <v-btn
            style="width: 100"
            color="success"
            @click="showAddDialog = !showAddDialog"
            ><span style="color: white">ເພີ່ມຂໍ້ມູນສິນຄ້າ</span>
            <v-icon color="white">mdi-plus-outline</v-icon>
          </v-btn>
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="productData"
        :items-per-page="5"
        color="success"
        :search="searchTerm"
      >
        <template #[`item.action`]="{ item }">
          <v-tooltip top color="error">
            <template #activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
                @click="showProductType(item)"
              >
                <v-icon color="error">mdi-trash-can-outline</v-icon>
              </v-btn>
            </template>
            <span>ລຶບ</span>
          </v-tooltip>
          <v-tooltip top color="#9155FD">
            <template #activator="{ on, attrs }">
              <v-btn icon v-bind="attrs" v-on="on" @click="showEdit(item)">
                <v-icon color="#9155FD">mdi-pencil-outline</v-icon>
              </v-btn>
            </template>
            <span>ເເກ້ໄຂ</span>
          </v-tooltip>
        </template>
        <template #[`item.created_at`]="{ item }">
          {{ $moment(item.created_at).format('DD-MM-YYYY') }}
        </template>
        <template #[`item.sale_price`]="{ item }">
          {{ toCurrencyString(item.sale_price) }}
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
          <div class="d-flex justify-space-between">
            <v-card-text>barcode : {{ showData.barcode }} </v-card-text>
            <v-card-text>ຊື່ສິນຄ້າ : {{ showData.name }} </v-card-text>
          </div>
          <div class="d-flex justify-space-between">
            <v-card-text
              >ປະເພດສິນຄ້າ : {{ showData.product_type_name }}
            </v-card-text>
            <v-card-text
              >ລາຄາ : {{ toCurrencyString(parseInt(showData.sale_price)) }}
            </v-card-text>
          </div>
          <div class="d-flex justify-space-between">
            <v-card-text>ຈຳນວນ : {{ showData.quatity }} </v-card-text>
            <v-card-text>ຫົວໜ່ວຍ : {{ showData.unit_name }} </v-card-text>
          </div>
        </div>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="error" width="100" @click="deleteData(showData.id)">
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
            <div>ແກ້ໄຂຂໍ້ມູນປະເພດສິນຄ້າ</div>
            <v-spacer></v-spacer>
            <v-btn icon dark @click="dialog = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-divider></v-divider>
          <v-col cols="12">
            <v-text-field
              v-model="product_type_update.name"
              outlined
              dense
              hide-details="auto"
              label="ຊື່"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="product_type_update.quatity"
              outlined
              dense
              hide-details="auto"
              label="ຈຳນວນ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="product_type_update.sale_price"
              outlined
              dense
              hide-details="auto"
              label="ລາຄາຂາຍ"
              color="success"
            />
          </v-col>
          <v-spacer></v-spacer>
          <div class="d-flex justify-end pa-4">
            <v-btn
              color="success"
              width="100"
              class="white--text"
              @click="editData(showEditData.id)"
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
            <div>ເພີ່ມຂໍ້ມູນສິນຄ້າ</div>
            <v-spacer></v-spacer>
            <v-btn icon dark @click="showAddDialog = !showAddDialog">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </v-toolbar>
          <v-divider></v-divider>
          <!-- <div>{{unitInfo.rows}}</div> -->
          <v-col cols="12">
            <v-select
              v-model="product_type_id"
              :items="productTypeData.rows"
              item-text="name"
              item-value="id"
              return-object
              outlined
              dense
              hide-details="auto"
              label="ຊື່ປະເພດສິນຄ້າ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-select
              v-model="unit_id"
              :items="unitInfo.rows"
              item-text="name"
              item-value="id"
              outlined
              dense
              return-object
              hide-details="auto"
              label="ຫົວໜ່ວຍ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-select
              v-model="supplier_id"
              :items="supplierInfo.rows"
              item-text="name"
              item-value="id"
              outlined
              dense
              hide-details="auto"
              label="ຜູ້ສະໜອງ"
              return-object
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="name"
              outlined
              dense
              hide-details="auto"
              label="ຊື່ສິນຄ້າ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="quatity"
              type="number"
              outlined
              dense
              hide-details="auto"
              label="ຈຳນວນ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="sale_price"
              type="number"
              outlined
              dense
              hide-details="auto"
              label="ລາຄາຂາຍ"
              color="success"
            />
          </v-col>
          <v-col cols="12">
            <v-text-field
              v-model="price"
              type="number"
              outlined
              dense
              hide-details="auto"
              label="ລາຄາ"
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
import laoCurrency from '@lailao10x/lao-currency'
export default {
  name: 'ManageProductType',
  data() {
    return {
      product_type: {
        name: '',
      },
      product_type_update: {
        name: '',
        quatity: '',
        sale_price: '',
      },
      supplierUpdate: {
        name: '',
        address: '',
        tel: '',
        email: '',
      },
      searchTerm: '',
      showDailog: false,
      dialog: false,
      showAddDialog: false,
      showData: {},
      showEditData: {},
      supplier_id: '',
      unit_id: '',
      product_type_id: '',
      name: '',
      quatity: 0,
      sale_price: 0,
      price: 0,

      headers: [
        { text: 'ລະຫັດ', value: 'barcode' },
        { text: 'ຜູ້ສະໜອງ', value: 'supplier_name' },
        { text: 'ຫົວໜ່ວຍ', value: 'unit_name' },
        { text: 'ປະເພດສິນຄ້າ', value: 'product_type_name' },
        { text: 'ຊື່ສິນຄ້າ', value: 'name' },
        { text: 'ລາຄາຂາຍ', value: 'sale_price' },
        { text: 'ຈຳນວນ', value: 'quatity' },
        { text: 'ວັນທີ່', value: 'created_at' },
        { text: 'actions', value: 'action' },
      ],
    }
  },
  computed: {
    productTypeData() {
      return this.$store.state.product.productTypeData
    },
    unitInfo() {
      return this.$store.state.product.unitData
    },
    supplierInfo() {
      return this.$store.state.product.supplierData
    },
    productData() {
      return this.$store.state.product.productData
    },
  },
  async mounted() {
    await this.$store.dispatch('product/getAll')
    await this.$store.dispatch('product/getUnit')
    await this.$store.dispatch('product/getAllData')
    await this.$store.dispatch('product/getProductData')
  },
  methods: {
    toCurrencyString(number) {
      return laoCurrency(number).format('LAK S')
    },
    showProductType(data) {
      console.log(data)
      this.showData = data
      this.showDailog = true
    },
    async deleteData(id) {
      await this.$store.dispatch('product/deleteData', id)
      await this.$store.dispatch('product/getProductData')
      this.showDailog = false
    },
    showEdit(dataEdit) {
      this.showEditData = dataEdit
      this.dialog = true
      if (this.showEditData) {
        this.product_type_update.name = this.showEditData.name
        this.product_type_update.quatity = this.showEditData.quatity
        this.product_type_update.sale_price = this.showEditData.sale_price
      }
    },
   async editData(id) {
      console.log(id)
      const data = this.product_type_update
      await this.$store.dispatch('product/updateData', { data, id })
       await this.$store.dispatch('product/getProductData')
      this.dialog = false
    },
    async addData() {
      const data = {
        name: this.name,
        quatity: this.quatity,
        sale_price: this.sale_price,
        price: this.price,
        supplier_id: this.supplier_id.id,
        unit_id: this.unit_id.id,
        product_type_id: this.product_type_id.id,
      }
      await this.$store.dispatch('product/createProduct', { ...data })
      this.name = ''
      this.quatity = ''
      this.sale_price = ''
      this.price = ''
      this.supplier_id = ''
      this.unit_id = ''
      this.product_type_id = ''
      await this.$store.dispatch('product/getProductData')
      this.showAddDialog = false
    },
  },
}
</script>
