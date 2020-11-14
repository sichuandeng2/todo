<template>
  <div id="app">

    <!-- 标题 -->
    <div class="app-title">
      <h3>代办事务</h3>
      <button @click="addAffair">添加事务</button>
      <button @click="sort">排序</button>
    </div>

    <!-- 事务 -->
    <div class="app-affair">
      <table>
       <tr>
         <th>序号</th>
         <th>代办事务名称</th>
         <th>代办事务内容</th>
         <th>时间</th>
         <th>状态</th>
         <th>删除</th>
       </tr>

        <!-- 数据事务渲染 -->
       <template v-for="(item,index) in totalAffair">
          <tr :key="item.id">
              <td>{{item.id}}</td>
              <td>{{item.affairName}}</td>
              <td>{{item.affairInner}}</td>
              <td>{{item.affairTime|timeformat(item.affairTime)}}</td>
              <td>
                <input type="checkbox" name="" id="" v-model=item.finished >{{item.finished?"已完成":"未完成"}}
              </td>
              <td>
                  <a href="#" @click.prevent="deleteAffair(index)">删除</a>
              </td>
          </tr>
      </template>

      <!-- 空行提示语 -->
      <tr v-if="totalAffair.length===0">
          <td colspan="6">未获取到代办事项</td>
      </tr>
      </table>
    </div>
  </div>
</template>

<style lang="scss" scope>
#app {
  font-family:微软雅黑, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: auto;
  width: 800px;
  border:1px solid #d6d622;

  //标题样式
  .app-title {
    button {
      float: right;
    }
  }

  //主题样式
  .app-affair{
  table{
    width: 100%;
    border-collapse: collapse;
    tr th,tr td{
      padding: 5px;
      border: 1px solid #000;
      text-align: center;
    }
  }

  }
}

</style>
<script>
export default {
  data() {
    return {

      //数据源
      totalAffair:[{
        id:1,
        affairName:"事务1",
        affairInner:"事务1的内容",
        affairTime:new Date(2020,11,10,11,10,21,110),
        finished:true
      },{
        id:2,
        affairName:"事务2",
        affairInner:"事务2的内容",
        affairTime:new Date(2020,9,13,11,10,21,110),
        finished:true
      },{
        id:3,
        affairName:"事务3",
        affairInner:"事务3的内容",
        affairTime:new Date(2020,11,14,11,10,21,110),
        finished:false
      }],

      //数据索引标识
      id:3,
    }
  },
  methods: {

    //新增事务
    addAffair() {
      this.totalAffair.unshift({
         id:++this.id,
        affairName:"事务"+this.id,
        affairInner:"事务"+this.id+"的内容",
        affairTime:new Date(),
      })
      this.sort();
    },

    //排序函数
    sort() {
      var data=this.totalAffair;
      data.sort((m,d)=>{
        let mdata = new Date(m.affairTime).getTime();
        let ddata = new Date(d.affairTime).getTime();
        return mdata-ddata;
      }).reverse();
      this.totalAffair = data;
    },
    //删除事务
    deleteAffair(index) {
      this.totalAffair.splice(index,1)
    }
  },
  filters: {
    //时间格式过滤器
    timeformat:function(value) {
      let date = new Date(value);
      let year = date.getFullYear();
      let mon = date.getMonth()+1;
      let day = date.getDate();
      let hour =date.getHours();
      let min = date.getMinutes();
      let second = date.getSeconds();
      return year+"年"+mon+"月"+day+"日 "+hour+"点" +min+"分"+second+"秒";
    }
  }
}
</script>
