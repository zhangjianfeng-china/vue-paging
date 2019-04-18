<template>
  <ul class="pager" v-on:click="changPage">
    <li class="prev" v-on:click="prev">
      <button>
        <i class="fa fa-angle-left"></i>
      </button>
    </li>
    <li v-for="page in pagers_con" :key="page" :class="{active:page===1}">{{page}}</li>
    <li class="next" v-on:click="next">
      <button>
        <i class="fa fa-angle-right"></i>
      </button>
    </li>
  </ul>
</template>
<style scoped>
.pager {
  margin: 20px 0px;
  overflow: hidden;
  user-select: none;
}
.pager li {
  width: 30px;
  height: 28px;
  margin: 0 5px;
  background-color: #f4f4f5;
  float: left;
  text-align: center;
  line-height: 28px;
  cursor: pointer;
  color: #606266;
  font-size: 13px;
  font-weight: 700;
  border-radius: 4px;
}
.pager li:hover {
  color: #409eff;
}
.active {
  background-color: #409eff !important;
  color: #fff !important;
}
button {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  cursor: pointer;
  background: #f4f4f5;
}
</style>
<script>
export default {
  name: "",
  data() {
    return {
      /** 当前码数 */
      currentPage: 1,
      pagers_con: [] /** 分页条数数组 */,
      totalPage: Math.ceil(this.total / this.display) /** 总页数 */
    };
  },
  props: {
    /** 总条目数 */
    total: {
      type: Number,
      default: 0
    },
    /** 一页显示多少数据 */
    display: {
      type: Number,
      default: 10
    },
    /**分页条数 */
    pagegroup: {
      type: Number,
      default: 6
    }
  },
  methods: {
    prev() {
      console.log('p')
      /** 翻页操作 */
      // this.currentPage
      this.$emit("prev", this.currentPage);
    },
    next() {
      console.log('x')
      this.$emit("next", this.currentPage);
    },
    changPage(event) {
      const target = event.target;
      if (target.tagName == "UL") {
        return;
      }
      // ||
      //   target.className == "prev" ||
      //   target.className == "next"

      let newPage = Number(event.target.textContent); // 点击后新的页数
      if (isNaN(newPage)) {
        return;
      }
      /** 点击后添加选中样式 */
      $(".active").removeClass("active");
      event.target.className = "active";

      this.currentPage = newPage;

      var total = this.totalPage;
      /** 当总页数小于默认分页条数 直接返回出去 */
      if (total <= this.pagegroup + 1) {
        return;
      }
      /** 当点击的页数小于等于4时，保持不变 */
      if (newPage <= 4) {
        this.pagers_con = [1, 2, 3, 4, 5, 6, "...", total];
        console.log("等于4");
        return;
      }
      /** 当点击的页数大于等于5时，分页开始折叠 */
      if (newPage <= 5) {
        this.pagers_con = [1, "....", 3, 4, 5, 6, 7, "...", total];
        console.log("小于等于5");
        return;
      }
      /** 点击的页数大于等于总页数-3 说明已到设定的尽头 显示后面所有的页数 */
      if (newPage >= total - 3) {
        this.pagers_con = [
          1,
          "....",
          total - 5,
          total - 4,
          total - 3,
          total - 2,
          total - 1,
          total
        ];
        console.log("点击的页数大于等于总页数-3");
        return;
      }
      /** 以上条件都不满足 */
      this.pagers_con = [
        1,
        "....",
        newPage - 2,
        newPage - 1,
        newPage,
        newPage + 1,
        newPage + 2,
        "...",
        total
      ];
    }
  },
  mounted() {
    /** 注意：this.pagegroup + 1 是为了避免 1,2,3,4,5,6,...7 这种情况*/

    /** 当计算的分页数小于设定的分页条数则全部显示出来 */
    if (this.totalPage <= this.pagegroup + 1) {
      console.log(this.pagegroup);
      for (let i = 0; i < this.totalPage; i++) {
        this.pagers_con.push(i + 1);
      }
    }
    /** 当计算的分页数大于设定的分页数是*/
    if (this.totalPage > this.pagegroup + 1) {
      for (let i = 0; i < this.pagegroup; i++) {
        this.pagers_con.push(i + 1);
      }
      this.pagers_con.push("...");
      this.pagers_con.push(this.totalPage);
    }
  }
};
</script>


