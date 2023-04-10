<template>
    <div class="container">
      
      <div v-if="errorMessage" class="alert alert-danger">{{ errorMessage }}</div>

      <form @submit.prevent="submitForm">
        <div class="form-group col-sm-4">
          <label for="id">Enter ID:</label>
          <input type="number" id="id" v-model="id" class="form-control" required>
          <button type="submit" class="btn btn-primary" style="margin: 0 5px">Submit</button>
        </div>
      </form>

      <div v-if="loading">
        <i class="fa fa-spinner fa-spin"></i> Loading...
      </div>

      <div v-if="data" class="form">
        <div class="form-group">
          <label for="courseCode">Código de curso:</label>
          <input type="text" id="courseCode" v-model="data.courseCode" readonly class="form-control">
        </div>

        <div class="form-group">
          <label for="fechaAlta">Fecha de alta:</label>
          <input type="text" id="fechaAlta" v-model="data.fechaAlta" readonly class="form-control">
        </div>
        
        <div class="form-group">
          <label for="colegioNombre">Colegio:</label>
          <input type="text" id="colegioNombre" v-model="data.colegioNombre" readonly class="form-control">
        </div>

        <div class="form-group">
          <label for="colegioNivel">Nivel:</label>
          <input type="text" id="colegioNivel" v-model="data.colegioNivel" readonly class="form-control">
        </div>

        <div class="form-group">
          <label for="colegioCurso">Curso:</label>
          <input type="text" id="colegioCurso" v-model="data.colegioCurso" readonly class="form-control">
        </div>

        <div class="form-group">
          <label for="colegioLocalidad">Localidad:</label>
          <input type="text" id="colegioLocalidad" v-model="data.colegioLocalidad" readonly class="form-control">
        </div>

        <div class="form-group" style="align-items: baseline;">
          <label for="order" style="margin-right: 10px;">Pedido:</label>
          <div id="order" style="white-space: nowrap; width: 90%;">
            <div v-for="orderDetail in data.orderDetails" :key="orderDetail.id" class="orderDetail-form">
              <label for="cantidadEgresados-{{ orderDetail.id }}">Cantidad de Egresados:</label>
              <input type="number" id="cantidadEgresados-{{ orderDetail.id }}" v-model="orderDetail.cantidadEgresados" readonly class="form-control">

              <label for="nombre-{{ orderDetail.id }}">Nombre:</label>
              <input type="text" id="nombre-{{ orderDetail.id }}" v-model="orderDetail.nombre" readonly class="form-control">

              <label for="precio-{{ orderDetail.id }}">Precio:</label>
              <input type="number" id="precio-{{ orderDetail.id }}" v-model="orderDetail.precio" readonly class="form-control">

              <label for="precioTotal-{{ orderDetail.id }}">Precio Total:</label>
              <input type="number" id="precioTotal-{{ orderDetail.id }}" v-model="orderDetail.precioTotal" readonly class="form-control">
            </div>
          </div>
        </div>

        <div class="form-group">
          <label for="total">Total:</label>
          <input type="number" id="total" v-model="data.total" readonly class="form-control">
        </div>

        <div class="form-group">
          <label for="fechaEntrega">Fecha de entrega:</label>
          <input type="text" id="fechaEntrega" v-model="data.fechaEntrega" readonly class="form-control">
        </div>
      </div>
    </div>
</template>
  
<script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        id: '',
        data: null,
        errorMessage: '',
        loading: false
      };
    },
    methods: {
      async submitForm() {
        this.loading = true; 

        // fake waiting time to test loading 
        await new Promise(resolve => setTimeout(resolve, 2000));
        try {
          const response = await axios.get(`https://localhost:7157/GetContract?Id=${this.id}`);
          this.data = response.data;
          this.errorMessage = '';
        } catch (error) {
          if (error.response && error.response.status === 404)
          {
            this.data = null;
            this.errorMessage = 'ID not found';
          }
          else
          {
            this.errorMessage = 'Error fetching data';
          }
        } finally {
          this.loading = false;
        }
      }
    }
  };
</script>

<style>
    .container {
    margin-left: auto;
    margin-right: auto;
    max-width: 600px;
    }

    .form-group {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    margin-bottom: 10px;
    }

    .orderDetail-form {
      display: flex;
      align-items: center;
    }

    label {
    margin-right: 10px;
    width: 200px;
    text-align: left;
    }

    input[type="text"],
    input[type="number"] {
    width: 100%;
    }

    input[readonly] {
    background-color: #eee;
    color: #888;
    }

    input[readonly]:focus {
    background-color: #eee;
    color: #888;
    }

</style>