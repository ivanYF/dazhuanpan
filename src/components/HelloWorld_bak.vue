<template>
    <div class="dazhuanpan">
        <div id="lottery">
            <table border="0" cellpadding="0" cellspacing="0">
                <tr>
                    <td class="lottery-unit lottery-unit-0" style="border:3px solid #000;">0</td>
                    <td class="lottery-unit lottery-unit-1" style="border:3px solid #000;">1</td>
                    <td class="lottery-unit lottery-unit-2" style="border:3px solid #000;">2</td>
                    <td class="lottery-unit lottery-unit-3" style="border:3px solid #000;">3</td>
                </tr>
                <tr>
                    <td class="lottery-unit lottery-unit-11" style="border:3px solid #000;">11</td>
                    <td colspan="2" rowspan="2" style="border:3px solid #f00;"><a href="#" @click.stop="clickAnimtion"></a></td>
                    <td class="lottery-unit lottery-unit-4" style="border:3px solid #000;">4</td>
                </tr>
                <tr>
                    <td class="lottery-unit lottery-unit-10" style="border:3px solid #000;">10</td>
                    <td class="lottery-unit lottery-unit-5" style="border:3px solid #000;">5</td>
                </tr>
                <tr>
                    <td class="lottery-unit lottery-unit-9" style="border:3px solid #000;">9</td>
                    <td class="lottery-unit lottery-unit-8" style="border:3px solid #000;">8</td>
                    <td class="lottery-unit lottery-unit-7" style="border:3px solid #000;">7</td>
                    <td class="lottery-unit lottery-unit-6" style="border:3px solid #000;">6</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data () {
        return {
            index:-1,    //当前转动到哪个位置，起点位置
            count: 0,    //总共有多少个位置
            timer: 0,    //setTimeout的ID，用clearTimeout清除
            speed: 20,   //初始转动速度
            times: 0,    //转动次数
            cycle: 50,   //转动基本次数：即至少需要转动多少次再进入抽奖环节
            prize: -1,   //中奖的index 
            click:false, //是否点击
            obj:null,    //obj
            
        }
    },
    created(){
    },
    mounted(){
    },
    computed: {
    },
    methods:{
        init:function(id){
            var _this = this;
            if ($("#"+id).find(".lottery-unit").length>0) {
                var $lottery = $("#"+id);
                var $units = $lottery.find(".lottery-unit");
                _this.obj = $lottery;
                _this.count = $units.length;
                $lottery.find(".lottery-unit-"+_this.index).addClass("active");
            };
        },
        roll:function(){
            var index = this.index;
            var count = this.count;
            var lottery = this.obj;
            $(lottery).find(".lottery-unit-"+index).removeClass("active");
            index += 1;
            if (index>count-1) {
                index = 0;
            };
            $(lottery).find(".lottery-unit-"+index).addClass("active");
            this.index=index;
            return false;
        },
        stop:function(index){
            this.prize=index;
            return false;
        },
        rollAnimation:function(){
            var _this = this;
            _this.times += 1;
            _this.roll();
            if (_this.times > _this.cycle+10 && _this.prize==_this.index) {
                clearTimeout(_this.timer);
                _this.prize=-1;
                _this.times=0;
                _this.click=false;
            }else{
                if (_this.times<_this.cycle) {
                    _this.speed -= 10;
                }else if(_this.times == _this.cycle) {
                    var index = Math.random()*(_this.count)|0;
                    _this.prize = index;        
                }else{
                    if (_this.times > _this.cycle+10 && ((_this.prize==0 && _this.index==7) || _this.prize==_this.index+1)) {
                        _this.speed += 110;
                    }else{
                        _this.speed += 20;
                    }
                }
                if (_this.speed<40) {
                    _this.speed=40;
                };
                console.log(_this.times+'^^^^^^'+_this.speed+'^^^^^^^'+_this.prize);
                _this.timer = setTimeout(_this.rollAnimation,_this.speed);
            }
            return false;
        },
        clickAnimtion:function(){
            var _this = this;
            _this.init('lottery');
            if (_this.click) {
                return false;
            }else{
                _this.speed = 100;
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
#lottery{width:574px;height:584px;margin:20px auto 0;padding:50px 55px;}
#lottery table td{width:142px;height:142px;text-align:center;vertical-align:middle;font-size:24px;color:#333;font-index:-999}
#lottery table td a{width:284px;height:284px;line-height:150px;display:block;text-decoration:none;}
#lottery table td.active{background-color:#ea0000;}
</style>
