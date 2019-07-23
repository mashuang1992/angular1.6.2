# angular 1.6.2基础
```javascript
(function (angular) {
  'use strict';
  // 1. 创建模块
  // 2. 基于模块创建控制器
  // 3. 将模块和控制器作用到具体的HTML元素上
  // 4. 基于视图业务划分不同的控制器，设计你的数据模型
  angular.module('TodoApp', [])
    .controller('MainController', function ($scope) {
      $scope.title = '任务清单'
      $scope.todoText = ''
      $scope.todos = [
        { title: '吃饭', completed: false },
        { title: '睡觉', completed: true },
        { title: '打豆豆', completed: false }
      ]

      $scope.addTodo = function () {
      	if ($scope.todoText.trim() === '') {
      		return
      	}
        $scope.todos.push({
          title: $scope.todoText,
          completed: false
        })
        $scope.todoText = ''
      }
    })
})(window.angular);

```
- JSON 序列化
  + JSON.parse(String)
  + JSON.stringify(Object)
  
# 1.ng-model

- ng-model 在 ng 中也被称之为一个指令
    该指令一般用于和表单控件（input、text、checkbox、radio、select、textarea。。）进行模型数据绑定，
    双向数据绑定：
      这里就是将对象中的 foo 属性绑定到了 input 的 value 上
      当 input 的 value 改变的时候，数据对象上的 foo 也跟着变
      当 数据对象上的 value 发生改变的时候，把所有引用或者绑定了我 foo 属性的地方同步改变过去
      
```
	<input type="text" ng-model="foo">
  <input type="text" ng-model="foo">
  <p>{{ foo }}</p>
  
```

# 2.ng-bind

- ng-bind 可以解决表达式闪烁的问题
    表达式如何使用，ng-bind 就如何使用
      
```
	<p ng-bind="'hello world'"></p>
  <p>{{ 'Hello World' }}</p>
  
```
# 3.ng-bind

- ng-bind 可以解决表达式闪烁的问题
    表达式如何使用，ng-bind 就如何使用
      
```
	<p ng-bind="'hello world'"></p>
  <p>{{ 'Hello World' }}</p>
  
```

# 4.ng-repeat
```
<li ng-repeat="fruit in fruits track by $index">{{ fruit }}</li>

```
# 5.ng-class
```
	ng-class 指令专门可以解决样式的问题
  ng-class="{css类名: bool}"
     当 bool 为 true，作用 css类
     当 bool 为 false，取消 css 类
```
# 6.$templateCache 模板缓存

```
angular.module('app', ['ngRoute'])
	.run(['$templateCache', function($templateCache){
	  // 下一步：将所有的 HTML 视图模板文件读取出来，生成下面一坨这样的代码
	  $templateCache.put('./home.html', '<p>这是首页</p>')
	  $templateCache.put('./login.html', '<p>登陆</p><h1>登陆</h1>')
	}])

```

# 7.组件通信

## 父子通信

```
angular.module('DemoApp', [])
      .controller('ParentController', ['$scope', function ($scope) {
        $scope.food = ''
        // 为父亲添加或者说注册一个事件
        $scope.$on('chifan', function (event, data) {
          $scope.food = data
          console.log('ParentController 孩子通知父亲该吃饭了')
          	
          	//!!!!!!!!!!
          	$scope.$broadcast('father', '不错') //父亲通知孩子
          
        })
      }])
      .controller('SubController', ['$scope', function ($scope) {
        $scope.eat = function () {
        
        //!!!!!!!!!!
          $scope.$emit('chifan', '面条')  //孩子通知父亲
        }
        $scope.$on('father', function (event, data) {
          console.log('爸爸说：' + data)
        })
      }])

```

## 兄弟之前的通信

```
通过$rootscope 用法如上

```

# 与路由相关的一些配套 API

- ngView
  + 留坑，标记被路由替换处理的位置
- $routeProvider
  + 配置路由表
  + 根据不同的请求路径执行对应的控制器及渲染对应的HTML视图模板
  + when(path: String, route: Object)
    * route
      - controller
      - template
      - templateUrl
      - redirectTo
  + otherwise(params: Object | String)
- $route
  + $route.reload() 重载当前路由
  + $route.updateParams(Object) 更新路由中的动态路径参数
- $routeParams
  + 用来获取路由参数
  + 例如：/user/:id 是一个动态路径，则可以在控制器中通过 $routeParams 获取 :id 的值
  + 也可以获取查询字符串中的参数，例如请求路径是 /user?id=1 则通过 $routeParams 也可以获取到

## Controller as vm

ng 1.2 引入了新的语法：`Controller as`。
在此之前的版本需要在 Controller 中注入 `$scope` 这个视图模型服务对象，才能在视图绑定中使用这些变量。
