# javaScript模块规范

## amd && commonJS && CMD && UMD
- 首先amd只是模块规范，定义了其语法API，是 RequireJS 在推广过程中对模块定义的规范化产出。目前实现了amd规范的javaScript库有：requirejs和curl.js。其内容为: [https://github.com/amdjs/amdjs-api/wiki/AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)
```
define([module-name?], [array-of-dependencies?], [module-factory-or-object]);
```

- commonJS也是一个规范，包含了module,package,system,binary(二进制),console,encoding,fs,sockets,unit testing等。它规定：一个文件就是一个模块，加载模块使用require该方法读取文件并执行，返回文件内部的exports对象，如果需要返回的不是对象而是函数则对exports重新赋值：
```
module.exports=function(){
  console.log('test');
}
```

CMD也是一个规范：是 SeaJS 在推广过程中对模块定义的规范化产出。[https://github.com/seajs/seajs/issues/242](https://github.com/seajs/seajs/issues/242)
exports 仅仅是 module.exports 的一个引用。在 factory 内部给 exports 重新赋值时，并不会改变 module.exports 的值。因此给 exports 赋值是无效的，不能用来更改模块接口。

![https://s17.mogucdn.com/mlcdn/c45406/180411_35d9il9ch6e1gc6gb97bebj5gejk4_1407x577.jpg](前端模块规范)

- UMD:从上图可以看出如果用CommonJS写的模块用在AMD的项目中是不行的，为了让AMD和CommonJS和谐共处，并支持global变量的形式，出现了UMD模式


### CommonJS 
- CommonJS 模块规范 每个文件里面定义的变量，函数，类都是私有的，除非定义为global对象属性。否则对于其他文件不可见
- module变量代表当前模块，该变量是一个对象，他的属性export是对外的接口。
- require对应的用于加载模块，其加载的其实就是export属性。

  ### CommonJS特点：
  - 1.所有代码运行在模块作用域不会污染全局。
  - 2.按模块加载顺序执行。
  - 3.同一个模块只会在第一次加载的时候执行一次，之后结果会被缓存，以后再记载使用的是缓存数据。如果想让模块再次执行必须先清除缓存。
  ```
  require('./example.js');
  delete require.cache('./example.js');
  ```

  - 4.CommomJS加载模块是同步的。

  ### module对象
  属性：id,filename,children,parent,loaded,exports,path
  ```
  const jquery = require('jquery');
  exports.$ = jquery;
  ```

  ### exports变量
  - Node会为每个模块提供一个exports变量
  ```
  var exports = module.exports;
  ```

## CMD&&AMD比较
CMD 推崇依赖就近，AMD 推崇依赖前置，AMD会在开始就require依赖，而CMD则是需要的时候require



## 兼容Node,AMD,CMD以及浏览器的浏览器环境设置
```
(function(name, root, factory) {
    //amd or cmd
    if (typeof define === 'function') {
        define(name, ['b'], factory)
    } else if (typeof module === 'object' && module.exports) { //node
        factory(require('b'));
    } else { //global
        root[name] = factory(root.b);
    }
})('demo', typeof self !== "undefined" ? self : this, function(b) {
    return {};
});
```

