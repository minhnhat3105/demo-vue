<template>
  <div class="dashboard container">
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>Employees</h4>
      </li>
      <li
        v-for="employee in employees"
        :key="employee.id"
        class="collection-item"
      >
        <div class="chip">{{ employee.dept }}</div>
        {{ employee.employeeID }} : {{ employee.employeeName }}

        <router-link
          class="secondary-content"
          :to="{
            name: 'view-employee',
            params: { employee_id: employee.employeeID },
          }"
        >
          <i class="fa fa-eye"></i>
        </router-link>
      </li>
    </ul>

    <div class="fixed-action-btn">
      <router-link to="/new" class="btn-floating btn-large red">
        <i class="fa fa-plus"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from "./firebaseInit";
export default {
  name: "dashboard",
  data() {
    return {
      employees: [],
    };
  },
  created() {
    db.collection("employees")
      .orderBy("dept")
      .get()
      .then((querySnapshot) => {// querySnapshot is data after getting
        querySnapshot.forEach((doc) => {// doc is data for each element
          const data = {
            id: doc.id,
            employeeID: doc.data().employeeID,
            employeeName: doc.data().employeeName,
            dept: doc.data().dept,
            role: doc.data().role,
          };
          this.employees.push(data);
        });
      });
  },
};
</script>
