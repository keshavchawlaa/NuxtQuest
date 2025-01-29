<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h3 style="text-align:center;">Students List
                </h3>
                <div class="container search">

                    <input
                    type="text"
                    placeholder="Search Student"
                    class="form-control"
                    style="max-width:60%;"
                    v-model="searchQuery"
                    @input="fetchStudentData"
                    />
                    <NuxtLink to="/students/create" class="btn btn-primary float-end addStudent" style=" ">Add Student</NuxtLink>
                </div>
            </div>
            <div class="card-body">
                <div class="table-responsive">
                    
                <div>
                    <table class="table table-striped table bordered">
                
                    <thead>
                        <tr>
                            <th>Sr.No</th>
                            <th>Name</th>
                            <th>COURSE</th>
                            <th>EMAIL</th>
                            <th>PHONE</th>
                            <th>CREATED AT</th>
                            <th class="sticky-column">ACTION</th>
                        </tr>
                    </thead>
                    <tbody>
                        <div v-if="isLoading">
                        <h4><Loading title="Loading"/></h4>
                    </div>
                    <tr v-else-if="students.length === 0">
                                <td colspan="7"><h4>No Students Found</h4></td>
                            </tr>
                        <tr v-else v-for="(student,index) in students" :key="index">
                            <td>{{ ++index }}</td>

                            <td>{{ student.name }}</td>
                            <td>{{ student.course }}</td>
                            <td>{{ student.email }}</td>
                            <td>{{ student.phone }}</td>
                            <td>{{ student.created_at }}</td>
                            <td>
                                <NuxtLink :to="`/students/${student.id}`" class="btn btn-success btn-sm mx-2">Edit</NuxtLink>
                                <Button type="button" @click="deleteStudent($event,student.id)" class="btn btn-danger btn-sm mx-2">Delete</Button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default{
    name:"student",
    data(){
        return {
            students:{},
            serialNo:1,
            isLoading:false,
            searchQuery: '',
        }
    },
    mounted(){
        this.getStudents();
    },
    methods: {
        getStudents(){
            this.isLoading=true;
            axios.get(`http://192.168.29.94:8000/api/student`).then(res => {
                this.students = res.data.students;
                this.isLoading=false;
            })  
        },

        deleteStudent(event,studentId){
            if(confirm('Are you sure you want to delete this data?')){
                event.target.innerText = 'Deleting..'
                axios.delete(`http://192.168.29.94:8000/api/student/${studentId}/delete`).then(res => {
                    event.target.innerText = 'Delete';
                    this.getStudents();
                })
            }
        },
        fetchStudentData() {
            if (this.searchQuery.length >= 2) {
                axios
                    .get(`http://192.168.29.94:8000/api/student/search/${this.searchQuery}`)
                    .then((response) => {
                        this.students = response.data.students; // ✅ Update list with search results
                    })
                    .catch((error) => {
                        console.error("Error fetching students:", error);
                        this.students = []; // ✅ Show "No Students Found" if error occurs
                    });
            } else {
                this.getStudents(); // ✅ Reset student list if search is cleared
            }
        },
    }}


</script>

<style>
.search{
    display:flex;
    flex-direction: row;
    justify-content: space-between;
}

.searchBox{
    width:40%;
}

.table-responsive {
    overflow-x: auto;
    max-width: 100%;
    white-space: nowrap;
}

.table th,
.table td {
    white-space: nowrap; /* Prevent text from wrapping */
}

.table th:last-child,
.table td:last-child {
    position: sticky;
    right: 0;
    background-color: white; /* Ensure it's visible */
    z-index: 2; /* Keep it above other content */
}

@media (max-width: 1384px) { /* ✅ Only apply on smaller screens */
    .table th:last-child,
    .table td:last-child {
        min-width: 120px; /* Adjust width as needed */
        box-shadow: -5px 6px 6px #b6b6b6;
    }
}


</style>