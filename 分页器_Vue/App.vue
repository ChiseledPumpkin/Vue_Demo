<template>
<div id="app">
  <div class="container">
    <!-- 内容显示部分 -->
    <div class="content">
      <div class="item" v-for="(item,index) of nowShowData" :key="index"><i>{{item}}</i></div>
    </div>
    <!-- 分页部分 -->
    <div class="pagination">
      <!-- 上一页 -->
      <div class="prePage" :class="preBtnIsClick?'isClick':''" @click="prePage">上一页</div>
      <!-- 当前页码 前面的多余页码 以省略号隐藏-->
      <div class="pages page_dot" v-if="this.getShowPage[0]!=1">...</div>
      <!-- 页码 -->
      <div class="pages" v-for="(currentPageNumber,index) of getShowPage" :key="index" @click="page(currentPageNumber)"
        :class="currentPageNumber==currentPage?'active':''" >{{currentPageNumber}}</div>
      <!-- 当前页码 后面的多余页码 以省略号隐藏-->
      <div class="pages page_dot" v-if="this.totalPageNum!=this.getShowPage[this.getShowPage.length-1]">...</div>
      <!-- 下一页 -->
      <div class="nextPage" :class="nextBtnIsClick?'isClick':''" @click="nextPage">下一页</div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      // 假设这是后台传来的数据来源
      dataSourceArr: [],
      // 根据每页显示数量分割源数据，分割成每页要显示的数据
      totalPage: [],
      // 总页数
      totalPageNum: 1,
      // 每页显示多少数据
      pageSize: 4,
      // 当前显示的页，默认第一页
      currentPage: 1,
      // 当前页显示的数据
      nowShowData: [],

      // 最多显示的页码数，如果总页数大于此，多出的页码以省略号代替
      showMaxPageCount: 8,

      // 上一页按钮可点击状态，初始不可点击
      preBtnIsClick: false,
      // 下一页按钮可点击状态，初始可点击
      nextBtnIsClick: true,
    };
  },
  computed: {
    getShowPage(){
      let pageNumList = [];
      //最多显示的页码数
      if(this.totalPageNum > this.showMaxPageCount){
        for(let i=0; i < this.showMaxPageCount; i++){
          pageNumList[i] = i+1;
        }
        return pageNumList;
      }else{
        // 总页数小于最多显示的页码，不使用省略号
        for(let i = 0; i < this.totalPageNum; i++){
          pageNumList[i] = i+1;
        }
        return pageNumList;
      }
    }
  },
  mounted(){
    // 这里简单模拟一下后台传过来的数据
    for (let i = 0; i < 50; i++) {
      // this.dataSourceArr.push({ name: "A" ,look:"very handsome"});
      this.dataSourceArr.push(`测试${i}`);
    }

    //求总页数，Math.ceil()方法计算值 向上取整，舍弃小数部分 整数位进1，保险起见这里加了或运算(得0时设为1)
    this.totalPageNum = Math.ceil(this.dataSourceArr.length/this.pageSize) || 1;

    // 分割源数据 根据每页显示数量 将源数据分割到 每一页
    // 每一页都是一个数组 形如 [['第一页的数据'],['第二页的数据'],['第三页数据']]
    for (let i = 0; i < this.totalPageNum; i++) {
      this.totalPage[i] = this.dataSourceArr.slice(this.pageSize * i, this.pageSize * (i + 1))
    }

    // 分割完数据后显示第一页内容(此时currentPage值为1)
    this.page(this.currentPage);
  },
  methods: {
    // 根据传入的页码展示页码对应的页面数据
    page(currentPageNumber){
      // 根据传入的参数 设置 当前显示页码 
      this.currentPage=currentPageNumber;
      // 根据 当前显示页码，从分割好的数据中提取对应 当前页要显示的数据
      // 数组下标从0开始，这里要减1
      this.nowShowData = this.totalPage[this.currentPage-1];

      // 修改当前显示的所有页码(计算属性getShowPage)
      if (this.currentPage == this.getShowPage[this.getShowPage.length - 1] && this.totalPageNum > this.currentPage) {
        for (let i = 0; i < this.getShowPage.length; i++) {
          // 判断页码右侧是否到头
          if (this.totalPageNum - this.getShowPage[this.getShowPage.length - 1] < 2) {
            this.getShowPage[i] = this.getShowPage[i] + 1;
          } else {
            this.getShowPage[i] = this.getShowPage[i] + 2;
          }
        }
      }
      if (this.currentPage == this.getShowPage[0] && this.currentPage > 1) {
        for (let i = 0; i < this.getShowPage.length; i++) {
          // 判断页码左侧是否到头
          if (this.currentPage == 2) {
            this.getShowPage[i] = this.getShowPage[i] - 1;
          } else {
            this.getShowPage[i] = this.getShowPage[i] - 2;
          }
        }
      }






      // 另一种方式 加载时不分割数据，需要用到时直接从源数据中取
      // 根据当前页码 求要显示的数据的的第一个索引
      // let list = (this.currentPage-1)*this.pageSize;  
      // 根据索引 从源数据中取出当前页要显示的数据
      // this.nowShowData = this.dataSourceArr.slice(list,list+this.pageSize);


      // 按钮的可点击状态
      if(this.currentPage!=1){
        this.preBtnIsClick = true;
      } else {
        this.preBtnIsClick = false;
      }

      if(this.currentPage<this.totalPageNum){
        this.nextBtnIsClick = true;
      } else {
         this.nextBtnIsClick = false;
      }
    },
    // 上一页
    prePage(){
      if(this.currentPage!=1){
        this.currentPage--;
        this.page(this.currentPage);
        // 按钮的可点击状态
        this.nextBtnIsClick = true;
        if(this.currentPage===1) this.preBtnIsClick = false;
      } else {
        this.preBtnIsClick = false;
      }
    },
    // 下一页
    nextPage(){
      if(this.currentPage<this.totalPageNum){
        this.currentPage++;
        this.page(this.currentPage);
        // 按钮的可点击状态
        this.preBtnIsClick = true;
        if(this.currentPage===this.totalPageNum) this.nextBtnIsClick = false;
      } else {
         this.nextBtnIsClick = false;
      }
    }
  },
}
</script>

