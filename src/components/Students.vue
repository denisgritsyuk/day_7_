<template>
    <div>  
        <table>
                <tr>
                    <th>Photo</th>
                    <th>Name</th>
                    <th>Group</th>
                    <th>Mark</th>
                    <th>PR is Done</th>
                    
                </tr>
                <tr v-for="stud in students" v-bind:key="stud._id" >
                    <tr v-for="stud in students" v-bind:key="stud._id">
                    <td><img v-bind:src="stud.photo" width="50px"></td>
                    <td v-bind:style="stud.name.indexOf(search)>-1 && search.length >0 ? 'background-color:#CA2C2C' : 'background-color:#fff'" ><router-link v-bind:to="'/student-info/'+stud._id">
{{stud.name}}
</router-link></td>

                        <td>{{stud.group}}</td>
                    <td>{{stud.mark}}</td>
                    <td><input type="checkbox" v-bind:checked="stud.isDonePr"></td>
                     <td><a href="#" v-on:click.prevent="deletes(stud._id)" v-show="stud.group==getCurrentUser.group">Видалити</a></td>
                    <td><button v-on:click="getData(stud._id,stud.name,stud.group,stud.isDonePr)"><img src="components/Pencil.svg.png" width="25px"></button></td>
                </tr>
            </table><h1>{{reload}}</h1>
               <p><h3>Add new student</h3>
            <br>Input Name: <input v-model="name">
            <br> Input Group:
             <select name="group" v-model="group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="isDonePr">
            <br><button v-on:click="addStudent">Add student</button>

            <p><h3>Update student</h3>
            <br>Input Name: <input v-model="newStudent.name">
            <br> Input Group: 
            <select name="group" v-model="newStudent.group">
                <option value="RPZ 18 1/9">RPZ 18 1/9</option>
                <option value="RPZ 18 2/9">RPZ 18 2/9</option>
            </select>
            <br> Pr Is Done: <input type="checkbox" v-model="newStudent.isDonePr">
            <br><button v-on:click="updateStudent()" v-bind:style="showInput ? 'display:inline' : 'display:none'">Update student</button>


             

             
   </div>
   
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

   export default {
    data: function() {
           return {
            students: [],
            currency:[],
            search:"",
            start_ccy:"",
            end_ccy:"",
            sell:0,
            buy:0,
            start_value:0,
            end_value:0,
            result:"",
             newStudent: {},
            search:"",
            result:"",
            name:"",
            group:"",
            mark:0,
            isDonePr:false,
            names:"",
            groups:"",
            isDonePrs:false,
            names:"",
            groups:"",
            isDonePrs:false,
            studId:"",
             showInput:false,
            idTest:"",
            marks:0,
            
        
        }},
    
    mounted: function(){
        
       axios.get("http://46.101.212.195:3000/students").then((response)=>{
            console.log(response.data);
            this.students = response.data;
            this.$store.commit('setCount',this.students.length);
        })
        axios.get("https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5").then((response)=>{
            console.log(response.data);
            this.currency = response.data;
        })
    },
     deletes:function(id){ 
         Vue.axios.delete("http://46.101.212.195:3000/students/"+id)  
         .then((response)=> { 
 
              
            axios.get("http://46.101.212.195:3000/students").then((response)=>{ 
                this.students = response.data; 
                this.$store.commit('setCount', this.students.length) 
            }) 
            })
        },
    computed:{
        getCount(){
            return this.$store.getters.getCount
        },
        
        getCurrentUser () {
                return this.$store.getters.getUser
            }
    },
    
    
    methods:{
        deleteStudent:function(id){ 
         Vue.axios.delete("http://46.101.212.195:3000/students/"+id)  
         .then((response)=> { 
 
              
            axios.get("http://46.101.212.195:3000/students").then((response)=>{ 
                this.students = response.data; 
                this.$store.commit('setCount', this.students.length) 
            }) 
            })
        },
        
         getData: function(id,name,group,isDone,mark){
             this.newStudent.id = id;
            this.newStudent.name = name;
            this.newStudent.group = group;
            this.newStudent.isDonePr = isDone;
            this.newStudent.mark = mark;
            this.studId = id;
            this.idTest = id;
            this.showInput = true;
        },
       updateStudent:function(){
            Vue.axios.put("http://46.101.212.195:3000/students/"+this.studId, {
                name: this.newStudent.name,
                group: this.newStudent.group,
                isDonePr: this.newStudent.isDonePr,
                mark:this.newStudent.mark,
               
            })
            axios.get("http://46.101.212.195:3000/students").then((response)=>{
                this.students = response.data;
            })
            this.idTest = "";
        },
        
    addStudent:function(){  
            Vue.axios.post("http://46.101.212.195:3000/students", {  
                 
                name: this.name,  
                group: this.group,  
                isDonePr: this.isDonePr,  
                mark:this.mark, 
            })  
            .then((response) => {  
 
                console.log(response.data) 
                axios.get("http://46.101.212.195:3000/students").then((response)=>{  
                this.students = response.data;  
                this.$store.commit('setCount', this.students.length) 
            })  
            })  
            
              
        },
        },

    filters:{
            roundValue: function(value){
                return parseFloat(value.toFixed(2));
            },
        },
    }
    
    
 
    
</script>