<template>
  <div>
    <input v-model="msg">
    <p>msg: {{ msg }}</p>
    <p>prop: {{ propMessage }}</p>
    <p>helloMsg: {{ helloMsg }}</p>
    <p>computed msg: {{ computedMsg }}</p>
    <button @click="greet">Greet</button>
    <Hello ref="helloComponent" />
    <World />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'
import Hello from './components/Hello.vue'
import World from './components/World'

@Component({
  props: {
    propMessage: String
  },
  components: {
    Hello,
    World
  }
})
export default class App extends Vue {
  msg: number = 123;
  propMessage!: string;
  helloMsg: string = 'Hello ' + this.propMessage;
  
  get computedMsg (): string {
    return 'computed ' + this.msg;
  }

  greet (): void {
    alert('greeting: ' + this.msg)
    this.$refs.helloComponent.sayHello();
  }

  mounted () {
    this.greet();
  }

  $refs!: {helloComponent: Hello}
}
</script>
