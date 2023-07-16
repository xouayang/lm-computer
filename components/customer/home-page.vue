<template>
  <div>
    <v-btn color="primary" @click="showListDialog = true">ເລືອກລູກຄ້າ</v-btn>

    <!-- User List Dialog -->
    <v-dialog v-model="showListDialog" persistent max-width="1000px">
      <v-card>
        <v-card-text>
          <v-data-table
            :headers="headers"
            :items="filteredUsers"
            :items-per-page="itemsPerPage"
            :page.sync="currentPage"
            item-key="id"
            hide-default-footer
          >
            <template #top>
              <v-toolbar flat>
                <v-toolbar-title>ລາຍການລູກຄ້າ</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-text-field
                  v-model="searchQuery"
                  hide-details
                  label="Search"
                  outlined
                  dense
                ></v-text-field>
                <v-divider vertical class="mx-1" inset></v-divider>
                <v-btn class="green white--text" @click="showFormDialog = true">
                  <v-icon>mdi-plus</v-icon> ລູກຄ້າໃຫມ່
                </v-btn>
              </v-toolbar>
            </template>

            <template #[`item.action`]="{ item }">
              <v-btn class="mr-2" @click="selectUser(item.id)">
                ເລືອກລູກຄ້າ
              </v-btn>
              <!-- <v-icon small @click="deleteUser(item.id)">mdi-delete</v-icon> -->
            </template>

            <template #footer>
              <v-pagination
                v-model="currentPage"
                :length="Math.ceil(filteredUsers.length / itemsPerPage)"
                prev-icon="mdi-chevron-left"
                next-icon="mdi-chevron-right"
              ></v-pagination>
            </template>
          </v-data-table>
          <v-alert v-if="filteredUsers.length === 0" value="true" type="info">
            ບໍ່ມີລາຍການທີ່ທ່ານຄົ້ນຫາ
          </v-alert>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="showListDialog = false"
            >Close</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- User Form Dialog -->
    <v-dialog v-model="showFormDialog" persistent max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">ສ້າງລູກຄ້າໃຫມ່</span>
        </v-card-title>

        <v-card-text>
          <v-form ref="form" v-model="valid">
            <v-text-field
              v-model="user.Fname"
              label="ຊື່"
              required
            ></v-text-field>
            <v-text-field
              v-model="user.Lname"
              label="ນາມສະກຸນ"
              required
            ></v-text-field>
            <v-select
              v-model="user.gender"
              :items="genders"
              label="ເພດ"
              required
            ></v-select>
            <v-text-field v-model="user.village" label="ບ້ານ"></v-text-field>
            <v-text-field v-model="user.province" label="ແຂວງ"></v-text-field>
            <v-text-field
              v-model="user.tel"
              label="ເບີ"
              required
            ></v-text-field>
            <v-text-field
              v-model="user.email"
              label="ອີເມວ"
              required
            ></v-text-field>
          </v-form>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="cancel">Cancel</v-btn>
          <v-btn
            :loading="loading"
            color="blue darken-1"
            text
            :disabled="!valid"
            @click="save"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      showListDialog: false,
      showFormDialog: false,
      valid: false,
      user: {
        Fname: '',
        Lname: '',
        gender: '',
        village: '',
        province: '',
        tel: '',
        email: '',
      },
      genders: ['ຊາຍ', 'ຍິງ'],
      searchQuery: '',
      itemsPerPage: 10,
      currentPage: 1,
      headers: [
        { text: 'Index', value: 'index' },
        { text: 'First Name', value: 'Fname' },
        { text: 'Last Name', value: 'Lname' },
        { text: 'Telephone', value: 'tel' },
        { text: 'Email', value: 'email' },
        { text: 'Action', value: 'action', sortable: false },
      ],
    }
  },
  computed: {
    getCustomer() {
      return this.$store.state.customer.all
    },
    filteredUsers() {
      const query = this.searchQuery.toLowerCase().trim()
      return this.getCustomer
        .map((item, index) => {
          return { index: index + 1, ...item }
        })
        .filter((user) => {
          const matchesFname = user.Fname.toLowerCase().includes(query)
          const matchesTel = user.tel.includes(query)
          return matchesFname || matchesTel
        })
    },
  },
  mounted() {
    this.$store.dispatch('customer/selectAll')
  },
  methods: {
    async save() {
      if (this.$refs.form.validate()) {
        this.loading = true
        try {
          await this.$axios.post('http://localhost:2023/customer', this.user)
          this.$store.dispatch('customer/selectAll')
          this.cancel()
        } catch (error) {
          this.$toast.success('Error creating customer:', error)
        }
        this.loading = false
      }
    },
    cancel() {
      this.showFormDialog = false
      this.$refs.form.reset()
    },
    selectUser(id) {
      this.$store.commit('customer/setCustomer_id', id)
      this.showListDialog = false
    },
    deleteUser(id) {
      // Implement delete user logic
    },
  },
}
</script>
