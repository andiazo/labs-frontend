<template>
  <div class="col-12 col-sm-10 col-md-8 offset-sm-1 offset-md-2">
    <div class="mt-5">
      <form class="border border-primary rounded form-inline" @submit="create">

        <h2 class="col-12 text-center text-primary mt-3 mb-5">Crear un nuevo curso</h2>

        <div class="form-group col-12">
          <label for="text" class="custom-label col-md-3">Nombre del curso</label>
          <input id="courseName" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="text"
                 placeholder="Nombre del Curso" v-model="courseName" required/>
        </div>
        <div class="form-group col-12">
          <label for="number" class="custom-label col-md-3">Duración</label>
          <input id="durationHours" class="form-control col-12 col-sm-10 col-md-7 offset-sm-1" type="number"
                 placeholder="Duración del curso en horas" v-model="durationHours" required/>
        </div>


        <div class="col-12 mb-3">
          <button class="col-sm-6 col-md-4 offset-sm-5 offset-md-7 btn btn-primary" type="submit">
            Crear curso
          </button>
        </div>

      </form>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import {getAuthenticationToken} from '@/dataStorage';

  export default {
    name: "AddCourse",
    data( ){
      return {
        courseName: '',
        durationHours: 0
      }
    },
    methods: {
      create( event ){
        axios
          .post( this.buildURI( ), {
              courseName: this.courseName,
              durationHours: this.durationHours
            }, {
              params: {
                access_token: getAuthenticationToken( )
              }
            }
          ).then( response => {
            if( response.status !== 201 ){
              alert( "Error en la petición. Intente nuevamente" );
            }else{
              alert( "Se ha creado el curso exitosamente" );
            }
          }).catch( response => {
            if( response.response.status === 401 ){
              alert( "¡Ups! la sesión ha finalizado" );
            }else{
              alert( "No es posible conectar con el backend en este momento" );
            }
          });
        event.preventDefault( );
      },
      buildURI( ){
        let createPath = "/profesor/crear-curso/";
        return this.$store.state.backURL + createPath;
      }
    }

  }
</script>

<style scoped>
  .form-inline .form-group{
    margin-bottom: 1rem;
  }
</style>
