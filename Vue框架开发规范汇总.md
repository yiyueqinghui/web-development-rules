[TOCM]

# 一、Vue项目规范  ( 官方规范: https://cn.vuejs.org/v2/style-guide/ )
#### 1、组件规范
  - 组件name属性为多个单词,且采用大驼峰形式, eg:  export default { name: 'TodoItem'  }
  - 组件的引用名，采用大驼峰形式，eg: import EditDialog from '@/components/edit/edit-dialog.vue',在组件中调用也有此名
  - 组件文件名, 多个单词的, 全部采用小写方式，以中线分隔 格式 , eg: my-component.vue
  - 和父组件紧密耦合的子组件应该以父组件名作为前缀命名, eg: todo-list.vue / user-profile-options.vue
  - Props 定义应该尽量详细,必须指定添加注释说明其作用、指定类型、添加 default ，以保证组件的独立性，

      ```
      props:{ 
        userInfo :{ 
          type:Object,
          default:()=>{
             return {
               name:'',
               password:''
             }
          }}
        }
      ```
  - 为组件样式设置作用域,使用vue自带的scoped属性。 eg: <style scoped>.....</style>
  - 写全局样式时，在其前面添加唯一的父类/祖先选择器，以限制样式的范围，防污染全局
  - 必须为 v-for 设置键值 key
  - v-show 与 v-if 选择, 如果运行时，需要非常频繁地切换，使用 v-show ；如果在运行时，条件很少改变，使用 v-if
  - 指令缩写 (用 : 表示 v-bind:、用 @ 表示 v-on: 和用 # 表示 v-slot:) 
  - Vue Router 规范
      - 路由配置, 使用路由懒加载（延迟加载）机制 --- import()
        ```
        { 
           path: '/upload', 
           name: 'uploadt', 
           meta: { 
             title: '上传附件' 
           },
           component: () => import('@/view/components/uploadAttachment/index.vue') 
        },
        ```
      - name字段命名规范采用大驼峰命名规范且和 component 组件名保持一致！(因为要保持 keep-alive 特性，keep-alive 按照 component 的 name 进行缓存，所以两者必须高度保持一致)

#### 2、目录结构
  - src
       - api (所有api接口) 
         - 同一业务接口,放到一个api.js文件下,尽量后端一个controller 一个api.js文件
         - api中的方法名字与后端api url 尽量保持语义的一致性
         - api中的第一个方法要添加注释说明其作用
         - 示例：
         ```
           后端 url： EmployeeController.java 
           /employee/add 
           /employee/delete/{id} 
           /employee/update 
           
           前端： employee.js 
           // 添加员工 
           function addEmployee(data) {
              return postAxios('/employee/add', data) 
           }
           // 更新员工信息 
           function updateEmployee(data) { 
             return postAxios('/employee/update', data) 
           }
           // 删除员工 
           function deleteEmployee(employeeId) {
              return postAxios('/employee/delete/' + employeeId) 
           }
         ```
       - filter (公用的过滤器)
       - components(存放多次复用的公共组件)  
         - commonComp(存在复用，但复用次数较少,可不必存放入全局)
         - globalComp(将多次复用的组件install到全局)
           - index.js (配制全局组件)
           - components (全局组件的存入位置)   

       - view(视图)
         - 目录1 (注意：此目录名与此页面的路由地址相同，以方便查找，eg:路由地址是'/home',则这个目录名就是home)
           - index.vue  (该页面的入口组件)
           - 目录1-1 (仅限该页面使用的组件，如果存在复用，则统一放入components中)
         - 目录2
           - index.vue  
         - ......  

       - router(路由管理)
         - index.js(旋转全局路由的配置)

       - store(全局状态管理，统一采用vuex，如果状态过多，自行划分模块管理)
         - index.js

       - utils(存放通用工具，比如,对axios请求封装、时间日期格式化等方法,这里把异步请求单独拿出来)
         - common.js(常用全局方法)
         - http.js(axios请求封装)

    - static    
      - css 
        - common.css(全局样式)
        - ...... (第三方样式，或自定义样式)
        - 字体图标，推荐使用阿里图标库中的图标；

      - img
        - 按路由划分，根据路由，新建目录存图片资源
       - font(图标库，建议统一使用阿里图标库)
	 - ...  

#### 3、注释说明 
    - vue 文件的 methods，每个 method 必须添加注释
    - vue 文件的 data, 非常见单词要加注释
    - api 目录的接口 js 文件必须加注释

#### 4、其他
    - 尽量不要手动操作 DOM
    - 无用代码必须及时删除  

# 二、SVN仓库提交管理
	- 每次修改提交，必须写注释说明；
    - 注意提交时, node_modules、打包后的文件(比如：dist) 不需要提交到仓库内，设置忽略即可；
	  SVN忽略设置的方法：https://blog.csdn.net/yiyueqinghui/article/details/116046034

# 三、多个项目公共组件的管理
  - 文件参考svn地址：http://10.2.25.199/svn/zxy/01开发域/03编码实现/源代码/03业务系统/trunk/stcc-tocc-business-bigscreen-center/commonVue

  - 新建公共组件并发布到npm的步骤如下:
    - 1、创建组件
      - 1.1、在src/components目录下新建目录，并创建自己的组件
      - 1.2、之后可以在App.vue中调用自定义的组件并测试
      - 1.3、在index.js中引入自定义的组件,并导出。参考login组件即可；
    
    - 2、发布组件到npm
      - 2.1、去 npm 官网注册个账号 https://www.npmjs.com/
      - 2.2、运行npm login，会提示输入个人信息，输入邮箱后，会自动给这个邮箱发送一个邮件，通知发布成功，输入完，登录成功。
      - 2.3、发布之前的检查包名是否已被占用，package.json中的name值,即你发布后npm安装时的包名，
        可通过在https://www.npmjs.com/ 中输入name值，查看是否可以搜索到,如果可查寻到，再次修改包名
      - 2.4、发布：npm publish  (注意：每次发布的时候package.json 里面的 version不能一样，不然不能发布出去，手动改下版本就行)
      - 2.5、登录npm官网，查看自己发布的组件
      - 2.6、更新时，修改package.json 里面的 version ，再次发布即可

    - 3、安装并引用
      - 3.1、安装：npm install 你自己发布的包名
      - 3.2、在main.js中引入。eg
      ```
        import GckjUI from 'gckj-ui';
        import 'gckj-ui/src/asset/css/common.css'
        Vue.use(GckjUI)
      ```
      - 3.3、调用，在任意组件中调用时添加前缀gc即可，比如login组件
      ```
         <gc-login></gc-login>
      ```