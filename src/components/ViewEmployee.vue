<template>
  <div class="view-employee container">
    <ul class="collection width-header">
      <li class="collection-header">
        <h4>{{ employeeName }}</h4>
      </li>
      <li class="collection-item">
        Employee ID#: {{ employeeID }}
      </li>
      <li class="collection-item">
        Department: {{ dept }}
      </li>
      <li class="collection-item">
        Role: {{ role }}
      </li>
    </ul>
    <router-link to='/' class="btn grey">Back</router-link>&ensp;
    <button @click="deleteEmployee" class="btn red">Delete</button>
    
    <div v-if="this.role == 'user'" class="fixed-action-btn">
      <router-link :to="{name: 'edit-employee',
      params: {employee_id: employeeID}}" class="btn-floating btn-large red">
        <i class="fas fa-pencil-alt"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from "./firebaseInit";
export default {
  name: "view-employee",
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
          next((vm) => {// access to component instance
            // vm stand for ViewModel
            vm.employeeID = doc.data().employeeID;
            vm.employeeName = doc.data().employeeName;
            vm.dept = doc.data().dept;
            vm.role = doc.data().role;
          });
        });
      });
  },
  watch: {// tracking data when they have some change
    '$route': "fetchData", // fetchData is function and take it into methods to clear meaning
  },
  methods: {
    fetchData() {
      db.collection("employees")
        .where("employeeID", "==", this.$route.params.employee_id)
        .get()
        .then(querySnapshot=>{
            querySnapshot.forEach((doc) => {
                this.employeeID = doc.data().employeeID;
                this.employeeName = doc.data().employeeName;
                this.dept = doc.data().dept;
                this.role = doc.data().role;
            })
        });
    },
    deleteEmployee(){
      if(confirm('Are you sure?')){
        db.collection("employees")
        .where("employeeID", "==", this.$route.params.employee_id)
        .get()
        .then(querySnapshot=>{
            querySnapshot.forEach((doc) => {
                doc.ref.delete(); // ref: back to firestore db
                this.$router.push('/');// from file index.js => given a route {path: '/'}
            })
        });
      }
    },
  },
};
</script>
