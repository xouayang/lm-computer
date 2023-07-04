<template>
  <div>
    <div class="mt-5 mb-3 pb-2 ml-2 font-weight-bold">
      ຄົ້ນຫາ
    </div>
    <v-card>
      <v-row class="d-flex align-center col-12">
        <v-col cols="12" md="12" sm="12">
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
      </v-row>

      <v-data-table
        :headers="headers"
      >
      </v-data-table>
    </v-card>
  </div>
</template>
<script>
export default {
  name: 'ManageChecklistsPages',
  data() {
    return {
      searchTerm: '',
      headers: [
        { text: 'ຊື່', value: 'name' },
        { text: 'ລາຄາ', value: 'price' },
        { text: 'Actions', value: 'action' },
      ],
    }
  }
}
</script>