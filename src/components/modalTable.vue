<template>


      <div class="modal fade bd-example-modal-lg" id="scheduleModal" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Entrevistas</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <div class="table-show">
            <table class="table" v-if="scheduled.length>0">
            <thead>
                <tr>
                <th scope="col">#</th>
                
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
                <tr v-for="item in scheduled" :key="item.id">
                <th scope="row">{{item.id}}</th>
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
                <td>{{item.website}}</td>
                <td>{{item.company.name}}</td>
                <td>{{item.company.catchPhrase}}</td>
                <td>{{item.company.bs}}</td>
                </tr>
            </tbody>
            </table>
            </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import { bus } from '../main'
export default {
    name: 'modalTable',
    data: function () {
        return {
            scheduled:[]
        }
    },  
    mounted() {
        var that=this;
        bus.$on('showSheduled', function (obj) {
            var objectToShow=that.drawTable=JSON.parse(JSON.stringify(obj));
            that.scheduled=[];
            objectToShow.find(function(fruta){
                if(fruta.schedule){
                    that.scheduled.push(fruta);
                }
            });
        });
    }
}
</script>
<style>
.table-show{
    overflow-x:scroll;
}

</style>