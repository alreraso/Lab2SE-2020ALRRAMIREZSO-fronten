<template>
  <div>
   ROLES
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: "Roles",
    data( ){
      return {
        password: '',
        role: '',
        roles: [],
      }
    },
    beforeCreate( ){
      const rolesPath = '/rola';
      axios
        .get( this.$store.state.backURL + rolesPath )
        .then( response => {
          if( response.status !== 200 ){
            alert( "Error en la petición. Intente nuevamente" )
          }else{
            this.roles = response.data;
          }
        }).catch( response => {
          alert( "No es posible conectar con el backend en este momento" );
        });
    },
    methods: {
      associate( event ){
        axios
          .post( this.buildURI( ), {
              password: this.password
            }, {
              params: {
                access_token: localStorage.getItem( "token" )
              }
            }
          ).then( response => {
            if( response.status !== 201 ){
              alert( "Error en la petición. Intente nuevamente" );
            }else{
              alert( "Se ha asignado exitosamente el nuevo rol" );
            }
          }).catch( response => {
            if( response.response.status === 401 ){
              alert( "¡Ups! Al parecer tu contraseña es incorrecta o la sesión ha finalizado" );
            }else if ( response.response.status === 400 ){
              alert( "¿Estás seguro de que aún no tienes ese rol asignado?" );
            }else{
              alert( "No es posible conectar con el backend en este momento" );
            }
          });
        event.preventDefault( );
      },
      buildURI( ){
        let associatePath = "/principal/roles/";
        return this.$store.state.backURL + associatePath + this.role;
      }
    }

  }
</script>

<style scoped>
  .form-inline .form-group{
    margin-bottom: 1rem;
  }
</style>