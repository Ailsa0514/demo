<template>
  <div class="container">
    <h1 class= "title">{{ msg }}</h1>
    <ul class="list">
       <li class="item-li" v-for="(item,index) in listData" :key="item.id">
          <div class="item">
            <div class="item-check" @click="selectItem(index)" :class = "{'check-true': item.checked}"></div>
             <div class="item-image">
               <img src="/static/images/88.jpg" alt="item.name">
             </div>
             <div class="item-content">
               <p>{{item.name}}</p>
               <p>&yen;{{item.price}}</p>
               <p>总价：&yen;{{item.price * item.quantity}}</p>
             </div>
             <div class="item-quantity">
               <span @click="changeQuantity(index,-1)">-</span>
               <input type="number" class="item-input" v-model="item.quantity" value="item.quantity">
               <span @click="changeQuantity(index,1)">+</span>
             </div>
          </div>
       </li>
    </ul>
    <div class="list-bottom"> 
      <div class="list-bottom-left">
        <div class="item-check" :class = "{'check-true': totalChecked}" @click="listSelect"></div>
        <label for="all">全选</label>
      </div>
       <div class="list-bottom-right">
         <div>结算：&yen;{{totalPrice}}({{totalQuantity}}件)</div>
      </div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */
const data = [
        {id:"3232423432452342s",name: "红色手账本", price :31,quantity:1},
        {id:"45565612fsdfsdfds",name: "苹果笔记本", price :10236,quantity:1},
        {id:"dfsdfgdsfgsdf6536",name: "华为手机", price :12023,quantity:1},
        {id:"fdsfsdfsfds696939",name: "苹果8P", price :1220,quantity:1},
        {id:"56f565656f5ds6f5s",name: "iphone 6s", price :8962,quantity:1},
        {id:"4d55656f5s6df5sd6",name: "English Book", price :300,quantity:1},
        {id:"ffdterv4541214554",name: "无人生还强烈推荐", price :1002,quantity:1},
        {id:"4fdsfv12xcv45sdgh",name: "中国最好的书", price :156,quantity:1},
]
 /* eslint-disable */ 
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '购物车结算',
      listData : data,
      totalPrice: 0,
      totalQuantity : 0,
      totalChecked : false
    }
  },
  watch:{
    listData : {
      handler : function (obj) {
         let totalPrice  = 0;
         let totalQuantity = 0;
         for(var i = 0 ;i < obj.length ; i++){
            if(obj[i].checked){
              totalQuantity += obj[i].quantity;
              totalPrice +=  obj[i].quantity * obj[i].price
            }
         }
         this.totalQuantity = totalQuantity;
         this.totalPrice = totalPrice;
      },
      deep : true
    }
  },
  methods:{
    changeQuantity (index,plus){
        var quantity = this.listData[index].quantity;
        var sum = quantity + plus;
        if(sum <= 1 ){
          sum = 1;
        }
        this.listData[index].quantity = sum;
        this.$set(this.listData,index,this.listData[index])
    },
    selectItem (index) {
       var checked = true ;
       var item = this.listData[index];
       if(item.checked != undefined){
         checked = !item.checked
       }
       item.checked = checked;
       var bolem = this.listData.every(function (item) { return item.checked});
       if(bolem){
        this.totalChecked = bolem;
       }
       this.$set(this.listData,index,this.listData[index]);
    },
    listSelect () {
      var checked  = !this.totalChecked;
      for(var i = 0 ;i < this.listData.length ; i++){
         this.listData[i].checked = checked;
         this.$set(this.listData,i,this.listData[i])
      }
      this.totalChecked = checked;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .container{width:100%; height:100%;}
    .title{font-size:16px;padding:10px;border-bottom : 1px solid #ddd;}
    .list{margin: 10px;box-sizing: border-box;}
    .item-li{margin: 10px 0;padding: 10px;background: #fff;box-shadow: 1px 0 5px rgba(0,0,0,.2)}
    .item{display: flex;align-items: center;justify-content: space-between;}
    .item-check{width: 20px;height: 20px;border:1px solid #333}
    .check-true {background: crimson}
    .item-image{width: 120px;height: 120px;padding: 10px;}
    .item-image img{width: 100%}
    .item-content{flex: 1}
    .item-quantity{background: cornflowerblue;height: 30px;display: flex;}
    .item-quantity span{display: inline-block;width: 30px;background: #ddd;font-size: 20px;text-align: center}
    .item-input{width: 50px;height: 100%;padding:0 5px;vertical-align: middle;display: inline-block;color: #fff;border:none;background: none;outline: none;}
    .list-bottom{position:fixed;bottom:0;height: 50px;width: 100%;display: flex;background: cornsilk;align-items: center;justify-content: space-between;}
    .list-bottom-left{padding:0 10px;display: flex;align-items: center;}
    .list-bottom-right{padding:0 10px;display: flex;align-items: center;background: darkorange;height: 50px;}
    .list-bottom-check{margin:10px;}
</style>
