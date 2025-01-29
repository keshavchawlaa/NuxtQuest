<template>
    <div class="mt-5 container">
       <div class="card">
            <div class="card-header">
                <h4>
                    <NuxtLink to="/" class="btn float-end" style="margin-left:10px; background-color:darkslateblue; color:white">Home</NuxtLink>
                    <NuxtLink to="/students" class="btn float-end" style="background-color:darkslateblue; color:white">Students</NuxtLink>
                </h4>
                <h4>Edit Student</h4>
            </div>
            <div class="card-body">
                <div v-if="isLoading">
                    <Loading :title="isLoadingTitle" />
                </div>
                <div v-else>
                    <form @submit.prevent="updateStudent">
                    <div class="mb-3">
                        <label for="">Name</label>
                        <input type="text" v-model="student.name" class="form-control">
                        <span class="text-danger" v-if='errorList.name'>{{ this.errorList.name?.[0] }}</span>

                    </div>
                    <div class="mb-3">
                        <label for="">Course</label>
                        <input type="text" v-model="student.course" class="form-control">
                        <span v-if='errorList.course' class="text-danger">{{ this.errorList.course?.[0] }}</span>
                    </div>
                    <div class="mb-3">
                        <label for="">Email</label>
                        <input type="text" v-model="student.email" class="form-control">
                        <span class="text-danger" v-if='errorList.email'>{{ this.errorList.email?.[0] }}</span>
                    </div>
                    <div class="mb-3">
                        <label for="">Phone</label>
                        <input type="text" v-model="student.phone" class="form-control">
                        <span class="text-danger" v-if='errorList.phone'>{{ this.errorList.phone?.[0] }}</span>
                    </div>
                    <div class="mb3">
                        <button type="submit" class="btn btn-primary">Update</button>
                    </div>
                    </form>
            </div>
            </div>
       </div>
    </div>
</template>

<script >
import axios from 'axios';

export default{
    name:'studentEdit',
    data(){
        return{
            studentId:'',
            student: {},
            isLoading:false,
            isLoadingTitle:'Loading...',
            errorList:{}
        }
    },
    mounted(){
        this.studentId = this.$route.params.id;

        this.getStudent(this.studentId);

    },
    methods:{
        getStudent(studentId){
            axios.get(`http://192.168.29.94:8000/api/student/${studentId}/edit`).then(res => {
                console.log(res,'res')
                this.student= res.data.student;
            })
        },
        updateStudent(){
            this.isLoading=true;
            this.isLoadingTitle="Saving";
            // alert('hi')
            var myThis = this;
            axios.put(`http://192.168.29.94:8000/api/student/${this.studentId}/edit`, this.student).then(res => {

                console.log(res,'res')
                alert(res.data.message)

                this.isLoading=false;
                this.isLoadingTitle="Loading..."
                this.errorList={}


            })
            .catch(function(error){
                console.log(error,'errors')
                if(error.response.status == 422){
                    myThis.errorList = error.response.data.errors;
                }
                myThis.isLoading = false;
            })
        }
    }
}

</script>

<style>

</style>