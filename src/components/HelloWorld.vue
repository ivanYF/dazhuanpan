<template>
<div>
    <div class="dazhuanpan">
        <span v-for="(item,idx) in list" :class="{'btnclick':item.num==0,'active':(index==item.num && item.isopen)}">{{item.num}}</span>
    </div>
    <div class="btn" @click.stop="clickAnimtion">开始抽奖</div>
</div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data () {
        return {
            index: 0,    //当前转动到哪个位置，起点位置
            count: 8,    //总共有多少个位置
            timer: 0,    //setTimeout的ID，用clearTimeout清除
            speed: 20,   //初始转动速度
            times: 0,    //转动次数
            cycle: 30,   //转动基本次数：即至少需要转动多少次再进入抽奖环节
            prize: -1,   //中奖的index 
            click:false, //是否点击
            list:[
                {num:1,isopen:false,},
                {num:2,isopen:true,},
                {num:3,isopen:true,},
                {num:8,isopen:true,},
                {num:0,isopen:true,},
                {num:4,isopen:true,},
                {num:7,isopen:true,},
                {num:6,isopen:true,},
                {num:5,isopen:true,},
            ]
        }
    },
    created(){
    },
    mounted(){
    },
    computed: {
    },
    methods:{
        roll:function(){
            var _this = this;
            var index = this.index;
            var count = this.count;
            index += 1;
            if (index > count) {
                index = 1;
            };
            this.index = index;
            return false;
        },
        stop:function(index){
            var _this = this;
            _this.prize = index;
            return false;
        },
        rollAnimation:function(){
            var _this = this;
            _this.times += 1;
            _this.roll();
            if (_this.times > _this.cycle + 10 && _this.prize == _this.index) {
                clearTimeout(_this.timer);
                _this.prize = 0;
                _this.times = 0;
                _this.click = false;
            }else{
                if (_this.times<_this.cycle) {
                    _this.speed -= 10;
                }else if(_this.times == _this.cycle) {
                    // 获取中奖结果
                    var index = Math.ceil(Math.random()*(_this.count))
                    _this.prize = index;        
                }else{
                    if (_this.times > _this.cycle + 10) {
                        _this.speed += 40;
                    }else{
                        _this.speed += 20;
                    }
                }
                if (_this.speed < 60) {
                    _this.speed = 60;
                };
                console.log(_this.times+'^^^^^^'+_this.speed+'^^^^^^^'+_this.prize);
                _this.timer = setTimeout(_this.rollAnimation,_this.speed);
            }
            return false;
        },
        clickAnimtion:function(){
            var _this = this;
            if (_this.click) {
                return false;
            }else{
                _this.speed = 130;
                _this.rollAnimation();
                _this.click = true;
                return false;
            }
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "../assets/components/mixin.scss";
$rem: 1rem/37.5;
.dazhuanpan{
    width: $rem*330;
    height: $rem*330;
    border: 1px solid #000;
    margin: 0 auto;
    span{
        width: $rem*110;
        height: $rem*110;
        display: inline-block;
        font-size:$rem*40;
        line-height: $rem*110;
        border: 1px solid #f00;
        box-sizing: border-box;
        &.active{
            background-color: #ff0;
        }
        &.btnclick{
            background-color: #f00;
            color: #fff;
        }
    }
}
.btn{
    line-height: $rem*30;
    font-size:$rem*30;
    margin: $rem*40 0 0 0;
}
</style>
