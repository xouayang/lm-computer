<template>
  <div>
    <!-- <v-row class="col-12 d-flex align-center">
      <v-col cols="12" md="6" sm="6"
        ><div class="text-body-1 pl-5" style="font-size: 20">
          Dashboard
        </div></v-col
      >
      <v-spacer></v-spacer>
      <v-col cols="12" md="6" sm="6">
        <v-text-field
          append-icon="mdi-magnify"
          label="ຄົ້ນຫາ"
          outlined
          hide-details
          dense
          color="#9155FD"
        />
      </v-col>
    </v-row> -->
    <v-row>
      <v-col cols="4">
        <v-card height="100"  class="pa-2" elevation="0"
          >ລາຍການຂາຍໃຫມ່
          <v-row align="center">
            <v-col><div class="mt-3 text-h4"></div></v-col>
            <v-col class="mt-2">
              <v-btn text small fab color="success">
                <v-badge
                  :content="showPatiented?.count ? showPatiented?.count : '0'"
                  color="error"
                >
                  <v-icon>mdi-bell-plus-outline</v-icon>
                </v-badge></v-btn
              >
              <!-- <v-btn color="success" rounded>{{showPatiented?.count}}++</v-btn> -->
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-col cols="4">
        <v-card height="100"  class="pa-2" elevation="0"
          >ລາຍການຂາຍທັງໝົດ
          <v-row align="center">
            <v-col><div class="mt-3 text-h4"></div></v-col>
            <v-col>
              <v-btn class="cyan accent-4 white--text" rounded
                ><span style="color: white">150</span
              ></v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-col cols="4">
        <v-card height="100"  class="pa-2" elevation="0">
          ພະນັກງານ (ຊາຍ)
          <v-row align="center">
            <v-col><div class="mt-3 text-h4"></div></v-col>
            <v-col>
              <v-btn color="success" rounded>3</v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-col cols="4">
        <v-card height="100"  class="pa-2" elevation="0"
          >ພະນັກງານ (ຍິງ)
          <v-row align="center">
            <v-col><div class="mt-3 text-h4"></div></v-col>
            <v-col>
              <v-btn class="cyan accent-4 white--text" rounded
                ><span style="color: white">2</span
              ></v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-col cols="4">
          
        <v-card height="100"  class="pa-2"  elevation="0">
          ລູກຄ້າ
          <v-row align="center">
            <v-col><div class="mt-3 text-h4"></div></v-col>
            <v-col>
              <v-btn color="success" rounded>1000</v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <v-col cols="4">
        <v-card
          height="100"
          
          class="pa-2"
          elevation="0"
          style="cursor: pointer"
          >ປະເພດ (Category)
          <v-row align="center">
            <v-col><div class="mt-3 text-h4">100</div></v-col>
            <v-col>
              <v-btn class="cyan accent-4 white--text" rounded
                ><span style="color: white">50++</span></v-btn
              >
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>
<script>
export default {
  name: 'Homepages',
  middleware: 'auth',
  data() {
    return {
      interval: {},
      value: 0,
    }
  },
  beforeUnmount() {
    clearInterval(this.interval)
  },
  // computed: {
  // dataMale() {
  //   return this.$store.state.user.maleData
  // },
  // dataFemal() {
  //   return this.$store.state.user.femaleData
  // },
  // countShow() {
  //  return this.$store.state.user.dataCount
  // },
  // showPatients() {
  //   return this.$store.state.user.Patients
  // },
  // showPatiented() {
  //   return this.$store.state.user.Patiented
  // }
  // },
  mounted() {
    this.interval = setInterval(() => {
      if (this.value === 100) {
        return (this.value = 0)
      }
      this.value += 10
    }, 1000)
    // await this.$store.dispatch('user/male')
    // await this.$store.dispatch('user/female')
    // await this.$store.dispatch('user/countType')
    // await this.$store.dispatch('user/getPatients')
    // await this.$store.dispatch('user/getPatiented')
  },
}
</script>
