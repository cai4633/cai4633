---
title: vue中computed属性的理解
date: 2019-11-02 00:25:19
categories: vue
tags: vue生命周期
---
computed是vue中一个重要的属性-**计算属性**。
## 基本用法：
{% codeblock lang:js %}
    computed: {
        test:function(){
            return this.param;
        }
    }
{% endcodeblock %}

## 进阶用法：
{% codeblock lang:js %}
    computed:{
        test:{
            get(){              //getter
                return this.param;
            },
            set(val){           //setter
                this.param = val;
            }
        }
    }
{% endcodeblock %}

##我的理解：
computed计算属性是在beforeCreate之后，created
之前这个时间段内***初始化***的。当前***依赖的初始值***决定了计算属性的的初始值。同时计算属性的结果会被缓存，除非之后依赖的响应式属性变化才会重新计算。注意，如果某个依赖 (比如非响应式属性) 
在该实例范畴之外，则计算属性是不会被更新的。