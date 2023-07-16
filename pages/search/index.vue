<template>
  <v-card>
    <v-tabs
      v-model="tab"
      background-color="primary"
      dark
    >
      <v-tab
        v-for="item in items"
        :key="item.tab"
      >
        {{ item.tab }}
      </v-tab>
    </v-tabs>

    <v-tabs-items v-model="tab">
      <v-tab-item key="product">
        <v-card flat>
          <v-card-text>
            <Product-component />
          </v-card-text>
        </v-card>
      </v-tab-item>

      <v-tab-item key="customer">
        <v-card flat>
          <v-card-text>
            <Customer />
          </v-card-text>
        </v-card>
      </v-tab-item>

      <v-tab-item key="employee">
        <v-card flat>
          <v-card-text>
            <Employee />
          </v-card-text>
        </v-card>
      </v-tab-item>
    </v-tabs-items>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      tab: null,
      items: [
        { tab: 'Product', content: 'Product Tab' },
        { tab: 'Customer', content: 'Customer Tab' },
        { tab: 'Employee', content: 'Employee Tab' }
      ]
    };
  }
};
</script>

