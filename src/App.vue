<template>
  <div id="app">
    <h2 class="mb-5">Go Kart Fuel Mixture Calculator</h2>
    
    <b-container fluid="lg" class="p-3">
        <div class="d-flex flex-column align-items-start">
          <b-input-group>
            <template v-slot:prepend>
              <b-input-group-text >Fuel</b-input-group-text>
            </template>
            <b-form-input v-model="volume" size="md" type="number"></b-form-input>

            <template v-slot:append>
              <b-dropdown :text="buttonText" variant="dark" class="text-light">
                <b-dropdown-item @click="selectUnit(1000,'L')">L</b-dropdown-item>
                <b-dropdown-item @click="selectUnit(1,'ml')">ml</b-dropdown-item>
                <b-dropdown-item @click="selectUnit(3785.41,'gal')">gal</b-dropdown-item>
              </b-dropdown>
            </template>
          </b-input-group>

          <b-form-checkbox v-model="isPercentage" switch size="lg" class="my-3">
            {{switchText}}
          </b-form-checkbox>

          <b-input-group v-if="isPercentage" size="md" append="%">
            <b-form-input v-model="percent" type="number" placeholder="Enter Percentage"></b-form-input>
          </b-input-group>

          <b-input-group v-if="!isPercentage" size="md" append=": 1">
            <b-form-input v-model="ratio" type="number" placeholder="Enter Ratio"></b-form-input>
          </b-input-group>

          <h3 class="mt-3" v-if="result>0">Add <b>{{result}} ml</b> of oil</h3>

        </div>

    </b-container>

    <v-footer padless absolute>
      <v-col class="text-center" cols="12">
        <a href="terms.html" target="_blank" class="text-dark mx-4">Terms of Use</a>
        <a href="https://github.com/logeeker/2strokemix" class="text-dark mx-4" target="_blank">Source</a>
      </v-col>
      <v-col class="text-center" cols="12">
        © {{ new Date().getFullYear() }} <strong>Seng Song</strong> | All Rights Reserved
      </v-col>
    </v-footer>
      
  </div>
</template>

<script>
export default {
  name: 'App',
  data: function(){
    return {
      buttonText:"L",
      volume:'',
      multiplier:1000,
      isPercentage:true,
      percent:'',
      ratio:'',
      dialog:false
    }
  },
  computed:{
    volumeInml:function(){
      return this.volume*this.multiplier
    },
    switchText:function(){
      if (this.isPercentage){
        return "Using Percentage";
      }else{
        return "Using Ratio";
      }
    },
    result:function(){
      if (this.isPercentage){
        
        return Math.round((this.percent*this.volumeInml/100)*100)/100;
      }else{
        var p=1/this.ratio;
        
        return Math.round((this.volumeInml*p)*100)/100;
      }
    }
  },
  methods:{
    selectUnit:function(m,text){
      this.buttonText=text;
      this.multiplier=m
    }
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
