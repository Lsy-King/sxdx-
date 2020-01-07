<template>
    <briup-fulllayout title="新增地址">
     <div>
     <van-cell-group>
        <van-field v-model="form.telephonr" placeholder="请输入手机号"/>
    </van-cell-group>
    <van-cell-group>
        <van-field v-model="form.province" placeholder="省"/>
    </van-cell-group>
    <van-cell-group>
        <van-field v-model="form.city" placeholder="市"/>
    </van-cell-group>
    <van-cell-group>
        <van-field v-model="form.area" placeholder="区"/>
    </van-cell-group>
    <van-cell-group>
        <van-field v-model="form.address" placeholder="详细地址"/>
    </van-cell-group>

    <van-button block type="primary" @click="submitHandler">保存</van-button>
     </div>
    </briup-fulllayout>
</template>

<script>
import {post} from '../../../http/axios'
import { mapState } from 'vuex';
export default {
    data(){
        return{
            form:{}
        }
    },
    computed:{
        ...mapState('user',['info'])
    },
    methods:{
        submitHandler(){
            let url="/address/asveOrUpdate"
            this.form.customedrId = this.info.id;
            post(url,this.form).then((response)=>{
                //返回上一个页面
                this.$router.go(-1);
            })
        },
        toAddressEditHandler(){
            this.$router.push("/manager/address_edit");
        }

    }
}
</script>