# **说一下vue实例的生命周期？**

new Vue() > 初始化事件和生命周期 > beforeCreate > 数据绑定 > created > 如果有el属性，则编译模板 > beforeMount > 添加$el属性，并替换掉挂载的DOM元素 > mounted > 数据更新 > beforeUpdate > 重新编译模板并渲染DOM > updated > 调用$destoryed > beforeDestory > 销毁vue实例 > destroyed

# **react中props和state有什么区别？**

react是单向数据流，props是一个父组件传递给子组件的数据流接口，可以一直的被传递到子孙组件中，在组件内部的props是只读的。而state是组件内部私有的状态，通过修改state会触发组件的重新渲染。



## vue 父子组件嵌套时，组件内部的各个生命周期钩子触发先后顺序

实例初始化前 beforecreated-------------------->

创建之前--------------------------------->

实例初始化完成 虚拟DOM 是不存在的 create-------------------->

实例创建后--------------------------->

实例挂载前 虚拟dom已经生成beforecreate---------------->

