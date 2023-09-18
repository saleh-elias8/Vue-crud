
<template>
    <div class="container">
        <div class="card">
            <div class="card-header">
                <h4>
                    Students
                    <RouterLink to="/students/create" class="btn btn-primary">
                        Add Student
                    </RouterLink>
                </h4>
            </div>
            <div class="card-body">

                <div class="table-responsive">
                <table class="table align-middle table-bordered table-hover">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Course</th>
                            <th>Email</th>
                            <th>Phone</th>
                            <th>Created At</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody v-if="this.students.length > 0">
                        <tr v-for="(student, index) in this.students" :key="index">
                            <td>{{ student.id }}</td>
                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.created_at.substring(0,10) }}</td>
                            <td>
                                <RouterLink :to="{path:'/students/'+student.id+'/edit'}" class="btn btn-success mx-1 my-1">
                                    Edit
                                </RouterLink>
                                <button type="button" @click="deleteStudent(student.id)" class="btn btn-danger mx-1 my-1">
                                    Delete
                                </button>
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-else-if="this.students.length = 0">
                        <tr><td colspan='7'>No Students Found</td></tr>
                    </tbody>
                    <tbody v-else>
                        <tr><td colspan='7'>Loading...</td></tr>
                    </tbody>
                </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default{
    name: 'students',
    data() {
        return {
            students : []
        }
    },
    mounted(){
        this.getStudents();
        // console.log('I am here.')
    },
    methods:{
        getStudents(){
            axios.get('http://localhost:8000/api/students').then(res =>{
                this.students = res.data.students
                // console.log(this.students)
            });
        },
        //delete student
        deleteStudent (studentId){
            if(confirm('Are you sure , you want to delete this data?')){
                // console.log(studentId)
                axios.delete(`http://localhost:8000/api/students/${studentId}/delete`)
                    .then(res => {
                        alert(res.data.message);
                        this.getStudents();
                    })
                    .catch(function (error) {
                        if (error.response) {
                            if(error.response.status == 404){
                                alert(error.response.data.message);
                            }
                        }
                    });
                
            }
        },
    },
}
</script>