<template>
  <b-container class="container-1">
    <b-row>
      <b-col  lg="12">
        <img :src="images?.[0]"/>
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="6">
        <h3>{{name}}</h3>
        <h3>{{code}}</h3>
      </b-col>
        <div class="col-6">
              <!-- Button trigger modal  v-if=isLoggedIn-->
          <button type="button" data-bs-toggle="modal" data-bs-target="#exampleModal" class="btn btn-danger float-end m-3" >Place online order
          </button>
                  
        </div>
        
    </b-row>
    <b-card no-body>
      <b-tabs card>
        <b-tab title="Overview" active>
          <b-card-text>{{
            overview
          }}
          </b-card-text>
        </b-tab>
        <b-tab title="Contact">
          <b-card-text>
            {{contact}}
          </b-card-text>
        </b-tab>
      </b-tabs>
    </b-card>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
     <order-form :code="code" :name="name"></order-form>
      
    </div>
  </div>
</div>
  </b-container>
</template>

<script>
import { mapGetters } from "vuex";
import OrderForm from "./OrderForm.vue";
// import { axiosInstance } from '../api/axios';
// import { GET_MENU_LIST } from "./../api/endpoints";
export default {
  data(){
        return{
        }
    },
  components:{
    'order-form':OrderForm
  },
  props: ["images","overview","contact","name","code"],
  computed: {
    ...mapGetters('authentication',['isLoggedIn','userDetails'])
  },
  methods:{
        
    },
};


</script>

<style scoped>
.container-1 {
  height: 500px;
  width: 100%;
}

img {
    height: 300px;
    width: 100%;
    object-fit: cover;
}
</style>