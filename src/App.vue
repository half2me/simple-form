<template lang="pug">
  q-layout(view="hHh lpR fFf")
    q-page-container
      .q-pa-md
        q-form(@submit="onSubmit" @reset="onReset" ref="myForm").q-gutter-md
          q-input(v-model="newRow.name" label="Name" required lazy-rules :rules="[ val => val && val.length > 0 || 'Please type your name']")
          q-input(v-model="newRow.dob" label="Date of Birth" required type="date" lazy-rules)
          q-input(v-model="newRow.email" label="Email" required type="email")
          q-input(v-model.number="newRow.noc" label="Number of Children" required type="number" lazy-rules :rules="[ val => val && val >= 0 || 'Please type a valid number']")
          div
            q-btn(label="Submit" type="submit" color="primary")
            q-btn(label="Reset" type="reset" color="primary" flat).q-ml-sm
        hr
        q-table(title="People" :data="people" :columns="columns" row-key="id" :pagination="pagination")
          template(v-slot:top-right)
            q-input(borderless dense debounce="300" v-model="search" placeholder="Search")
              template(v-slot:append)
                q-icon(name="search")
</template>

<script>

import axios from 'axios'

export default {
  name: 'LayoutDefault',

  data () {
    return {
      newRow: {
        name: '',
        dob: '',
        email: '',
        noc: 0,
      },
      search: '',
      pagination: {
        sortBy: 'name',
        descending: false,
        page: 1,
        rowsPerPage: 15,
      },
      columns: [
        {
          name: 'name',
          required: true,
          label: 'Name',
          align: 'left',
          field: row => row.name,
          sortable: true,
        },
        {
          name: 'dob',
          required: true,
          label: 'Date of Birth',
          sortable: true,
          field: row => row.dob,
        },
        {
          name: 'email',
          required: true,
          label: 'Email',
          sortable: true,
          field: row => row.email,
        },
        {
          name: 'noc',
          required: true,
          label: 'Number of Children',
          sortable: true,
          field: row => row.noc,
        },
      ],
    }
  },

  asyncComputed: {
    people: {
      get() {
        return axios.get('http://127.0.0.1:8000/', {
          params: {
            page: this.pagination.page,
            size: this.pagination.rowsPerPage,
            order_by: this.pagination.order_by,
            dir: this.pagination.descending ? 'desc' : 'asc',
          },
        }).then(d => d.data);
      },
      default() {
        return [];
      },
    },
  },

  methods: {
    onSubmit() {
      axios
        .post('http://127.0.0.1:8000/', this.newRow)
        .finally(() => {
          this.onReset();
          this.$refs.myForm.resetValidation();
          this.$asyncComputed.people.update();
        })
    },
    onReset() {
      this.newRow = {
        name: '',
        dob: '',
        email: '',
        noc: 0,
      };
    },
  },
}
</script>

<style>
</style>
