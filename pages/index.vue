<template>
  <div class="wrapper">
    <v-card class="mx-auto pa-6" max-width="900">
      <v-row class="align-center">
        <v-col>
          <v-text-field
            v-model="searchInput"
            label="Поиск"
            variant="outlined"
          ></v-text-field>
        </v-col>
        <v-col cols="auto">
          <v-icon small>mdi-sort</v-icon>
        </v-col>
        <v-col cols="auto">
          <v-btn color="primary" @click="search = searchInput">Найти</v-btn>
        </v-col>
      </v-row>
      <v-data-table
        v-model="selected"
        :footer-props="{
          itemsPerPageText: 'Строк на странице',
        }"
        :headers="headers"
        :items="users"
        :search="search"
        class="elevation-1"
        item-key="name"
        show-select
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            @click="deleteItem(item)"
          >
            mdi-delete
          </v-icon>
        </template>
        <template v-slot:footer.page-text="pagination">{{ pagination.pageStart }}-{{ pagination.pageStop }} из
          {{ pagination.itemsLength }}
        </template>
        <template v-slot:footer>
          <v-icon class="pa-5 pl-2" hide-details style="position: absolute; right: 55px" @click="deleteSelected">
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      search: '',
      searchInput: '',
      selected: [],
      headers: [
        {value: 'name'},
        {value: 'username'},
        {value: 'address.zipcode'},
        {value: 'actions'},
      ],
      users: [],
    }
  },
  methods: {
    deleteItem(item) {
      this.users.splice(item, 1)
    },
    deleteSelected() {
      let ids = []
      this.selected.map(m => {
        ids.push(m.id)
      })
      this.users = this.users.filter(f => !ids.includes(f.id))
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(json => this.users = json)
  }
}
</script>

<style>
.v-application--is-ltr .v-data-footer__select {
  margin-left: 10px;
}
.v-data-footer {
  border-top: 7px #7E7D7DFF solid !important;
}
</style>
