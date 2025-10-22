<template>
     <div class="main">
        <!-- 定义左侧部分的菜单 -->
        <div class="left">
            <ul style="margin: 0;padding: 0;">
                <!-- 使用v-bind指令未class属性绑定属性值,使用v-on指令监听并处理单击事件,实现单击<li>标签时,为其添加CSS样式 -->
                <li id="li-all" class="menu-item" @click="changeMenu(0)" :class="{'text-color':selectedType===0}" >
                    <span>全部教程</span> 
                </li>
                <!-- 使用v-for指令循环渲染列表，使用v-bind指令为class属性绑定属性值,使用v-on指令监听并处理单击事件,实现单击<li>标签时,为其添加CSS样式 -->
                <li id="li-list" class="menu-item" v-for="item in list" @click="changeMenu(item.type)" :class="{'text-color':selectedType===item.type}">
                    <!-- 教程类型名称 -->
                    <span>{{ item.name }}</span> 
                </li>
            </ul>
        </div>
        <!-- 右侧显示菜单对应的教程 -->
        <div class="right">
            <!-- 实现查询功能 -->
            <div class="header-s">
                <p>  
                    <input id="input" class="input" type="text" placeholder="请输入要查询的教程" v-model="select"/>
                    <button id="select" @click="selectItem" >搜 索</button>
                </p >
            </div>
            <!-- 显示菜单对应的教程 -->
            <ul >
                <!-- 使用v-show指令有条件的显示或隐藏数据 -->
                <li id="li-right" class="right-list" v-for="items in list" :key="items.type" v-show="menuShow(items.type)" >
                    <!-- 显示教程类型名称 -->
                    <h1>{{ items.name }}</h1>
                    <ul class="items">
                        <!-- 显示对应类型的所有教程 -->
                        <!-- 使用v-for指令循环渲染 -->
                        <li id="li-item" class="item" v-for="item in items.list" v-show="ifShow(items.type,item.id)"  >
                            <div class="img">
                                <!-- 显示教程的图标 -->
                                <img class="img" :src="geturl(item.image)">
                            </div>
                            <!-- 显示教程的名称和简介 -->
                            <div class="context">
                                <h3 class="text">{{item.id_name}}教程</h3>
                                <p>{{ item.message }}</p >
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import {ref} from 'vue'
import list from '../assets/json/list.json'     //导入数据
const select = ref('')      //单行文本框的内容
const selectId = ref([])    //搜索结果,数组中的对象为教程类型和对应类型中符合条件的教程id
const selectedType = ref(0)     //选中的教程类型,默认为0,即全部教程
// 获取图片对应的URL
const geturl = (img) =>{
    return new URL(img,import.meta.url).href
}
// 实现查询功能
const selectItem = () =>{
    // 当使用搜索功能时,选中的教程类型变为0
    selectedType.value = 0
    selectId.value = []     //每此查询教程之前,先清空上次的查询结果
    if(select.value === ''){    //若单行文本框的内容为空,则显示全部教程
        selectedType.value = 0
    } else{
        for(let items of list){     
            for(let item of items.list){ 
                // 若当前遍历的教程名称包含输入的查询内容,则符合条件   
                if(item.id_name.includes(select.value)){
                    // 将教程类型和教程id作为一个对象添加到数组selectId中
                    selectId.value.push({type:items.type,id:item.id})
                }
            }
        }
    }
}
// 判断是否显示该教程
const ifShow = (type,id) =>{
    // 若selectId数组的长度大于0
    if(selectId.value.length>0){
        var flag = false
        for(let item of selectId.value){
            if(item.type === type && item.id === id){
                flag = true
            } 
        }
        return flag

    } else{     //若selectId数组的长度为0,则显示该教程
        return true
    }
}
// 判断是否显示对应type的教程类型
const menuShow = (type) =>{
    // 若selectId数组的长度大于0
    if(selectId.value.length>0){
        var flag = false
        for(let item of selectId.value){
            // 若当前遍历的教程类型与type相等,则显示
            if(item.type === type){
                flag = true
            }
        }
        return flag
    } else{     //若selectId数组的长度为0,则当选中的教程类型为当前教程类型或selectedType的值为0时,显示该教程类型
        return selectedType.value===type||selectedType.value ===0
    }
}
// 切换选中的菜单
const changeMenu = (value) =>{
    //修改selectedType的值为当前选中的教程类型
    selectedType.value = value
    // 设置selectId的值为空
    selectId.value = []
}
</script>
<style src="../assets/CSS/main.css" scoped></style>