<template>
  <div class="home">
  	<div class="container" style="margin-top: 40px;">
  		<div class="row">
  			<div class="col">
  				<h2>Frontend Avatar</h2>
  			</div>
  		</div>
  	</div>
  	<div class="container" style="margin-top: 20px;">
  		<div class="row">
  			<div class="col-md-10 offset-md-1 text-right">
  				<!-- Button trigger modal -->
				<button type="button" class="btn btn-primary" v-on:click="abrir_modal(0,null)">
				  <i class="fas fa-plus-circle"></i> Nueva Persona
				</button>
  			</div>
  		</div>
  	</div>

  	<div class="container" style="margin-top: 40px;">
  		<div class="row">
  			<div class="col-md-10 offset-md-1">
  				<table class="table">
			  		<thead>
			  			<tr>
			  				<th>ID</th>
			  				<th>NAME</th>
			  				<th>HEIGHT</th>
			  				<th>MASS</th>
			  				<th>HAIR COLOR</th>
			  				<th>GENDER</th>
			  				<th>PLANET</th>
			  				<th></th>
			  				<th></th>
			  			</tr>
			  		</thead>
			  		<tbody>
			  			<tr v-for="person in list">
			  				<td>{{person.id}}</td>
			  				<td>{{person.name}}</td>
			  				<td>{{person.height}}</td>
			  				<td>{{person.mass}}</td>
			  				<td>{{person.hair_color}}</td>
			  				<td>{{person.gender}}</td>
			  				<td>{{person.planet}}</td>
			  				<td>
			  					<button v-on:click="abrir_modal(1,person.id)"><i class="fas fa-edit"></i> Editar</button>
			  				</td>
			  				<td>
			  					<button v-on:click="borrar_persona(person.id)"><i class="fas fa-trash"></i> Borrar</button>
			  				</td>
			  			</tr>
			  		</tbody>
			  	</table>
  			</div>
  		</div>
  	</div>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Agregar / Editar Persona</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <form>
      <div class="modal-body">
        		
				  <div class="form-group row">
				    <label for="name" class="col-sm-3 col-form-label text-left">Name</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="name" v-model="item.name">
				    </div>
				  </div>
				  <div class="form-group row">
				    <label for="height" class="col-sm-3 col-form-label text-left">Height</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="height" v-model="item.height">
				    </div>
				  </div>
				  <div class="form-group row">
				    <label for="mass" class="col-sm-3 col-form-label text-left">Mass</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="mass" v-model="item.mass">
				    </div>
				  </div>
				  <div class="form-group row">
				    <label for="hair_color" class="col-sm-3 col-form-label text-left">Hair Color</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="hair_color" v-model="item.hair_color">
				    </div>
				  </div>
				  <div class="form-group row">
				    <label for="gender" class="col-sm-3 col-form-label text-left">Gender</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="gender" v-model="item.gender">
				    </div>
				  </div>
				  <div class="form-group row">
				    <label for="planet" class="col-sm-3 col-form-label text-left">Planet</label>
				    <div class="col-sm-9">
				      <input type="text" class="form-control" id="planet" v-model="item.planet">
				    </div>
				  </div>
				  
				

				<div class="alert alert-warning alert-dismissible fade show" role="alert" v-if="show_alert1">
				  <strong>Error!</strong> {{mensaje_error}}
				  <button type="button" class="close" data-dismiss="alert" aria-label="Close">
				    <span aria-hidden="true">&times;</span>
				  </button>
				</div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="submit" v-on:click.prevent="guardar_persona()" class="btn btn-primary">Guardar</button>
      </div>
      </form>
    </div>
  </div>
</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "home",
  components: {

  },
  data() {
    return {
      list: null,
      item: {
      	id: null,
	    name: null,
	    height: null,
	    mass: null,
	    hair_color: null,
	    gender: null,
	    planet: null
      },
      mensaje_error: "",
      show_alert1: false,
    };
  },
  created() {
    this.listar();
  },
  methods: {
  	editar_persona: function(id) {
  		this.show_alert1 = false
  		axios.put("http://localhost:8080/person/edit/"+id, this.item).then(response => {
	      if (response.status == 200) {
	      	this.listar();	
	      }
	    }).catch(e => {
        	this.mensaje_error = "El nombre del planeta ingresado no existe";
        	this.show_alert1 = true;
        });
  	},
  	borrar_persona: function(id) {
  		axios.delete("http://localhost:8080/person/delete/"+id).then(response => {
	      if (response.status == 200) {
	      	this.listar();	
	      }
	    });
  	},
  	guardar_persona: function(){
  		this.show_alert1 = false
  		axios.post("http://localhost:8080/person/add", this.item).then(response => {
	      if (response.status == 201) {
	      	this.item = {};
	      	this.listar();
	      	$('#exampleModal').modal('hide')
	      }	      
	    }).catch(e => {
            console.log(e);
            this.mensaje_error = "El nombre del planeta ingresado no existe";
            this.show_alert1 = true;
        });
  	},
  	listar: function() {
  		axios.get("http://localhost:8080/person").then(response => {
	    	this.list=response.data;
	    });
  	},
  	obtener_persona: function(id) {
  		axios.get("http://localhost:8080/person/"+id).then(response => {
	    	this.item=response.data;
	    });
  	},
  	abrir_modal(obtener,id){
  		this.item = {};
  		this.show_alert1 = false;
  		if (obtener == 1) {
  			this.obtener_persona(id);
  		}
  		$('#exampleModal').modal('show')
  	}


  }
};
</script>
