<template>
  <div id="app">

    <NewStudentForm v-on:student-added="newStudentAdded"></NewStudentForm>
    <StudentTable
      v-bind:students="students" 
      v-on:student-present="studentArrivedOrLeft"
      v-on:delete-student="studentDeleted">
    </StudentTable>
    <StudentMessage v-bind:student="mostRecentStudent"></StudentMessage>

  </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue';
import StudentMessage from './components/StudentMessage.vue';
import StudentTable from './components/StudentTable.vue';

export default {
  name: 'app',
  //emits:['student-added'], //Document the events this component emits
  data(){
    return{
      students:[
        // {name: 'Example', 'starID': 'abc123', present:true},
        // {name: 'Example2', 'starID': 'abc456', present:false},
      ],
      mostRecentStudent: {}
    }
  },
  components: {
    NewStudentForm,
    StudentTable,
    StudentMessage    
  },
  methods: {
    newStudentAdded(student){
      this.students.push(student)
      this.students.sort(function(s1, s2) {
        return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
      })
    },
    studentArrivedOrLeft(student, present) {
      //find student in array of students
      let updateStudent = this.students.find(function(s){
        if (s.name === student.name && s.starID === student.starID){
          return true
        }
      })

      if (updateStudent){
        updateStudent.present = present
        this.mostRecentStudent = student
      }
    },
    studentDeleted(student) {
      //filter returns a new array of all students for whom the function returns true
      this.students = this.students.filter(function(s){ return s != student })

      //clear welcome message
      this.mostRecentStudent={}
    }
  }
}
</script>

<style>

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";


</style>
