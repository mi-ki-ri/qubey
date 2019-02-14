<template>
    <div class="myQubeyWrap">
        <button class="STBtn" @click="QBStart">ST</button>
        <div class="myQubeyLane" @click="setObj">
            <p class="myQubey" v-bind:style="{top: this.i * 32 + 'px'}">&nbsp;</p>
            <div class="myObj"
                v-bind:style="{top: obj.grid * 32 + 'px'}"
                v-bind:key="key" v-for="(obj,key) in objs"
                @click="delObj(key, $event)"
            >
                &nbsp;
            </div>
        </div>
    </div>
</template>

<script>

import Tone from 'tone'

const Obj = function( grid ){
    this.grid = grid
}

export default {
    data() {
        return {
            i: 0,
            isMoving: false,
            myLoop: new Tone.Loop((time)=>{
                this.i +=  1;
                if( this.i >= 16 ){
                    this.i = 0;
                }
                console.log(this.i)

                for( let obj of this.objs ){
                    if( obj.grid == this.i ){
                        console.log("hit")
                        this.synth.triggerAttackRelease( "16n")
                    }
                }


            }, "16n"),
            objs: [],
            synth: new Tone.NoiseSynth({
                type: "pink"
            }).toMaster()
        }
    },
    methods:{
        QBStart: function(){
            this.isMoving = ! this.isMoving
            if( this.isMoving == true ){
                this.myLoop.start(0)
                Tone.Transport.start();
            }else{
                this.myLoop.stop(0);
                Tone.Transport.stop()
            }
        },
        setObj: function(e){
            //console.log( e.offsetY )
            let gridNum = Math.floor(e.offsetY / 32)
            console.log(gridNum)

            this.objs.push( new Obj( gridNum ) )
            

        },
        delObj: function(key, e){
            this.objs.splice(key, 1)
            e.preventDefault();
            e.stopPropagation();
        }
    }
}
</script>

<style>
.myQubeyLane{
    box-sizing: border-box;
    height: calc(32px * 16 + 16px + 6px);
    width: calc(32px + 16px + 6px);
    padding: 8px;
    border: dashed #ccc 3px;
    position: relative;
}
.myQubey{
    height: 32px;
    width: 32px;
    background-image: url("~assets/Qubey.png");
    background-position: center center;
    position: absolute;
    top: 0;
}
.STBtn{
    width: 32px;
    height: 32px;
    background-color: #fff;
    border-radius: 3px;
    margin-bottom: 3px;
}
.myObj{
    height: 32px;
    width: 32px;
    background-color: rgb(225,225,0);
    background-position: center center;
    position: absolute;
    top: 0;
}
</style>
