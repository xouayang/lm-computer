<template>
  <div>
    <div>
      <v-btn tile fixed right color="error" class="mb-10" to="/sale"
        ><v-icon>mdi-close</v-icon> ປິດ</v-btn
      >
    </div>
    <div class="shop-info">
      <img src="/image/adminlogo.png" alt="Shop Logo" class="logo" />

      <div class="shop-details">
        <h2>LM Computer</h2>
        <p>Dongdok, Saythany, vientian capital</p>
        <p>Phone: (020) 7878-0000</p>
        <p class="bill-date"> ວັນທີຂາຍ: {{ formatDateLo(getSale.sale_date) }}</p>
    <p class="bill-date"> ພະນັກງານຂາຍ: {{ getSale.employeeName }}</p>
      </div>{{ getDetail }}
      <v-data-table :items="getDetail" :headers="headers" class="product-table">
      <!-- <template #[`item.total`]="{ item }"> -->
        <!-- <td>{{ formatPrice(item.sale_price * item.quantity) }}</td> -->
      <!-- </template> -->
    </v-data-table>

      <p class="total-price green--text">
        ລວມທັງໝົດ: {{ formatPrice(getSale.Totalkip) }}ກິບ
      </p>
    {{ getSale }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'LmComputerId',

  data() {
    return {
        headers: [
        {
          text: 'Name',
          value: 'productName',
        },
        {
          text: 'Category',
          value: 'categoryName',
        },
        {
          text: 'Price',
          value: 'sale_price',
        },
        {
          text: 'Quantity',
          value: 'quantity',
        },
        {
          text: 'Total',
          value: 'total',
        },
      ],
    }
  },
computed:{
    getDetail() {
      return this.$store.state.sale.saleDetail
    },
    getSale() {
      return this.$store.state.sale.sale
    },
},
  mounted() {
    this.$store.dispatch('sale/getBysaleId', this.$route.params.id)
    this.$store.dispatch('sale/selectSale', this.$route.params.id)
  },

  methods: {},
}
</script>

<style lang="scss" scoped></style>
