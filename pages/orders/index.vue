<template >
  <div>
    <div class="mt-4 mb-3 ml-2 font-weight-bold container">
      ກວດສອບຂໍ້ມູນສິນຄ້າ
    </div>
    <!-- <div>ກວດສອບນຳເຂົ້າ</div> -->
    <!-- {{medicines}} -->
    <v-card class="pa-10">
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            v-model="searchTerm"
            prepend-inner-icon="mdi-magnify"
            label="ຄົ້ນຫາ"
            outlined
            hide-details
            dense
            small
            color="#9155FD"
            class="mb-2"
          />
        </v-col>
        <v-col cols="12" md="6">
          <v-select
            v-model="supId"
            :items="supData"
            item-text="name"
            item-value="id"
            outlined
            dense
            hide-details="auto"
            label="ຜູ້ສະໜອງ"
            return-object
            color="success"
          ></v-select>
        </v-col>
      </v-row>

      <v-col>
        <v-data-table
          v-model="selectedItems"
          :items="orderData"
          :headers="headers"
          show-select
          :search="searchTerm"
          item-key="id"
        >
          <template #[`item.indx`]="{ index }">
            {{ index + 1 }}
          </template>
          <template #[`item.sale_price`]="{ item }">
            <span style="color: red">{{
              toCurrencyString(item.sale_price)
            }}</span>
          </template>
        </v-data-table>
      </v-col>

      <v-divider></v-divider>
    </v-card>
    <v-row class="mt-3 mb-8">
      <v-col cols="12" class="text-end">
        <v-btn color="#9155FD" @click="getSelectedItems">
          <span style="color: white">ບັນທຶກການສັ່ງຊື້</span>
          <v-icon color="white">mdi-content-save-check-outline</v-icon>
        </v-btn>
      </v-col>
    </v-row>

    <v-dialog
      v-model="dialog"
      persistent
      :overlay="false"
      max-width="800px"
      transition="dialog-transition"
    >
      <v-card>
        <v-card-title> ລາຍການສັ່ງຊື້ສິນຄ້າ </v-card-title>
        <v-card-text>
          <v-data-table :headers="headers" :items="selectedItems">
            <template #[`item.indx`]="{ index }">
              {{ index + 1 }}
            </template>
            <template #[`item.quatity`]="{ item }">
              <v-text-field
                v-model="item.quatity"
                type="number"
                hide-details="auto"
                dense
                style="width: 50px"
              ></v-text-field>
            </template>
            <template #[`item.price`]="{ item }">
              <span style="color: red">{{ toCurrencyString(item.price) }}</span>
            </template>
          </v-data-table>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red " outlined @click="dialog = false">ຍົກເລີກ</v-btn>
          <v-btn color="success white--text " @click="order">ບັນທຶກ</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
import laoCurrency from '@lailao10x/lao-currency'
export default {
  name: 'OrderPages',
  data() {
    return {
      supId: '',
      token: this.$cookies.get('token'),
      amount: [],
      selectedItems: [],
      orderData: [],
      supData: [],
      value: null,
      value1: null,
      searchTerm: '',
      dialog: false,
      showAddDialog: false,
      search: '',
      data: [],
    }
  },
  computed: {
    headers() {
      return [
        { text: 'ລຳດັບ', value: 'indx' },
        { text: 'ປະເພດສິນຄ້າ ', value: 'product_type_name' },
        { text: 'ຊື່ສິນຄ້າ', value: 'name' },
        { text: 'ຈຳນວນ', value: 'quatity' },
        { text: 'ລາຄາຂາຍ', value: 'sale_price' },
        { text: 'ຫົວໜ່ວຍ', value: 'unit_name' },
      ]
    },
  },
  async mounted() {
    await this.$axios.get('http://localhost:2023/Product').then((res) => {
      this.orderData = res.data
    })

    await this.$axios.get('http://localhost:2023/supplier').then((res) => {
      this.supData = res.data.rows
    })
  },
  methods: {
    async order() {
      const data = {
        supplier_id: this.supId,
        item: this.selectedItems,
        // token: this.token
      }
      await this.$axios
        .post('http://localhost:7000/create-prescription', data, {
          headers: {
            Authorization: `CLINIC ${this.token}`,
          },
        })
        .then((res) => {
          this.$router.push('/orders/historyOrder')
          this.$toast.success('ສັ່ງຊື້ສຳເລັດ', {
            duration: 2000,
            position: 'top-right',
            iconPack: 'mdi',
            icon: 'check',
          })
        })
        .catch(() => {
          this.$toast.error('ສັ່ງຊື້ບໍ່ສຳເລັດ', {
            duration: 2000,
            position: 'top-right',
            iconPack: 'mdi',
            icon: 'close',
          })
        })
      this.dialog = false
    },
    onAmountEdit(item) {
      console.log(`Edited amount value: ${item.amount}`)
    },
    showData() {
      this.dialog = false
      this.$refs.anyName.reset()
    },
    getSelectedItems() {
      this.dialog = true
    },
    toCurrencyString(number) {
      return laoCurrency(number).format('LAK S')
    },
  },
}
</script>
<style scoped>
.font {
  font-family: 'Noto Serif Lao', serif;
}
</style>



