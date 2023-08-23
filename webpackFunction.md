## require.context()
是webpack的方法，用于在编译时动态地引入模块
directory: 要搜索的目录
useSubdiretories: 是否搜索子目录
regExp：用于匹配文件的正则表达式

代码示例
const context = require.context('./directory', true, /\.js$/);
const keys = context.keys(); // 获取匹配到的模块路径数组
const module = context('./module.js'); // 获取对应模块的内容

## filter
过滤， 指创建一个新的数组，新数组中的元素是原数组中所有符合条件的元素
filter回调函数要求必须返回一个Boolean值
当返回的Boolean值为true时，函数内部会自动将回调的值加入数组中，反之，函数内部会过滤掉该值

代码示例 filter <=> for+if
const num = [1,2,3,4,2,34,55,0,33,25,8]
let filterArr = num.filter(function(n){
  reture n > 50
}) // [55] 

## map
返回一个新数组，新数组中的元素为原数组中元素调用函数处理后的值

代码示例
let mapArr = filterArr.map(function(n){
  return n * 2
})

## reduce
接受一个函数作为累加器，它可以对数组中所有内容进行汇总

代码示例
let total = mapArr.reduce(function(preValue, n){ 
  return preValue + n
})

const modulesFiles = require.context('./modules', true, /\.js$/)
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1') // set './app.js' => 'app'
  const value = modulesFiles(modulePath) // app.js function
  modules[moduleName] = value // {app: function}
  return modules
}, {})
const api = modules
export default api
