<template>
  <div id="app">
    <mt-button type="danger" @click="showPiker">select city</mt-button>
    <mt-picker v-if="showFlag" :slots="slots" @change="onValuesChange"></mt-picker>
  </div>
</template>

<script>
import cityName from '../public/address3.json'
export default {
  name: 'app',
  data(){
    return{
      showFlag:true,
      slots: [
        {
          flex: 1,
          values: Object.keys(cityName),
          className: 'slot1',
          textAlign: 'right'
        }, 
        {
          flex: 1,
          className: 'slot2',
          values: [],
          textAlign: 'center'
        }, 
        {
          flex: 1,
          values: [],
          className: 'slot3',
          textAlign: 'left'
        }
      ]
    };
  },
  mounted () {
    const citys = '北京市 北京 昌平区';
    const resCity = citys.split(' ');
    const sheng1 = resCity[0];
    const shi1 = resCity[1];
    const qu1 = resCity[2];
    
    this.$nextTick(() => {
      let sheng = this.getNumber(Object.keys(cityName),sheng1);
      let shi = this.getNumber(Object.keys(cityName[this.slots[0].values[sheng]]),shi1);
      let qu = this.getNumber(Object.values(cityName[this.slots[0].values[sheng]])[shi],qu1);

      this.slots[0].defaultIndex = sheng; 
      this.slots[1].defaultIndex = shi; 
      this.slots[2].defaultIndex = qu; 

      this.slots[1].values = Object.keys(cityName[this.slots[0].values[sheng]])
      this.slots[2].values = Object.values(cityName[this.slots[0].values[sheng]])[shi]
    });
  },
  methods: {
    getNumber(arr,val){
      let numbers;
      arr.some((item,i) => {
        if(item === val){
          numbers = i;
          return true;
        }
      })
      return numbers;
    },
    showPiker:function(){
      this.showFlag = !this.showFlag
    },
    onValuesChange(picker, values) {
      if(cityName[values[0]]){
        picker.setSlotValues(1,Object.keys(cityName[values[0]])); // Object.keys()会返回一个数组，当前省的数组
        picker.setSlotValues(2,cityName[values[0]][values[1]]); // 区/县数据就是一个数组
      }
    }
  }
}
</script>

<style>
*{
  margin:0;
  padding:0;
}
body{
  margin:50px;
}
</style>
