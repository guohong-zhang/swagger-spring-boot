<template xmlns="http://www.w3.org/1999/xhtml" xmlns:v-bind="http://www.w3.org/1999/xhtml">
  <div  class="tree-menu" :class="{menuBorder:isResponse}">
     <li v-if="isResponse" :class="{fontColor:properties}" @click="toggleChildren" class="table-tr">
       <span :class="{fontRight:depth>0}" class="table-td-md">{{item.name ? item.name : (keyTo ? keyTo : "无")}}</span>
       <span class="table-td-md">{{item.type ? item.type : "无"}}</span>
       <span class="table-td-md">{{item.description ? item.description : "无"}}</span>
     </li>
     <li :class="{fontColor:properties}" @click="toggleChildren" class="table-tr" v-else>
       <span :class="{fontRight:depth>0}" class="table-td">{{item.name ? item.name : (keyTo ? keyTo : "无")}}</span>
       <span class="table-td">{{item.description ? item.description : "无"}}</span>
       <span class="table-td">{{item.type}}</span>
       <span class="table-td">无</span>
       <span class="table-td">{{item.in ? item.in : ""}}</span>
       <span class="table-td">{{isRequired}}</span>
     </li>
    <transition-group  name="slide-fade" tag="ul">
      <form-fold :name="name" :key="key" :depth="depth + 1" v-show="showChildren"
                 v-for="(item,key) in childProperties" :requireds="requireds"
                 :item="item" :keyTo="key" :properties="item.properties">
      </form-fold>
    </transition-group>
  </div>
</template>
<script>
  export default {
    props: ['name', 'item', 'properties', 'keyTo', 'depth','requireds'],
    name: 'form-fold',
    data() {
      return {showChildren: false}
    },
    computed: {
      isRequired(){
        if(this.item.required&&typeof this.item.required === 'boolean'&&this.item.required||(typeof this.item.required === 'object'&&this.item.required['length']>0)){
          return true;
        }
        if(this.requireds&&typeof this.requireds === 'object'&&this.requireds['length']>0&&(this.requireds.includes(this.item.name)||this.requireds.includes(this.keyTo))){
          return true;
        }
        return false;
      },
      isResponse() {
        return this.name === 'response';
      },
      childProperties() {
        if (this.properties && ((this.properties["length"]) || (typeof this.properties) === "array")) {
          return this.properties[0] && this.properties[0].properties;
        }
        return this.properties;
      },
      indent() {
        return {textAlign: `right`}
      },
    },
    methods: {
      toggleChildren() {
        this.showChildren = !this.showChildren;
      }
    }
  }
</script>
<style>
  .hides {
    display: none;
  }

  .slide-fade-enter-active, .slide-fade-leave-active {
    transition: all .5s;
  }

  .slide-fade-enter, .slide-fade-leave-to {
    opacity: 0;
  }

  .copyTr {
    display: table-row;
    vertical-align: inherit;
    border-color: inherit;
  }

  .menuBorder {
    border-bottom: 0;
  }

  /* 请求参数表格 */
  .table-tr {
    border-bottom: 1px solid #ddd;
    overflow: hidden;
    word-wrap: break-word;
    padding-bottom: 8px;
  }

  .table-head {
    font-size: 16px;
    font-weight: 700;
    background-color: #F8F8F8;
  }

  .table-head .table-td {
    padding: 8px 4px 999px;
    text-align: center;
  }

  .table-td {
    border-right: 1px solid #ddd;
    width: 15%;
    float: left;
    padding: 8px 4px;
    /*min-height: 18px;
    text-align: left;*/
    min-height: 18.4px;
    text-align: left;
    padding-bottom: 999px;
    margin-bottom: -999px;
  }

  .table-td:nth-child(2) {
    width: 28%;
  }

  .table-td:nth-child(3) {
    width: 10%;
  }

  .table-td:nth-child(5) {
    width: 10%;
  }

  .table-td:last-child {
    border-right: 0;
    width: 12%;
  }

  .table-td-md {
    border-right: 1px solid #ddd;
    width: 30%;
    float: left;
    padding: 8px 4px;
    text-align: left;
  }

  .table-td-md:last-child {
    border-right: 0;
  }

  .table-tr .fontRight {
    text-align: right;
  }

  .fontColor {
    cursor: pointer;
    color: #4395ff;
  }
</style>
