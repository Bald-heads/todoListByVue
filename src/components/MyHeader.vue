<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入任务名称,回车确认" @keyup.enter="add">
    </div>
</template>

<script>
import {nanoid} from 'nanoid'
import pubSub from "pubsub-js"

export default {

    name: 'MyHeader',
    data() {
        return {
            title: ""
        }
    },
    methods: {
        add(event) {
            //校验数据
            if (!event.target.value.trim()) return alert("输入不能为空")
            //包装用户的输入
            const todoObj = {id: nanoid(), title: event.target.value, done: false}
            //把数据交给app
            pubSub.publish("receive", todoObj)
            event.target.value = ""
        }
    },
}
</script>

<style scoped>
.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
}

.todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>