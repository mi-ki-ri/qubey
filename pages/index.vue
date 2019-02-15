<template>
  <div class="wrap">
    <section class="container">
      <start-switch @startStop="startStop" />
      <section class="lanes">
        <lane inst="kick" ref="kick" />
        <lane inst="snare" ref="snare" />        
        <lane inst="hat" ref="hat" />
      </section>
    </section>
  </div>
</template>

<script>
import Lane from '~/components/Lane.vue'
import StartSwitch from "~/components/StartSwitch.vue"

export default {
  components: {
    StartSwitch, // スイッチ
    Lane // キュービーが落ちるレーン
  },
  data(){return{
    isStart: false
  }},
  methods:{
    startStop:function(){
      // start toggle
      this.isStart = ! this.isStart

      // ref="" で指定した名前でイベントを呼べる
      this.$refs.kick.QBStart();
      this.$refs.snare.QBStart();
      this.$refs.hat.QBStart();
    }
  }
}
</script>

<style>
.wrap{
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(rgb(200,225,255), rgb(225,255,255));
  min-height: 100vh;
}
.container{
  display: flex;
  justify-content: center;
  align-items: stretch;
  flex-direction: column;
  min-height: 100vh;
}
.lanes {
  margin: 32px auto 0;

  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  text-align: center;
}

</style>
