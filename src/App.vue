<template>
  <div id="app" class="small-container">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <h1>Employees</h1>
    <employee-form @add:employee="addEmployee"/>
    <employee-table 
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
  // import HelloWorld from './components/HelloWorld.vue'
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'App',
    components: {
      // HelloWorld
      EmployeeTable,
      EmployeeForm
    },
    data() { //data is like react state
      return {
        employees: [
          // {
          // id: 1,
          // name: 'Richard Hendricks',
          // email: 'richard@piedpiper.com',
          // },
          // {
          //   id: 2,
          //   name: 'Bertram Gilfoyle',
          //   email: 'gilfoyle@piedpiper.com',
          // },
          // {
          //   id: 3,
          //   name: 'Dinesh Chugtai',
          //   email: 'dinesh@piedpiper.com',
          // },
        ]
      }
    },
    mounted() {
      this.getEmployees()
    },
    methods: {
      // addEmployee(employee) {
      //   const lastId =
      //     this.employees.length > 0
      //       ? this.employees[this.employees.length - 1].id
      //       : 0;
      //   const id = lastId + 1;
      //   const newEmployee = { ...employee, id };

      //   this.employees = [...this.employees, newEmployee];
      // },
      // deleteEmployee(id) {
      //   console.log("del")
      //   this.employees = this.employees.filter(
      //     employee => employee.id !== id
      //   )
      // },
      // editEmployee(id, updatedEmployee) {
      //   this.employees = this.employees.map(employee =>
      //     employee.id === id ? updatedEmployee : employee
      //   )
      // }
      async getEmployees() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch (error) {
          console.error(error)
        }
      },

      async addEmployee(employee) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = [...this.employees, data]
        } catch (error) {
          console.error(error)
        }
      },

      async editEmployee(id, updatedEmployee) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
        } catch (error) {
          console.error(error)
        }
      },

      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
          });
          this.employees = this.employees.filter(employee => employee.id !== id);
        } catch (error) {
          console.error(error);
        }
      }
    }
  }
</script>

<style>
  /* #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  } */
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  button:hover,
  button:active,
  button:focus {
    background: #32a95d;
    border: 1px solid #32a95d;
  }

  .small-container {
    max-width: 680px;
  }
</style>
