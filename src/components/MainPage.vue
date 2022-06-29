<template>
  <div class="main">
    <div class="input">
        <input type="text" v-model="search" @change="toggleSearch($event)" 
        placeholder="Digite o nome do usuario"/>

   </div>
    <div class="users">
      <table  class="content-table">
        <thead>
          <tr>
            <th>Name</th>
            <th>Gender</th>
            <th>Birth</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in searchName" :key="user.id.value
          ">
              <td>{{user.name.first}}</td>
              <td>{{user.gender}}</td>
              <td>{{user.registered.date.split("T")[0].split("-").reverse().join("/")}}</td>
              <td><img src="../assets/view.png" @click="() => handleViewUser(user,'open')"/></td>
          </tr>
        </tbody>

      </table>


      <img src="../assets/seta.png"  @click="nextPage"/>

    </div>
    <div class="popup" v-if="showUser">
    <div class="popup-inner">
      <img src="handleImg(userView)" />
      <p>Nome:{{userView.name.first}}  {{userView.name.last}}</p>
      <p>Email:{{userView.email}}</p>
      <p>Genero: {{userView.gender}}</p>
      <img src="../assets/close.png"  @click="handleViewUser"/>
      
    </div>
      

    </div>
  </div>
</template>

<script>
import axios from 'axios'


export default {
  name:'MainPage',
  data(){ 
    return {
      users:[],
      page:1,
      search:"",
      newUsers:[],
      showUser:false,
      userView:{}
    }
  
  },
  methods:{
    getUsers:function(){
     axios.get(`https://randomuser.me/api/?seed=f54749b11a4f1deb&nat=BR&page=${this.page}&results=10`).then(response => {
       this.users=response.data.results;
       console.log(response.data.results)
     }) 
    },
    nextPage:function() {
      this.page=this.page+1;
      this.getUsers();
    },
    toggleSearch: function() {
      console.log('entrou')
      axios.get(`https://randomuser.me/api/`).then(response => {
      this.users=response.data.results;
      console.log(response.data.results);
    })
    },
    handleViewUser: function(user,oc) {
      if(oc === 'open'){
      this.userView = user;
      return this.showUser = true;
      }
      this.showUser = false;
     
    },
    handleImg: function(user) {
      return user.picture.thumbnail
    }

      },
  computed:{
    searchName:function() {
    return this.users.filter((user) => user.name.first.toLowerCase().includes(this.search))
    }
 
    },
  beforeMount() {
    this.getUsers();
  }

}
</script>

<style>
.main{
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: rgb(219, 210, 198);
    padding-top: 100px;
}

img{
  width: 50px;
  padding-left: 20px;
}

.content-table {
  border-collapse: collapse;
  margin: 25px 0;
  width: 100%;
  font-size: 0.9em;
  font-family: 'Poppins';
  min-width: 400px;
  border-radius: 5px 5px 0 0;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}
.content-table thead tr {
  background-color: #dfe3e6;
  text-align: center;
  font-weight: bold;
}
.content-table th,
.content-table td {
  padding: 12px 15px;
}
.content-table tbody tr {
  border-bottom: 1px solid #dddddd;
}

.popup {
  position: fixed;
  flex-direction: column;
  align-items: center;
  width: 50%;
  top: 0;
  left:190px;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-inner {
  background: #fff;
  padding: 32px;
  display: flex;
}

</style>
