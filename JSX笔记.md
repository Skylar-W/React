### JSX中使用JS表达式的注意点:
+ {}单大括号中可以使用任意的JS表达式.
+ JSX本身也是一个JS表达式.
+ JS对象不要用在JSX的{}单大括号中,样式style属性中可以使用.
+ if / for 之类的语句不要用在JSX中的{}里.

### JSX的条件渲染
+ if / else
+ 三目运算符
+ 逻辑运算符(短路与,短路或)

### JSX的列表渲染
+ 使用数组的map方法
+ 遍历谁,就给谁带上key属性,key值应该唯一
+ 避免使用索引号作为key

### JSX的样式
+ 行内样式
  <h1 style={{width: '200px', height: '50px', color: '#f61'}}>Hello JSX</h1>
+ 类名className添加样式(推荐写法!!)
  <h1 className="bgc352">这是一条JSX语法代码</h1>