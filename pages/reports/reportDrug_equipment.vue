<template>
  <div class="mt-3">
    <v-card>
      <div class="pa-2 ml-1">ລາຍງານຂໍ້ມູນຢາ-ອຸປະກອນ</div>
      <v-row class="col-12 d-flex justify-center">
        <v-col cols="12" sm="12" md="6">
          <v-text-field
            v-model="value"
            clearable
            outlined
            dense
            color="#9155FD"
            label="ວັນ ເດືອນ ປີ"
          >
            <template #append>
              <DataPicker v-model="value" />
            </template>
          </v-text-field>
        </v-col>
        <v-col cols="12" sm="12" md="6" class="text-center">
          <div>ຄົ້ນຫາວັນທີ ເດືອນ ປີ : <span style="color:red">{{ value }}</span></div>
        </v-col>
        <div>Data : {{num}}</div>
      </v-row>
      <!-- <v-data-table :headers="headers" :items="items"> </v-data-table> -->
    </v-card>
  </div>
</template>
<script>
export default {
  name:"ReportDrugEquipment" ,
  data() {
    return {
        value:'',
        items:[]
    }
  }, 
  // computed: {
  //   num() {
  //     return this.$store.state.user.number;
  //   },
  //   mapUser() {
  //     return this.$store.state.user.suppliers;
  //   },
  // },
  //   async mounted() {
  //   await this.$store.dispatch("user/showUser");
  // },

}
</script>
