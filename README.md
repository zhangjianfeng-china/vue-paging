# vue-paging
vue简单分页组件

将分页组件在全局注册

```
import pager from './components/pagination/src/pager.vue';
Vue.component('v-pagination', pager);
```

在组件中调用
```
<v-pagination :total="total" v-on:prev="prev" v-on:next="next" v-on:changePage="changePage"></v-pagination>
```

### 配置
    参数            说明            类型            可选值           默认值     
    total          总数据条数       Number          -----
    display        一页显示条数     Number          -----           10
    
### Event事件
    事件名称                说明                回调参数
     prev                 上一页回调函数         当前页
     next                 下一页回调函数         当前页
     changePage           页数改变时触发         当前页





最终效果

![](https://github.com/Essjs/vue-paging/blob/master/page.PNG)
