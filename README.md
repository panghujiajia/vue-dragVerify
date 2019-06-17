# myComponents

#### 介绍
个人写的VUE组件

#### 使用说明

1. 把文件放入项目中并引入到项目
2. 注册组件并写入对应的组件标签进行使用
3. 传递相应的参数

##### 滑块验证组件
> `dragVerify.vue`
``` js
    import dragVerify from 'dragVerify'
    export default {
        components: { dragVerify },
        data() {
            return {
                options:{
                    //以下均为默认值，可不传
                    dragWidth: 350, //容器的宽度
                    dragHeight: 40, //容器的高度
                    ballWidth: 40, //滑块的宽度
                    defaultText: "滑动到右侧验证", //滑块滑动前的提示语
                    successText: "验证通过" //滑块滑动成功后的提示语
                },
                dragStatus:false,//滑块默认状态
            }
        }
    }
    <drag-verify v-bind="options" v-bind:status="dragStatus"></drag-verify>
```