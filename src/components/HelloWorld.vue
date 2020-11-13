<template>
  <div class="app">
    <ul class="national list">
      <li class="item">
        <div class="dotted">
          <input type="checkbox" id="input1" v-model="checked" @click="checkedNational">
        </div>
        <label for="input1">全国</label>
        <div class="right">
          <span></span>
        </div>
      </li>
    </ul>
    <!-- 二级菜单 -->
    <ul class="area list">
      <li class="item" v-for="(item,index) in twoList" :key="item" >
        <div class="dotted">
          <input ref="input2" type="checkbox" :id="item" :checked="checked1" @click="checkedArea(index)">
        </div>
        <label :for="item">{{item}}</label>
        <div class="right">
          <span></span>
        </div>
      </li>
    </ul>
    <!-- 三级菜单 -->
    <ul class="province list">
      <li class="item" v-for="(item,index) in threeList" :key="item" >
        <div class="dotted">
          <input ref="input3" type="checkbox" :id="item" :checked="checked2" @click="checkedProvince(index)">
        </div>
        <label :for="item">{{item}}</label> 
        <div class="right">
          <span></span>
        </div>      
      </li>    
    </ul>
    <!-- 四级菜单 -->
    <ul class="county list">
      <li class="item" v-for="(item,index) in fourList" :key="item">
        <div class="dotted">
          <input ref="input4" type="checkbox" :id="item" :checked="checked3" @click="checkedCounty(index)">
        </div>
        <label :for="item">{{item}}</label> 
        <div class="right">
          <span></span>
        </div>      
      </li>    
    </ul>
  </div>
</template>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
*{
	margin: 0;
	padding: 0;
}

  ol, ul {
    list-style: none;
  }
  .app{
    position: relative;
  }
  .list {
    position:absolute;
    top:0;
    left:0;
    width:200px;
    height:800px;
    background:#eee;
  }
  .area{
    left:200px;
  }
  .province{
    left:400px;
  }
  .county{
    left:600px;
  }
  .item{
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height:60px;
  }
  .dotted,.right{
    display: flex;
    justify-content: center;
    align-items: center;
    width:30px;
    height:30px;
    border:2px dotted #444;
    border-radius: 2px;
  }
  input{
    outline: 0;
    width: 24px;
    height: 24px;
    border:1px solid #444;
  }
  
  input[type=checkbox]:checked{
    border:1px solid blue;
      /* // background: url("/img/checkedbox.png")no-repeat center; */
    color:blue;
  }
  label{
    font-size:18px;
  }

  span{
    display: block;
    width: 0;
    height: 0;
    border-top: 5px solid transparent;
    border-left: 10px solid #444;
    border-bottom: 5px solid transparent;
  }
</style>
<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      threelists:[['北京市','天津市','河北省'],['河南省','吉林省','山西省'],['山东省','黑龙江省'],['陕西省']],
      fourlists:[['昌平区'],['蓟县'],['保定市','石家庄'],['大连市','沈阳市','葫芦岛市'],['太原市','大同市'],['青岛市','聊城市'],['漠河市'],['西安市']],
      twoList:['华南地区','华西地区','华东地区','华北地区'],
      threeList:['北京市','天津市','河北省','河南省','吉林省','山西省','山东省','内蒙古','黑龙江省','陕西省'],
      fourList:['昌平区','朝阳区','大兴区','通州区'],
      checked:false,
      checked1:false,
      checked2:false,
      checked3:false,
      twoMenu:0,
      threeMenu:0
    }
  },
  methods:{
    checkedNational() {
      console.log(this.$refs.input2)
      if(!this.checked) {
        this.$refs.input2.forEach(ele => {
          ele.checked = true
        });
        this.$refs.input3.forEach(ele => {
          ele.checked = true
        });
        this.$refs.input4.forEach(ele => {
          ele.checked = true
        });
      }else {
        this.$refs.input2.forEach(ele => {
          ele.checked = false
        });
        this.$refs.input3.forEach(ele => {
          ele.checked = false
        });
        this.$refs.input4.forEach(ele => {
          ele.checked = false
        });
      }
      
    },
    checkedArea(index) {
      this.twoMenu = index
      this.threeList = this.threelists[index]
      this.fourList = this.fourlists[0]
      this.$nextTick(()=>{
        if(this.$refs.input2[index].checked) {
          this.$refs.input3.forEach(ele => {
            ele.checked = true
          });
          this.$refs.input4.forEach(ele => {
            ele.checked = true
          });
          this.allchecked()
        }else {
          this.checked = false
          this.$refs.input3.forEach(ele => {
            ele.checked = false
          });
          this.$refs.input4.forEach(ele => {
            ele.checked = false
          });
        }
      })
      
    },
    checkedProvince(index) {
      this.threeMenu = index
      this.fourList = this.fourlists[index]
      this.$nextTick(()=>{
        if(this.$refs.input3[index].checked) {
          this.$refs.input4.forEach(ele => {
            ele.checked = true
          });
          let result2 = this.$refs.input3.every(el=>{
            return el.checked == true
          })
          if(result2) this.$refs.input2[this.twoMenu].checked = true
          this.allchecked()
        }else {
          this.$refs.input2[this.twoMenu].checked = false
          this.$refs.input4.forEach(ele => {
            ele.checked = false
          });
          this.checked = false
        }
      })
      
    },
    checkedCounty(index) {
      if(this.$refs.input4[index].checked) {
        let result3 = this.$refs.input4.every(el=>{
          return el.checked == true
        })
        if(result3) this.$refs.input3[this.threeMenu].checked = true;
        let result2 = this.$refs.input3.every(el=>{
          return el.checked == true
        })
        if(result2) this.$refs.input2[this.twoMenu].checked = true;
        this.allchecked()
      }else {
        this.$refs.input2[this.twoMenu].checked = false
        this.$refs.input3[this.threeMenu].checked = false
        this.checked = false
      }
    },
    allchecked() {
        let result1 = this.$refs.input2.every(el=>{
          return el.checked == true
        })
        let result2 = this.$refs.input3.every(el=>{
          return el.checked == true
        })
        let result3 = this.$refs.input4.every(el=>{
          return el.checked == true
        })
        console.log(result1,result2,result3)
        if(result1 && result2 && result3) this.checked = true
      
    }

  }
}
</script>


