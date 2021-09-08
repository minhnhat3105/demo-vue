<template>
    <div class="new-employee">
        <h3>New Employee</h3>
        <div class="row">
            <form action="" class="col s12" @submit.prevent = "saveEmployee">
                <div class="row">
                    <div class="input-field col s12">
                        <input id="id" type="text" v-model="employeeID" required>
                        <label for="id">Employee ID#</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input id="name" type="text" v-model="employeeName" required>
                        <label for="name">Employee Name</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input id="dept" type="text" v-model="dept" required>
                        <label for="dept">Department</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input id="role" type="text" v-model="role" required>
                        <label for="role">Role</label>
                    </div>
                </div>
                <button type="submit" class="btn">Submit</button>&ensp;
                <router-link to="/" class="btn grey">Cancel</router-link>
            </form>
        </div>
    </div>
</template>

<script>
import db from './firebaseInit'
export default {
    name: 'new-employee',
    data () {
        return {
            employeeID: null,
            employeeName: null,
            dept: null,
            role: null
        }
    },
    methods:{
        saveEmployee(){
            db.collection("employees").add({
                employeeID: this.employeeID,
                employeeName: this.employeeName,
                dept: this.dept, 
                role: this.role
            }).then(this.$router.push('/'))//.catch(error => console.log(err))
        }
    },
}
</script>