<style lang="less">
* {
	padding: 0;
	margin: 0;
	// 禁止元素被选中
	user-select: none;
}

body {
	// 弹性布局 让页面元素垂直+水平居中
	display: flex;
	justify-content: center;
	align-items: center;
	// 让页面占浏览器可视区域的高度
	height: 100vh;
	background-color: #f8f9ff;
}

// 定义公共颜色
@lightColor: #cdd5f7;
@darkColor: #646b8a;
@activeColor: #6b5bfd;

.container {
	display: flex;
	flex-direction: column;
	align-items: center;
	width: 600px;
}

.container .content{
  display: flex;
  // 允许换行
  flex-wrap: wrap;
}

.container .content .item {
  display: flex;
  // 主轴向下
  flex-direction: column;
  // 两端对齐
  justify-content: space-between;
  align-items: center;
  width: 100px;
  height: 100px;
  padding: 10px;
}

.container .content .item i {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 70px;
  height: 70px;
  // 圆角
  border-radius: 8px;
  font-size: 20px;
  color: @darkColor;
  background-color: #fff;
  // 鼠标移入变小手
  cursor: pointer;
  // 过渡
  transition: all 0.3s;
  // 阴影
  box-shadow: 0 16px 32px rgba(90, 100, 130, 0.1);
  &:hover {
    color: @activeColor;
    // 放大1.2倍
    transform: scale(1.2);
  }
}
// 这个样式没有用到
.container .content .item p {
  font-size: 13px;
  color: @lightColor;
}

.container .pagination {
  display: flex;
  align-items: center;
  height: 50px;
  margin: 30px;
  border-radius: 8px;
  background-color: #fff;
  box-shadow: 0 16px 32px rgba(90, 100, 130, 0.1);
}

.container .pagination .prePage,
.container .pagination .pages,
.container .pagination .nextPage {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 25px;
  height: 25px;
  margin: 10px;
  font-size: 14px;
  border-radius: 5px;
  transition: all 0.3s;
}

// 左右按钮
.container .pagination .prePage,
.container .pagination .nextPage{
  // 默认都为不可点击态
  color: @lightColor;
  width: 80px;
}

.isClick {
  // 可点击态
  // 这里权重不够，需要提权
  color: @darkColor !important;
  cursor: pointer;
}

.isClick:hover {
  color: #fff !important;
  background-color: @activeColor !important;
}

// 数字按钮
.container .pagination .pages {
  color: @darkColor;
  cursor: pointer;
}

.container .pagination .pages:hover {
  color: #fff;
  // 文字加粗
  font-weight: bold;
  background-color: @activeColor;
}

.active {
  // 这里权重不够，需要提权
  color: #fff !important;
  // 文字加粗
  font-weight: bold;
  background-color: @activeColor;
  // 鼠标变为默认箭头
  cursor: default !important;
}

// 省略号
.container .pagination .page_dot {
  cursor: default !important;
}
</style>
