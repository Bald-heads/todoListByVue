<template>
    <div>
        <li>
            <label>
                <input type="checkbox" :checked="todoObj.done" @click="handCheck(todoObj.id)"/>
                <span v-show="!todoObj.isEdit">{{ todoObj.title }}</span>
                <input :value="todoObj.title"
                       v-show="todoObj.isEdit"
                       type="text"
                       @blur="handleBlur(todoObj,$event)"
                       ref="inputTitle"
                />
            </label>
            <button class="btn btn-danger" @click="handDelete(todoObj.id)">删除</button>
            <button class="btn btn-demo" @click="handleEdit(todoObj,$event)">编辑</button>
        </li>
    </div>
</template>

<script>
export default {

    name: 'MyItem',
    props: ['todoObj'],
    methods: {
        handCheck(nid) {
            this.$bus.$emit("checkTodo", nid)
        },
        handDelete(nid) {
            if (confirm('确定删除吗?')) {
                this.$bus.$emit("deleteTodo", nid)
            }
        },
        handleEdit(todoObj) {
            if (Object.keys(todoObj).indexOf("isEdit") !== -1) {
                todoObj.isEdit = true
            } else {
                this.$set(todoObj, 'isEdit', true)
            }
            this.$nextTick(() => {
                this.$refs.inputTitle.focus()
            })
        },
        handleBlur(todoObj, event) {
            todoObj.isEdit = false
            this.$bus.$emit("editTodo", todoObj.id, event.target.value)
        }
    }
}
</script>

<style scoped>

li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
}

li label {
    float: left;
    cursor: pointer;
}

li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
}

li button {
    float: right;
    display: none;
    height: 30px;
    width: 50px;
    margin-top: 3px;
}

li:before {
    content: initial;
}

li:last-child {
    border-bottom: none;
}

li:hover {
    background-color: darkgray;
}

li:hover button {
    display: block;
}
</style>
