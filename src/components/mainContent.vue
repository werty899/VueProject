<template>
  <div class="hello">
    <div class="container">
      <h2>Proceso de selección</h2>  

      <div v-if="tableTecno">
    <form>
      <div class="form-group">
        <label for="exampleFormControlSelect1">Seleccione tecnología</label>
        <select v-model="tecnology" class="form-control" id="tec">
          <option>Seleccione </option>
          <option value="net">Microsot .Net </option>
          <option value="java">Oracle  Java </option>
        </select>
      </div>
    </form>
       <table class="table" v-if="tecnology">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th v-if="net" scope="col">.Net</th>
          <th v-if="java" scope="col">JAVA</th>   
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td v-if="net"><a href="javascript:;" @click="getDataRest(1)">Asp.Net</a></td>
          <td v-if="java" ><a href="javascript:;" @click="getDataRest(1)">Java Server Pages</a></td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td v-if="net"><a href="javascript:;" @click="getDataRest()">MVVM</a></td>
          <td v-if="java" ><a href="javascript:;" @click="getDataRest()">Java Server Faces</a></td>
        </tr>
        <tr>
          <th scope="row">3</th>
          <td v-if="net"><a href="javascript:;" @click="getDataRest(1)">Ado.Net</a></td>
          <td v-if="java" ><a href="javascript:;" @click="getDataRest(1)">Enterprise Java Beans</a></td>
        </tr>
        <tr>
          <th scope="row">4</th>
          <td v-if="net"><a href="javascript:;" @click="getDataRest()">Entity FrameWork</a></td>
          <td v-if="java" ><a href="javascript:;" @click="getDataRest()">Java Persistence Api</a></td>
        </tr>
        <tr>
          <th scope="row">5</th>
          <td v-if="net" ><a href="javascript:;" @click="getDataRest(1)">LinQ</a></td>
          <td v-if="java" ><a href="javascript:;" @click="getDataRest(1)">Java Messaging Services</a></td>
        </tr>
      
      </tbody>
    </table>
    </div>

    <hr>


    <h4 v-if="drawTable.length>0" id="candidatos">Listado de candidatos</h4>  
    <div v-if="drawTable.length>0" class="tabla-candidatos">
    <table class="table" >
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col"></th>
          <th scope="col">Entrevista</th>
          <th scope="col">Nombre</th>
          <th scope="col">Usuario</th>   
          <th scope="col">Dirección</th>   
          <th scope="col">Telefono</th>   
          <th scope="col">Página Web</th>   
          <th scope="col">Compañia</th>   
          <th scope="col">Cargo</th>   
          <th scope="col">Funciones</th>   
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in drawTable" :key="item.id">
          <th scope="row">{{item.id}}</th>
          <td><button @click="scheduler(item.id)">Programar</button></td>
          <td>
            <div v-if="item.schedule">
              Modalidad:{{item.schedule.forma}} <br>   
              Fecha y Hora:{{item.schedule.fecha}} <br>   
            </div>
            <div v-else>
              No Disponible
            </div>
          </td>
          <td>{{item.name}}</td>
          <td>{{item.username}}</td>
          <td>street:{{item.address.street}},suite:{{item.address.suite}},city:{{item.address.city}},zipcode:{{item.address.zipcode}}</td>
          <td>{{item.phone}}</td>
          <td><a :href="item.website" target="_blank">{{item.website}}</a></td>
          <td>{{item.company.name}}</td>
          <td>{{item.company.catchPhrase}}</td>
          <td>{{item.company.bs}}</td>
        </tr>
      </tbody>
    </table>
    </div>

  </div>
  <modal :id="idModal"></modal> 
  <modal-table></modal-table> 

  </div>
</template>

<script>
import { bus } from '../main'
import modal from '../components/modal.vue'
import modalTable from '../components/modalTable.vue'
var moment=require('moment');
export default {
  name: 'mainContent',
   components: {
    modal,
    modalTable
  },
  data: function () {
    return {
      tecnology: '',
      dataRest:[],
      idModal:null,
      drawTable:[],
      sendTableScheduled:[],
      odd:null,
      tableTecno:true
    }
  },
  computed: {
    java: function () {
      return this.tecnology=="java"?true:false;
    },
    net: function () {
      return this.tecnology=="net"?true:false;
    },
  },
  methods: {
    getOdd: function (val,parImpar) {
      var arr=[];
      val.filter(function(person){
        if(parImpar){
          if(person.id%2==1){
            arr.push(person);
          }
        }else{
          if(person.id%2==0){
            arr.push(person);
          }
        }
      });  
      return arr;
    },
    getDataRest: function (parImpar) {
      this.odd=parImpar!=null;
      this.tableTecno=false;
      var that=this;
      
      fetch("http://jsonplaceholder.typicode.com/users").then((resp)=>{
          return resp.json();
      }).then((resp)=>{
          that.dataRest=resp;
          that.drawTable=JSON.parse(JSON.stringify(that.getOdd(that.dataRest,that.odd)));
      })
    },
    scheduler: function (id) {
      this.idModal=id;
      $('#exampleModal').modal('show');
    },
  },
  mounted() {
    var that=this;
    bus.$on('dataS', function (obj) {
      var str = obj.fecha;//date exist
      var n = str.indexOf("null");

      if(n==-1){
          let valida=true;
          that.sendTableScheduled = null;
          that.dataRest.find(function(person,index){
            if(person.id == obj.id){
                let validateArr=JSON.parse(JSON.stringify(that.dataRest));
                validateArr[index].schedule=obj;
                
                let hasMeeting=[];
                validateArr.find(function(person){
                  if(person.schedule){
                      hasMeeting.push("meeting");
                  }
                });
                
                if(hasMeeting.length>1){
                  validateArr.find(function(person){
                      if(person.schedule){
                        var llega = moment(obj.fecha);
                        var b = moment(person.schedule.fecha);
                        var dif = llega.diff(b, 'minutes'); 
                        if(Math.abs(dif)<59 && obj.id!=person.id){
                          alert("La fecha ya no esta disponible");
                          valida=false;
                        }
                      }
                    });
                }

                if(valida){
                  that.dataRest[index].schedule=obj;
                }
            }
          });
          if(valida){
            bus.$emit('showSheduled', that.dataRest);
            that.drawTable=[];
            that.drawTable=that.getOdd(that.dataRest,that.odd);
            $('#exampleModal').modal('hide');
          }
    }else{
        alert("Complete los campos");
    }
      });
    }
}
</script>

<style scoped>
.tabla-candidatos{
      overflow-x: scroll;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
