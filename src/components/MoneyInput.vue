<template>
   <input v-model="formatNum" :placeholder="placeholder" :disabled="disabled"/>
</template>

<script>
    /**
     * 食用方法：
     * <money-input v-model="amountMoney" :placeholder='msg' :disabled="true"></money-input>
     * placeholder 占位符 String
     * disabled 禁用  Boolean
     */
    export default {
        name: "MoneyInput",
        props: {
            value: {
                type: String,
                default() {
                    return '';
                },
            },
            placeholder:{
                type: String,
                default() {
                    return '';
                },
            },
            disabled:{
                type: Boolean,
                default() {
                    return false;
                },
            }
        },
        data() {
            return {
                value_inner:null,
                formatNum:'',
            }
        },
        created() {

        },
        mounted() {
            //初始化
            this.value_inner = this.value;
            this.formatFunc(this.value_inner);
        },
        watch: {
            formatNum(newValue){
                this.formatFunc(newValue);
            }
        },
        methods: {
            formatFunc(newValue){
                //当用户输入时 去除说有 所有的 ','和 '，'判断换是否为数字
                let valueNotDou = newValue.replace(/,|，/g,'');

                let reg = /^[0-9]+(.[0-9]{0,2})?$/

                if(reg.test(valueNotDou)){
                    //验证成功给一个数字 实际值赋值
                    this.$emit('input', valueNotDou);
                    this.value_inner = valueNotDou;
                    //将显示的值格式化话
                    let numSteps = valueNotDou.split(".");
                    numSteps[0] = numSteps[0].replace(/(\d)(?=(?:\d{3})+$)/g, '$1,'); //添加千分位
                    this.$nextTick(()=>{
                        this.formatNum = numSteps.join(".");
                    });

                }else if(newValue===''){
                    this.$emit('input', newValue);
                    this.value_inner = newValue;
                }else{
                    this.$nextTick(()=>{
                        this.formatNum = this.value_inner;
                    });
                }
            }
        },
        computed: {},
        components: {}
    }
</script>

<style scoped>

</style>
