<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <h1 class="T">Vue-Giant-Tree 
      <a href="https://github.com/tower1229/Vue-Giant-Tree" target="_blank" titie="Vue-Giant-Tree">
        <svg class="octicon octicon-mark-github v-align-middle" height="32" viewBox="0 0 16 16" version="1.1" width="32" aria-hidden="true"><path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"></path></svg>
      </a>
    </h1>
    
    <div class="wrap">
      <div class="c">
        <tree 
          :setting="setting" 
          :nodes="nodes" 
          @onClick="onClick" 
          @onCheck="onCheck" 
          @onCreated="handleCreated" />
      </div>
    </div>

    <div class="toolbar">
      <button class="btn" type="button" @click="update">更新数据</button>
    </div>
  </div>
</template>

<script>
const bigData = require("@/mock/big-tree.json");
const simpleData = [
  { id:1, pid:0, name:"随意勾选 1", open:true},
  { id:11, pid:1, name:"随意勾选 1-1", open:true},
  { id:111, pid:11, name:"随意勾选 1-1-1"},
  { id:112, pid:11, name:"随意勾选 1-1-2"},
  { id:12, pid:1, name:"随意勾选 1-2", open:true},
  { id:121, pid:12, name:"随意勾选 1-2-1"},
  { id:122, pid:12, name:"随意勾选 1-2-2"},
  { id:2, pid:0, name:"随意勾选 2", checked:true, open:true},
  { id:21, pid:2, name:"随意勾选 2-1"},
  { id:22, pid:2, name:"随意勾选 2-2", open:true},
  { id:221, pid:22, name:"随意勾选 2-2-1", checked:true},
  { id:222, pid:22, name:"随意勾选 2-2-2"},
  { id:23, pid:2, name:"随意勾选 2-3"}
];
const dataQueue = [bigData.data, simpleData]

export default {
  name: "app",
  components: {
    tree: resolve => require(["./components/ztree.vue"], resolve)
  },
  data() {
    return {
      showIndex: 0,
      setting: {
        check: {
          enable: true
        },
        data: {
          simpleData: {
            enable: true,
            pIdKey: "pid"
          }
        }
      }
    };
  },
  computed: {
    nodes: function(){
      return dataQueue[this.showIndex]
    }
  },
  methods: {
    onClick: function(evt, treeId, treeNode) {
      // 点击事件
      console.log(evt.type, treeNode);
    },
    onCheck: function(evt, treeId, treeNode) {
      // 选中事件
      console.log(evt.type, treeNode);
    },
    handleCreated: function(ztreeObj){
      // onCreated 中操作ztreeObj对象展开第一个节点
      ztreeObj.expandNode(ztreeObj.getNodes()[0], true)
    },
    update: function(){
      // 更新示例数据
      this.showIndex = this.showIndex === 0 ? 1 : 0;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.wrap {
  text-align: center;
}
.c {
  display: inline-block;
  width: 600px;
  height: 400px;
  overflow: auto;
  margin: 0 10px;
}
.T{
  font-size: 34px;
  margin-bottom: 44px;
}
.toolbar{
  margin: 20px auto;
}
.toolbar .btn{
  padding: .5em 1em;
  outline: none;
  border-radius: 4px;
}

</style>
