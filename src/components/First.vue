<template>
  <div>
    <h1>{{ msg }}</h1>
  </div>
</template>
<script>
export default {
    // name 表示设置别名，可以不设置，建议和组件的名称一致
    name:"First",
    data(){
        return{
            msg:"First Vue"
        }
    }
}
</script>

