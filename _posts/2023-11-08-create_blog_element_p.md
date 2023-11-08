---
layout: post
title: "Event-admin注册功能笔记"
date:   2023-11-08
tags: [code]
comments: true
author: huheshuangxing
---
  在写完静态页面后，就要实现登录/注册输入框时的校验，涉及到element组件内表单属性:**:model="formModel"**
  以下举一个注册时密码校验的例子:
  ```
  <el-form
        :model="formModel"
        :rules="rules"
        ref="form"
        size="large"
        autocomplete="off"
        v-if="isRegister"
      >
      ...//中间其余代码
    <el-form-item prop="password">
          <el-input
            v-model="formModel.password"
            :prefix-icon="Lock"
            type="password"
            placeholder="请输入密码"
          ></el-input>
    </el-form-item>
  </el-form>
```
  ***
 首先定义绑定整个注册表单里所有的属性(用户名，密码，再次确认密码)：
 使用**:model="formModel"**,formModel为自定义form数据对象内容为:
 ```
 const formModel = ref({
  username: '',
  password: '',
  repassword: ''
})
```
再定义整个表单的校验规则这里只举密码的规则例子：
```
const rules = {
  username: [
    { required: true, message: '请输入用户名', trigger: 'change' },
    { min: 5, max: 10, message: '用户名必须是5-10位字符', trigger: 'change' }
  ],
  password: [
    {
      required: true,
      message: '请输入密码',
      trigger: 'change'
    },
    {
      pattern: /^.(\S){6,15}$/,
      message: '密码必须是6-15位 的非空字符',
      trigger: 'change'
    }
  ]
}
```
以password里的内容来说，required属性代表非空校验，当校验不通过时，会使用message来告诉用户。
**trigger**属性为什么时候触发校验，blur代表当这个dom节点失焦时进行一次校验，change代表当这个dom里数据发生改变时进行校验。

在上面的代码里，创建好了formModel和rules后对整个`<el-form>`进行绑定
通过：
        :model="你定义的用于提交form数据对象的名字"
        :rules="你定义的规则"
之后对给表单元素，绑定form的子属性，这里是在`<el-input>`上使用`v-model="formModel.password"`(绑定了密码的属性)
最后在输入框外层的`<el-form-item>`上使用`prop`,
prop里面的内容为刚才定义的规则，这里就是`prop="password"`

---
### 总结
1.el-form => :model="ruleform" 绑定的整改form的数据对象 {    用户名  ，   密码   ，   确认密码  }
2.el-form => :rules="rules"    绑定的整个rules规则对象  {用户名的规则，密码的规则，确认密码的规则} 
3.表单元素 => v-model="ruleForm.xxx"  给表单元素，绑定form的子属性
4.el-form-item => prop配置生效的是哪个校验规则（和rules中的字段要对应）

























