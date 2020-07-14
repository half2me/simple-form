<template lang="pug">
  q-layout(view="hHh lpR fFf")
    q-page-container
      q-form(@submit="onSubmit" @reset="onReset").q-gutter-md
        q-input(v-model="newItem.name" label="Name" required lazy-rules :rules="[ val => val && val.length > 0 || 'Please type your name']")
        q-input(v-model="newItem.dob" label="Date of Birth" required type="date" lazy-rules)
        q-input(v-model="newItem.email" label="Email" required type="email")
        q-input(v-model="newItem.noc" label="Number of Children" required type="number" lazy-rules :rules="[ val => val && val >= 0 || 'Please type a valid number']")
        div
          q-btn(label="Submit" type="submit" color="primary")
          q-btn(label="Reset" type="reset" color="primary" flat).q-ml-sm
      hr
      q-pa-md
        q-table(title="Rows" :data="rows" :columns="columns" row-key="id")
          template(v-slot:top-right)
            q-input(borderless dense debounce="300" v-model="search" placeholder="Search")
              template(v-slot:append)
                q-icon(name="search")
</template>

<script>

export default {
  name: 'LayoutDefault',

  data () {
    return {
      newItem: {
        name: '',
        dob: '',
        email: '',
        noc: 0,
      },
      search: '',
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
      rows: [
        { id: 1, name: 'Testy Tester', dob: '1993 06 11', email: 'yolo@gmail.com', noc: 2 },
        { id: 2, name: 'Testy Tester1', dob: '1993 06 11', email: 'yolo@gmail.com', noc: 2 },
      ],
    }
  },

  methods: {
    onSubmit() {
    },
    onReset() {
      this.newItem = {
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
