<template>
     <div class="row" >
        <div class="col">
          <select class="form-control" v-model="address.address_type">
            <option 
            v-bind:value="type.value" 
            v-for="(type,index) in address_type_defaults" 
            v-bind:key="index">
            {{type.label}}
            </option>
          </select>
        </div>
        <div class="col">
          <input type="text" ref="searchTextField" class="form-control" placeholder="Last name" v-model="address.address_string">
        </div>
        <div class="col">
         <button class="btn btn-danger" @click="$emit('removeRow',row);">Remove</button>
        </div>

     </div>
</template>

<script>
export default {
  props:["address","address_type_defaults","row"],
  mounted() {
        new google.maps.places.Autocomplete(
        this.$refs.searchTextField
        );
    }

}
</script>

<style>

</style>