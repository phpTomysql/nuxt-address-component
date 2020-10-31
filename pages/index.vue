<template>

  <div class="col-md-12 mt-2">
      <div class="row mt-1 mb-1 row-style" v-for="(address ,index) in address" v-bind:key="index">
        <div class="col">
          <select class="form-control" v-model="address.address_type">
            <option value="">Choose Type</option>
            <option 
            v-bind:value="type.value" 
            v-for="(type,index) in address_type_defaults" 
            v-bind:key="index">
            {{type.label}}
            </option>
          </select>
        </div>
        <div class="col">
          <div class="row">
            <div class="col-md-2">
              <b-button v-b-tooltip.hover title="Tooltip directive content" class="rounded-circle">
              ?
              </b-button>
            </div>
            <div class="col-md-10">
              <input type="text" ref="searchTextField" class="form-control" placeholder="Address" v-model="address.address_string">
            </div>
          </div>
          
          
        </div>
        <div class="col">
         <button @click="removeRow(index)" class="btn btn-danger" v-show="showRemoveBtn">Remove</button>
        </div>
    </div>
    <div class="row mt-2">
      <div class="col-md-6">
        <button @click="addRow" class="btn btn-success" v-if="address.length <=9">Add</button>
      </div>
    </div>
  </div>
</template>

<script>
 
export default {
    asyncData() {
      return { 
        field:0,
        address_type_defaults:[
              { value:'residential',label:'Residential'},
              { value:'domicile',label:'Domicile'},
              { value:'legal',label:'Legal'},
              { value:'operational',label:'Operational'}
            ],
        address:[
          {
            address_type:'',
            address_string:''
          }
        ]
       }
    },
    computed:{
        showRemoveBtn() {
          return this.address.length > 1;
        }
    },
    watch:{
     
    },
    updated() {
      this.initLocationSearch();
    },
    methods:{
      addRow() {
        
        this.address.push({address_type:'',
            address_string:''});
            this.field++
            
      },
      removeRow( index ) {
        this.address.splice(index,1);
        this.field--
      },
      async initLocationSearch() {
        let self = this;

        let ele = await (this.field <= 0 ) ? this.$refs.searchTextField[this.field] : this.$refs.searchTextField[this.field];
        let  autocomplete = new google.maps.places.Autocomplete(
            ele,
            {
              fields: ["formatted_address"] // Reduce API costs by specifying fields
            }
          );

        autocomplete.addListener("place_changed", () => {
          let place = autocomplete.getPlace();
          if (place && place.formatted_address) {
            let eleIndex = (this.field <= 0) ? 0 : this.field;
            console.log(this.address[eleIndex]);
            this.address[eleIndex].address_string = place.formatted_address;
          }
        });
      }
    },
    mounted() {
      this.initLocationSearch();
    }

}
</script>

<style>
body {
  background-color: black;
  color: white;
}
input[type="text"]{
background-color:black;
color: white;
}
select {
   background: transparent !important;
    -webkit-appearance: none !important;
}
select option {
    margin: 40px !important;
    background:black !important;
    color: #fff !important;
    text-shadow: 0 1px 0 rgba(0, 0, 0, 0.4);
}

</style>
