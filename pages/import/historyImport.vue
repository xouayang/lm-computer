<template>
  <div class="mt-2 ml-2">
    <!-- {{prescrition}} -->
    <div style="font-size: 20px; color: #9155fd" class="mb-7 ml-4">
      ປະຫວັດຈັດຊື້ຢາ
    </div>
    <v-card class="mt-3 text-center">
      <v-row class="col-12">
        <v-col cols="12" md="12" sm="12">
          <v-text-field
            v-model="search"
            name="sale"
            label="ຄົ້ນຫາ"
            prepend-inner-icon="mdi-magnify"
            outlined
            dense
          >
            <!-- <template #[`item.createdAt`]="{ item }">
              {{ $moment(item.createdAt).format('DD-MM-YYY') }}
            </template> -->
          </v-text-field>
        </v-col>
      </v-row>
      <v-data-table :headers="headers" :items="getStory" :search="search">
        <template #[`item.order_date`]="{value}">
          <div>
            {{ formatDateBill(value) }}
          </div>
        </template>
        <template #[`item.status`]="{value}">
          <div v-if="value === 1" class="green white--text px-1">
           ລໍຖ້າການນຳເຂົ້າ
          </div>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>
<script>
export default {
  name: 'History_Imports',
  data() {
    return {
      getStory:[],
      token: this.$cookies.get('token'),
      search: '',
      headers: [
        { text: 'ລະຫັດໃບບິນ', value: 'bill_number' },
        { text: 'ປະເພດຢາ', value: 'type_name' },
        { text: 'ຊື່ສິນຄ້າ', value: 'name' },
        { text: 'ຈຳນວນ', value: 'order_details_price' },
        { text: 'ລາຄາ', value: 'qty_small' },
        { text: 'ຫົວໜ່ວຍ', value: 'unit_name' },
        { text: 'ວັນເດືອນປີສັ່ງຊື້', value: 'order_date' },
        { text: 'ສະຖານະ', value: 'status' },
      ],
    }
  },
  computed: {
    // history() {
    //   return this.$store.state.user.importHistory
    // },
  },
  async mounted() {
    // await this.$store.dispatch('user/getImportHistory')
    await this.$axios
      .get('http://localhost:2023/order', {
        headers: {
          Authorization: `LMCOMPUTER ${this.token}`,
        },
      })
      .then((res) => {
        this.getStory = res.data
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
  },
}
</script>
