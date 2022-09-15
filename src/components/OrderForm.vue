<template>
    <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">{{name}}</h5>
        <!-- <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button> -->
      </div>
      <div class="modal-body">
        <!-- {{JSON.stringify(getMenuList)}} -->
        <table class="table">
            <thead>
                <tr>
                    <th>
                        Name
                    </th>
                    <th>
                        Price
                    </th>
                    <th>Quantity</th>
                </tr>
            </thead>
            <tr v-for="(item,index) in menuList" :key="index">
                <td>{{item.name}}</td>
                <td>{{item.price}}</td>
                <td>
                    <button type="button" class="btn btn-outline-danger">Add to Cart</button>
                </td>
            </tr>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Pay Now</button>
      </div>
</template>

<script>
import { axiosInstance } from "./../api/axios";
import { GET_MENU_LIST} from './../api/endpoints';
export default {
    data(){
        return{
            qty:0,
            menuList:[]
        }
    },
    props:["code","name"],
    methods:{
        add(){
            this.$data.qty+=1;
        },
        minus(){

            this.$data.qty>0 ? this.$data.qty-=1 : 0
        },
        getMenuList(){
            const payload = {
                code: this.$props.code,
            };
            console.log("payload",payload);
            axiosInstance.get(GET_MENU_LIST,{ params: payload }).then((res)=>{
                if(res.result == 'success'){
                    this.$data.menuList = res.data;
                    console.log("this.$data.FoodList",this.$data.menuList);
                    // this.captureMenu(this.$data.menuList);
                }
            })
        },
    },
    created(){
        this.getMenuList();
    }

}
</script>

<style scoped>
.btn-group input.form-control {
    border-radius: 0px;
}

.btn-group{
    width: 150px;
}
</style>