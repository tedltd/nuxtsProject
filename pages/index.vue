<template>     

<div>
	<table class="table" v-if="nonEditMode === true">
                <thead>
                          <tr>
                            <th>Action</th>
                            <th>Name</th>
                            <th>Email</th>
							<th>company Name</th>
                          </tr>
                 </thead>
				 <tbody>
						<tr v-for="user in users">
						    <v-btn color="info" v-on:click="editUser(user.id)">Edit</v-btn>
                            <td><label>{{user.name}}</label></td>
                            <td><label>{{user.email}}</label></td>
							<td><label>{{user.company.name}}</label></td>
						</tr>

				 </tbody>
	</table>
	<div v-if="nonEditMode === false" >
	<v-layout>
        <v-flex xs12 md4 >
			<v-text-field v-model="user.name" label="Name" required></v-text-field>
		</v-flex>
		 <v-flex xs12 md4 >
			<v-text-field v-model="user.email" label="Email" required></v-text-field>
		</v-flex>
		 <v-flex xs12 md4 >
			<v-text-field v-model="user.company.name" label="Company Name" required></v-text-field>
		</v-flex>
	</v-layout>
		
		<v-btn color="success" v-on:click="saveUser(user.id)">Save</v-btn>
		<v-btn color="error" v-on:click="getUsers()">Cancel</v-btn>
		</div>	
		<label >{{savedMessage}}</label>
	</div>  
					
						
</template>

<script>
import axios from 'axios';
export default {

    data: function () {
        return {
          users: [], 
		  user: [],
		  nonEditMode: true,
		  editIndex : 2,
		  savedMessage : '',
		  returnedUser: []
		  
		  
        }
    },
	mounted() {
           this.getUsers();         
        },
	methods: {
		editUser: function(userId) {
		     this.savedMessage = '';
			 this.nonEditMode = false;
			 axios.get('https://jsonplaceholder.typicode.com/users/' + userId).then(response => {
             this.user = response.data
          });
		},
		getUsers: function() {
		    this.nonEditMode = true;
			
		    axios.get('https://jsonplaceholder.typicode.com/users').then(response => {
            this.users = response.data
			});
		}, 
		saveUser: function() {
		    const config = { headers: {'Content-Type': 'application/json'} };
			axios.put('https://jsonplaceholder.typicode.com/users/' + this.user.id, this.user, config).then(response => {
            if (response.status === 200) {
              this.savedMessage = 'User ' + this.user.id + ' has been updated successfully.'
			  } else {
			  this.savedMessage = 'User ' + this.user.id + ' has failed to be updated.'}
			
			});
					 
			this.getUsers();
			}
		
	},
	computed: {
	
	}

   

}
</script>
