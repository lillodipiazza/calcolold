<template>
  <div>
    <div class="success" v-show="success">
      <div class="testo">Dati Salvati</div>
    </div>
    <div v-if="date" class="salvataggio">
      Salvato: {{date | moment("dddd, Do MMMM YYYY") }}
    </div>
    <b-container>
      <b-row>
        <b-col>
          <label><code>IERI</code></label>
        </b-col>
        <b-col>
          <label><code>OGGI</code></label>
        </b-col>
        <b-col align-self="center">
          <label><code>VENDUTO</code></label>
        </b-col>
      </b-row>      
      <b-row>
        <b-col>
          <label><code>50</code></label>
          <b-form-input type="number" v-model="dati.nopesoieri" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col>
          <label><code>50</code></label>
          <b-form-input type="number" v-model="dati.nopesooggi" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col align-self="center">
          <label><code>50</code></label>
          <b-form-input :placeholder="nopeso"></b-form-input>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <label><code>52</code></label>
          <b-form-input type="number" v-model="dati.leggeroieri" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col>
          <label><code>52</code></label>
          <b-form-input type="number" v-model="dati.leggerooggi" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col align-self="center">
          <label><code>52</code></label>
          <b-form-input :placeholder="leggero"></b-form-input>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <label><code>56</code></label>
          <b-form-input type="number" v-model="dati.medioieri" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col>
          <label><code>56</code></label>
          <b-form-input type="number" v-model="dati.mediooggi" placeholder="Valore"></b-form-input>
        </b-col>
        <b-col align-self="center">
          <label><code>56</code></label>
          <b-form-input :placeholder="medio"></b-form-input>
        </b-col>
      </b-row>   
      <b-row>
        <b-col>
        </b-col>
        <b-col>
        </b-col>
        <b-col align-self="center">
          <label><code>Totale</code></label>
          <b-form-input :placeholder="totale"></b-form-input>
        </b-col>
      </b-row>          
      <b-row>
        <b-col class="pulsanti mt-4">
          <b-button style="margin-right: 1rem" class="w-100" variant="warning" @click="inverti">Inverti</b-button>
          <b-button class="w-100" variant="success" @click="persist">Salva</b-button>
        </b-col>
      </b-row>      
    </b-container>
    
  </div>
</template>

<script>
export default {
  data(){
    return {
      dati: {
        leggerooggi: null,
        leggeroieri: null,
        mediooggi: null,
        medioieri: null,
        nopesooggi: null,
        nopesoieri: null
      },
      success: false
    }
  },
  mounted() {
    if(
      localStorage.medioieri && 
      localStorage.mediooggi && 
      localStorage.nopesoieri && 
      localStorage.nopesooggi && 
      localStorage.leggeroieri && 
      localStorage.leggerooggi &&
      localStorage.date
      ) {
      this.dati.medioieri = localStorage.medioieri;
      this.dati.mediooggi = localStorage.mediooggi;
      this.dati.nopesoieri = localStorage.nopesoieri;
      this.dati.nopesooggi = localStorage.nopesooggi;
      this.dati.leggeroieri = localStorage.leggeroieri;
      this.dati.leggerooggi = localStorage.leggerooggi;   
      this.date = localStorage.date   
    }
  },
  methods: {
    persist() {
      localStorage.medioieri = this.dati.medioieri;
      localStorage.mediooggi = this.dati.mediooggi;
      localStorage.nopesoieri = this.dati.nopesoieri;
      localStorage.nopesooggi = this.dati.nopesooggi;
      localStorage.leggeroieri = this.dati.leggeroieri;
      localStorage.leggerooggi = this.dati.leggerooggi;
      this.success = true
      localStorage.date = new Date()
      setTimeout(() => {this.success = false}, 3000)    
    },
    inverti(){
      this.dati.nopesoieri = localStorage.nopesooggi
      this.dati.nopesooggi = null
      this.dati.leggeroieri = localStorage.leggerooggi
      this.dati.leggerooggi = null
      this.dati.medioieri = localStorage.mediooggi
      this.dati.mediooggi = null
    }    
  },  
  computed: {
    nopeso() {
      if (this.dati.nopesooggi > 0 ) {
        return this.dati.nopesooggi-this.dati.nopesoieri
      } else {
        return 0
      }      
    },
    leggero() {
      if (this.dati.leggerooggi > 0 ) {
      return this.dati.leggerooggi-this.dati.leggeroieri
      } else {
        return 0
      }
    },
    medio() {
      if (this.dati.mediooggi > 0 ) {
      return this.dati.mediooggi-this.dati.medioieri
      } else {
        return 0
      }
    },
    totale() {
      return this.nopeso+this.leggero+this.medio
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.success {
  background-color: #42b983;
  color: #fff;
  position: absolute;
  top: 0;
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.salvataggio {
  background-color: #42b983;
  color: #fff;
  padding: 10px;
  margin-bottom: 1rem;
}
.pulsanti {
  display: flex;
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
