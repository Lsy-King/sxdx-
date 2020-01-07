<template>
  <briup-fulllayout title="订单确认">
  {{addressId}}
    <van-dropdown-menu>
      <van-dropdown-item v-model="addressId" :options="options" />
    </van-dropdown-menu>
    <p>{{addresses}}</p>
    <p>{{$route.query}}</p>

    <div style="padding:0 2em">
       <p>服务名称：{{$route.query.name}}</p>
       <p>服务简介：{{$route.query.description}}</p>
       <p>服务价格：{{$route.query.price}}</p>
       <p>服务数量：1</p>
       <p>服务小计：{{$route.query.price*1}}</p>

    </div>
    <div style="position:fixed;bottom:0;width:100%;">
    <van-button @click="submitHandler" block type="primary">提交订单</van-button>
    </div>
  </briup-fulllayout>
</template>
<script>
import { mapState } from 'vuex'
import {get,post_obj_array} from '../../../http/axios'
export default {
  data(){
    return {
      addressId:0,    // 服务地址id
      addresses:[],
      orderLines:[]
    }
  },
  created(){
    // 在页面加载的时候查询地址信息
    this.loadAddress();
    //初始化订单项 （将我们购买的产品放入购物车）
    let orderLine = {
        number:1,
        price:this.$router.price,
        productId:this.$route.query.id
    };
    this.orderLines.push(orderLines);
  },
  computed:{
    // 映射info信息
    ...mapState("user",["info"]),
    // [{id:1,province:'山西省',city:"晋中市"},{id:1,province:'江苏省',city:"昆山市"}] =》 [{text:"山西省晋中市",value:1}]
    options:function(){
      // 将addresses的数据计算为一个新的数组返回
      return this.addresses.map(item=>{
        return {
          text:item.province+" "+item.city+" "+item.area+" "+item.address,
          value:item.id
        }
      })
    }
  },
  methods:{
     submitHandler(){
         let url ="/order/save";
         let data = {
             customerId:this.info.id,
             addressId:this.addresses,
             orderLines:this.orderLines
         }
         console.log(data);
         post_obj_array(url,data).then(response=>{
             this.$toast("ti'jiao");
             this.$route.push("/manager/order")

         })
     },

    // 查询地址信息
    loadAddress(){
      let id = this.info.id;  
      let url = "/address/findByCustomerId?id="+id;
      get(url).then((response)=>{
        // 将查询到的地址信息绑定到address这个变量中
        this.addresses = response.data;
        // 将查询到的地址列表中第一个地址的id赋值给data中addressId,表示默认地址
        this.addressId = this.addresses[0].id;
      })
    }
  }
}
</script>