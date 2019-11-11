<template>
  <div id="app">
    <Search></Search>
    <hr />
    <CountDown></CountDown>
  </div>
</template>

<script>
import Search from './components/search'
import CountDown from './components/countDown'
export default {
  name: 'app',
  components: {
    Search,
    CountDown
  }
}
</script>
