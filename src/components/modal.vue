<template>
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Participante {{id}}</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
      <form>
      <div class="form-group form-hora">
        <label>Fecha: </label>
        <input required type="date" v-model="date">   
        <label> Hora:</label>
        <input required type="number" placeholder="18" v-model="hour">:<input required type="number"  placeholder="59" v-model="minute">   
      </div>
      <div class="form-group">
        <input type="radio" id="presencial" value="presencial" v-model="picked">
        <label for="presencial">Presencial</label>
      </div>
      <div class="form-group">
        <input type="radio" id="telefono" value="telefono" v-model="picked">
        <label for="telefono">Telefono</label>
      </div>
      <div class="form-group">
        <input type="radio" id="remoto" value="remoto" v-model="picked">
        <label for="remoto">Remoto</label>
      </div>
      
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
        <input type="button"  @click="saveData()" class="btn btn-primary" value="Guardar">
      </div>
      </form>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import { bus } from '../main'
export default {
  name: 'modal',
  data: function () {
    return {
        picked:null,
        date: null,
        hour: null,
        minute:null,
        objectSchedule:{}
    }
  },
  props: {
       id: Number
  },
   computed: {
    dateTotal: function () {
      return this.date + " " + this.hour + ":" + this.minute;
    },
  
  },
 
  methods: {
    saveData: function () {
        this.objectSchedule={
            id:this.id,
            fecha: this.dateTotal,
            forma:this.picked
        };
        bus.$emit('dataS', this.objectSchedule);
        this.objectSchedule={
            id:null,
            fecha: null,
            forma:null
        };
     
    }
  }
}
</script>
<style>
.form-group.form-hora input[type="number"] {
    width: 48px;
}
</style>