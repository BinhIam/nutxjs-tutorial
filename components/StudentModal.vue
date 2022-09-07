<template>
  <v-dialog
    v-model="dialog"
    persistent
    light
    width="500px"
  >
    <v-card>
      <v-card-title>
        <span class="text-h5">Add Student</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field
                label="Student Name"
                v-model="studentName"
                disabled="disabled"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="Age"
                v-model="studentAge"
                disabled="disabled"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="Email"
                v-model="studentEmail"
                disabled="disabled"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="Class"
                v-model="studentClass"
                disabled="disabled"
                required
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="blue darken-1"
          text
          @click="closeModal"
        >
          Close
        </v-btn>
        <v-btn
          color="blue darken-1"
          text
          @click="submit"
        >
          Save
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
  export default {
    name: "StudentModal",
    props: {
      action: String,
      extra_data: Object
    },
    emits: ['closeModal'],
    data: () => ({
      studentName: '',
      studentAge: '',
      studentEmail: '',
      studentClass: '',
      dialog: true,
      disabled: false,
    }),
    methods: {
      submit() {
        console.log(this.action)
        if (this.action === 'add') {
          this.$axios.post("/students", {
            name: this.studentName,
            age: this.studentAge,
            email: this.studentEmail,
            class: this.studentClass
          });

          this.openModal = false;
          this.studentName = '',
          this.studentAge = '',
          this.studentEmail = '',
          this.studentClass = ''
        }
        this.$emit('closeModal', false)
      },
      closeModal() {
        this.$emit('closeModal', false)
      },
      retrieveStudent(id) {
        this.$axios.get(`${'/students/' + id}`).then(response => {
          let student = response.data
          this.studentName = student.name
          this.studentAge = student.age
          this.studentEmail = student.email
          this.studentClass = student.class.name
        })
      }
    },
    mounted() {
      this.disabled = false
      if (this.action === 'view') {
        this.retrieveStudent(this.extra_data.id);
        this.disabled = true;
      }
    }
  }
</script>