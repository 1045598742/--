# Vue实战总结

### sync修饰符

```vue
//在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为   子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。

//注意带有 .sync 修饰符的 v-bind 不能和表达式一起使用 (例如 v-bind:title.sync=”doc.title + ‘!’” 是无   效的)。取而代之的是，你只能提供你想要绑定的属性名，类似 v-model。

//将 v-bind.sync 用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，是无法正常工作的，   因为在解析一个像这样的复杂表达式的时候，有很多边缘情况需要考虑。

//父组件
<asyc a='a' b='b' c='c' :val.sync=val></asyc>
//子组件
<el-input v-model="input" placeholder="请输入内容"></el-input>
<el-button @click="$emit('update:val',input)">点击</el-button>
//子组件相当于绑定了一个update事件,将子组件的input值传回给父组件改变父组件val的值
```

### $props  

```vue
//当前组件接收到的 props 对象。Vue 实例代理了对其 props 对象属性的访问。
可用作父组件传值，子组件this.$props接收(也就是组件的props对象)

//父组件
<child1 :a='a' b='b' c='c' @h='h' @test1="onTest1" placeholder="请输入你的姓名" type="text" title="姓名" v-model="name" />

//子组件
<input v-bind="$props">
props:['placeholder','type']
//通过v-bind绑定相当于 <input type="text" placeholder="请输入你的姓名"/>

```

### $attrs

```vue
//其实用法个人感觉和$props差不多,只不过$attrs是父组件传过来的值的排出$props的其他部分

//父组件
<myinput placeholder="请输入你的姓名" type="text" title="姓名" v-model="name"/>

//子组件
<input v-bind="$attrsAll" @input="$emit('input',$event.target.value)"/>
<scrpit>
    inheritAttrs:false,//这个属性是避免绑定的属性在渲染时候绑在组件根节点标签上，false可以去掉他们
	computed: {
        $attrsAll() {
            return {
                value: this.$vnode.data.model.value,
                ...this.$attrs
            }
        }
    }
</scrpit>

//同时 $props 和 $attrs可以继续给孙组件传递
//子组件里的孙组件写法
<child3 v-bind="{...$props,...$attrs}"></child3>
//孙组件的接法和组件一样,具体做什么就灵活运用，可以配合$listeners一起
```

### $listeners

```vue
//可以进行简单的元素层次嵌套的事件回传 子组件接收的$listeners对象就是父组件传过来的v-on事件的集合，和$props 和 $attrs一样，可以传给子组件依次，孙组件可以$emit到祖父级组件

//父组件里引入的子组件
<child @a='a' @b='b' hehe='hehe'></child>
<scrpit>
    methods:{
        a(){
			console.log(1)
        },
        b(){
                console.log(2)
            }
    }
</scrpit>

//子组件
<div  v-bind="$attrs" @c='c'  @click='$emit("a")'>点我试试</div>
//子组件里的孙组件
<child2 v-on="$listeners"></child2>
//这样写就是把父组件传过来的a和b继续给了孙组件 孙组件可以直接拿来$emit("a")到祖父,因为子组件加了个c事件，所以孙组件里也可以$emit("c")到父组件
```

### v-model

```vue
//v-model不只可以在表单上面使用，可以使用在组件上

<!-- parent -->
<template>
<div class="parent">
  <p>我是父亲, 对儿子说： {{sthGiveChild}}</p>
  <Child v-model="sthGiveChild"></Child>
</div>
</template>
<script>
import Child from './Child.vue';
export default {
  data() {
    return {
      sthGiveChild: '给你100块'
    };
  },
  components: {
    Child
  }
}
</script>

<!-- child -->
<template>
<div class="child">
  <p>我是儿子，父亲对我说： {{give}}</p>
  <a href="javascript:;"rel="external nofollow" @click="returnBackFn">回应</a>
</div>
</template>
<script>
export default {
  props: {
    give: String
  },
  model: {
    prop: 'give',
    event: 'returnBack'//这个event是改变emit回父组件的事件名（默认为'input'）
  },
  methods: {
    returnBackFn() {
      this.$emit('returnBack','还你200块');
    }
  }
}
</script>

```

### v-slot

```vue
<!-- 以后最好使用v-slot因为之前的slot快废弃了，v-slot的简写是#号(v-slot='a'相当于#a) -->

<!-- 父组件 -->
<hello>
    <!-- 注意：这块的 v-slot 指令只能写在 template 标签上面，而不能放置到 p 标签上 -->
    <!-- <template #header>
              <p>我是头部</p>
            </template>
             <template #footer>
              <p>我是底部</p>
            </template> 
     -->
    
    <!--下面是子组件循环的情况-->
    <template #text='hh'>
      <template v-if="hh.i==1"> <!-- 如果循环的i是1我们对这一项做点什么-->
        <div>11{{hh.firstName}}</div>
      <div>11{{hh.lastName}}</div>
      </template>
      <template v-else> <!-- 其他情况正常渲染-->
          <div>{{hh.firstName}}</div>
          <div>{{hh.lastName}}</div>
      </template>
    </template>
  </hello>

<!-- 子组件 -->
	<div v-for="i in 4" :key="i">
        <slot name="text" :i='i' :firstName="firstName" :lastName="lastName"></slot>
	</div>

```

### $refs

```vue
<!-- 父组件 -->
<child1 ref='a' ></child1>
<child2 ref='b' ></child2>
<div @click='$refs.a.methods'></div><!-- 可以操作子组件的方法和改变子组件的值 -->

```

### watch

```vue
<script>
    export default{
        watch:{
           a(val, oldVal){//普通的watch监听
         	console.log("a: "+val, oldVal);
           },
           b:{//深度监听，可监听到对象、数组的变化
            handler(val, oldVal){
              console.log("b.c: "+val.c, oldVal.c);//但是这两个值打印出来却都是一样的
             },
            deep:true,//为了发现对象内部值的变化，可以在选项参数中指定 deep: true 。注意监听数组的变动                         不需要这么做
            immediate: true//页面刚进来就监听需要加这个属性，否则兼听不到初始值第一次的变化
           }
        }
      }
   }
</script>
```

