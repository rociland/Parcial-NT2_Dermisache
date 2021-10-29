<template>

  <section class="src-componentes-formulario">
     <div class="jumbotron mt-3">
    <h1>Ingrese sus datos</h1>
    <hr class="hr">
    <vue-form :state="formstate" @submit.prevent="enviar()">
       <!-- NOMBRE -->
        <validate tag="div">
          <label for = "nombre">Nombre </label>  
          <input   id="nombre"
                 name="nombre"
                 class="form-control"
                 autocomplete="off" 
                 v-model="formData.nombre" 
                 required 
                 :minlength="nombreMin"
                 :maxlength="nombreMax"
                 />
      
          <field-messages name="nombre" show ="$dirty">
              <div slot="required" class="alert alert-danger mt-2">Campo Obligatorio</div>
              <div slot="minlength" class="alert alert-danger mt-1">
                Debe ingresar al menos {{nombreMin}} caracteres
              </div>
              <div class="alert alert-warning mt-1">
                Este solo puede tener {{nombreMax}} caracteres
              </div>
            </field-messages>

        </validate>
        <br>
        <!-- DESCRIPCION -->
        <validate tag="div">
          <label for = "desc">Descripcion </label>  
          <input type="text" id ="desc" v-model="formData.desc" required name="desc" autocomplete="off" class ="form-control" />
      
          <field-messages name="desc" show ="$dirty">
              <div slot="required" class="alert alert-danger mt-2">Campo Obligatorio</div>
            </field-messages>
        </validate>
        <br>
        <!-- IMPORTE -->
        <validate tag="div">
          <label for = "importe">Importe </label>  
          <input type="number" id ="importe" v-model.number="formData.importe" required name="importe" autocomplete="off" class ="form-control" />
      
          <field-messages name="importe" show ="$dirty">
              <div slot="required" class="alert alert-danger mt-2">Campo Obligatorio</div>
            </field-messages>
        </validate>

        
      
        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
    </vue-form>
    <br>
    <h2>Datos Ingresados</h2>
    <hr>
    <div v-if="datos.length" class="table-responsive">
      <table class="table table-dark">
        <tr>
          <th>Nombre</th>
          <th>Descripcion</th>
          <th>Importe</th>
          <th>Fecha</th>
        </tr>
        <tr v-for="(dato, i) in datos" :key="i" >
          <td>{{dato.nombre}}</td>
          <td>{{dato.desc}}</td>
          <td>$ {{dato.importe}}</td>
          <td>{{dato.fecha}}</td>
        </tr>
        <tr :style="{color: calcular().color }">TOTAL: ${{calcular().total}}</tr>
      </table>
    </div>
    <div v-else>
      <h4 style="background-color: #40D0E0; padding: 15px">No hay datos a mostrar</h4>
    </div>

    </div>
  </section>

  

</template>

<script>

  export default  {
    name: 'src-componentes-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate: {},
        formData : this.getInitialData(),
        datos : [],
        nombreMin : 3,
        nombreMax : 15
      }
    },
    methods: {
      getInitialData(){
        return {
          nombre: null,
          desc: null,
          importe: null,
        }
      },

      enviar(){

      let dato = {...this.formData}
        dato.fecha = new Date().toLocaleString()

       console.log(this.formData)
        this.datos.push(this.formData)
        this.formData =this.getInitialData()
        this.formstate._reset()
        
      },
      calcular() {
        
        
        
        var total = 0;
        this.datos.forEach(i => {
          total += i.importe
        });
        let color = '#080'
        if(total < 1000) color = '#39FF33 '
        if(total > 5000) color = '#ff00ff'
        else color = '#F39D28'
        return{total, color}
      

        
      }
        
    },
    computed: {
      
    }
}


</script>

<style scoped lang="css">
  .src-componentes-formulario {
   
  }
   .jumbotron {
      background-color: rgb(194, 228, 248);
    }
    .hr{
      background-color: black;
    }
  
</style>
