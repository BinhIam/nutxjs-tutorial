<style>
  table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }

  button {
    border-radius: 5px;
    padding: 10px;
    margin: 5px;
    min-width: 100px;
  }

  .btn-primary {
    background-color: rgb(59, 139, 230);
  }

  .btn-success {
    background-color: rgb(45, 226, 0);
  }

  .btn-danger {
    background-color: rgba(255, 0, 0, 0.651);
  }

  #create-btn {
    background-color: rgb(1, 255, 56);
    float: right;
  }
</style>

<template>
  <div class="container" data-app>
    <StudentModal 
      v-if="viewModal"
      :action="action"
      :extra_data="extra_data"
      @closeModal="dialog => viewModal = dialog"
    />
    <div id="head-create">
      <button id="create-btn" @click="openModal('add')">New Student</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Email</th>
          <th>Class</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr 
          v-for="student in students"
          key="student.id"
        >
          <td>{{student.name}}</td>
          <td>{{student.age}}</td>
          <td>{{student.email}}</td>
          <td>{{student.class.name}}</td>
          <td>
            <button class="btn-primary" @click="openModal('view', student.id)">
              View
            </button>
            <button class="btn-success" @click="openModal('edit', student.id)">
              Edit
            </button>
            <button class="btn-danger" @click="deleteStudent(student.id)">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  auth: false,
  name: "IndexPage",
  data: () => ({
    viewModal: false,
    students: [
      {
        id: 1,
        name: "huong",
        age: 25,
        email: "huong@gmail.com",
        class: { id: 1, name: "class 1" }
      }
    ],
    action: "",
    extra_data: {}
  }),
  methods: {
    getStudents() {
      this.$axios.get("/students").then(response => {
          this.students = response.data;
      });
    },

    deleteStudent(id) {
      this.$axios.delete(`${"/students/" + id}`)
    },

    openModal(type, id=null) {
      this.viewModal = true;
      this.action = type;
      if (type === 'view') {
        this.extra_data.id = id
      }
    }
  },
  mounted() {
      // this.getStudents();
  },
}
</script>
