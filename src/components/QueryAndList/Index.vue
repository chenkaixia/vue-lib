/*
*QueryAndList组件 (例子见 components/order/list/OrderList.vue)
*   query-items          {Array,必填}      //查询条件 ,
*   list-config          {Object,必填}     //设置每一列标题thead和宽度span,例如
*        listConfig:{
*            thead:['商品基本信息','服务商','金额','订单状态','操作'],
*            span: [ 5,            5,       5,     5,        4]    //数组之和需等于24
*        },
*    get-list-func         {Function,必填}  //获取列表数据方法
*    rowTemplateComponent  {component,必填} //列表每一行的模板
*/

<template>
    <div>
        <!--查询搜索组件-->
        <query 
            :query-items="queryItems" 
            @search="search">
        </query>

        <!--列表组件-->
        <list ref="List"
            :list-config= "listConfig"
            :get-list-func = "getListFunc"
            :row-template-component="rowTemplateComponent"
            :use-slot="useSlot">   
            <div slot="row" slot-scope="scope" v-if="useSlot">
                <slot name="row" :item="scope.item"></slot>
            </div> 
        </list>

    </div>
</template>

<script type="text/javascript">
    import Query from './Query';
    import List from './List';
    export default {
        components: {Query,List},
        props: {    
            queryItems: {
                type: Array,
                required: true
            },
            rowTemplateComponent: {
                type: Object
            },
            listConfig: {
                thead:{
                    type: Array,
                    required: true
                },
                span:{
                    type:Array,
                    required: true
                },
                required: true
            },
            getListFunc: {
                type: Function,
                required: true
            },
            useSlot: {
                type: Boolean,
                default: false
            }
        },
        methods: {
            //查询
            search(args) {
                this.$refs['List'].getList(args[0],args[1]);
            },
        }
    }
</script>
