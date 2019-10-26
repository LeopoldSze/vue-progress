<template>
  <div id="app">
    <h1>进度条外显</h1>
    <Progress :percentage="10"></Progress>
    <Progress :percentage="50" :color="'#ff0'"></Progress>
    <Progress :percentage="70" status="exception"></Progress>
    <Progress :percentage="100" status="success"></Progress>
    <br>
    <br>
    <h1>进度条内显</h1>
    <Progress :percentage="21" :text-inside="true" :strok-width="25"></Progress>
    <Progress :percentage="50" :text-inside="true" :strok-width="25"></Progress>
    <Progress :percentage="70" status="exception" :text-inside="true" :strok-width="25"></Progress>
    <Progress :percentage="87" status="success" :text-inside="true" :strok-width="25"></Progress>
    <br>
    <br>
    <h1>环形进度条</h1>
    <Progress :percentage="30" type="circle"></Progress>
    <Progress :percentage="50" type="circle"></Progress>
    <Progress :percentage="80" type="circle" status="exception"></Progress>
    <Progress :percentage="80" type="circle" status="success"></Progress>
  </div>
</template>

<script>
import Progress from './components/Progress'
export default {
  name: 'app',
  components: {
    Progress
  },
  data() {
    return {
      strokWidth: 6
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
  h1{
    font-size: 25px;
    text-align: left;
    padding-left: 1%;
  }
</style>
