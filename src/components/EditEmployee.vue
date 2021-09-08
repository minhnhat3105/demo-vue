<template>
  <div class="edit-employee container">
    <h3>Edit Employee</h3>
    <div class="row">
      <form action="" class="col s12" @submit.prevent="updateEmployee">
        <div class="row">
          <div class="input-field col s12">
            <input disabled id="id" type="text" v-model="employeeID" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input id="name" type="text" v-model="employeeName" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input id="dept" type="text" v-model="dept" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input id="role" type="text" v-model="role" required />
          </div>
        </div>
        <button type="submit" class="btn">Submit</button>&ensp;
        <router-link to="/" class="btn grey">Cancel</router-link>
      </form>
    </div>
  </div>
</template>

<script>
import db from "./firebaseInit";
export default {
  name: "edit-employee",
  data() {
    return {
      employeeID: null,
      employeeName: null,
      dept: null,
      role: null,
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("employees")
      .where("employeeID", "==", to.params.employee_id)
      .get()
      .then((querySnapshot) => {
        querySnapshot.forEach((doc) => {
          next((vm) => {
            vm.employeeID = doc.data().employeeID;
            vm.employeeName = doc.data().employeeName;
            vm.dept = doc.data().dept;
            vm.role = doc.data().role;
          });
        });
      });
  },
  watch: {
    $route: "fetchData",
  },
  methods: {
    fetchData() {
      db.collection("employees")
        .where("employeeID", "==", this.$route.params.employeeID)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forearch((doc) => {
            this.employeeID = doc.data().employeeID;
            this.employeeName = doc.data().employeeName;
            this.dept = doc.data().dept;
            this.role = doc.data().role;
          });
        });
    },
    updateEmployee() {
      db.collection("employees")
        .where("employeeID", "==", this.$route.params.employee_id)
        .get()
        .then((querySnapshot) => {
          querySnapshot.forEach((doc) => {
            doc.ref.update({
              employeeID: this.employeeID,
              employeeName: this.employeeName,
              dept: this.dept,
              role: this.role,
            }).then(() => {
              this.$router.push({name: 'view-employee', params:{employee_id: this.employeeID}});
            })
          })
        });
    },
  },
};
</script>
