<template>
    <div class="myQubeyWrap">
        <div class="myQubeyLane">
            <div class="myGuide"
                v-bind:key="`${finalInst}${key}`" v-for="(num,key) in my16s"
                v-bind:style="{top: key * 32 + 'px'}"
                @click="setObj(key, $event)">
                ・
            </div>
            <div class="myObj"
                v-bind:style="{top: obj.grid * 32 + 'px'}"
                v-bind:key="key" v-for="(obj,key) in objs"
                @click="delObj(key, $event)"
            >
                ♪
            </div>
            <p class="myQubey" @click="setObj(i, $event)"
                v-bind:style="{top: this.i * 32 + 'px',
                transform: 'rotate('+ (this.i + 1) * 45 * this.snareOrElse +'deg)'}">&nbsp;</p>
            
        </div>
    </div>
</template>

<script>

import Tone from 'tone'

const Obj = function( grid ){
    this.grid = grid
}

export default {
    props: ["inst", "isStart"],
    data() {
        return {
            i: -1,
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
                        switch (this.finalInst){
                            case "hat":
                                this.hat.triggerAttackRelease( "16n");
                                break;
                            case "snare":
                                this.snare.triggerAttackRelease("16n");
                                this.snareRim.triggerAttackRelease("C3", "16n")

                                break;
                            case "kick":
                            default:
                                this.kick.triggerAttackRelease("C1", "16n")
                                break;
                        }
                        
                    }
                }


            }, "16n"),
            objs: [],
            hat: new Tone.NoiseSynth({
                type: "pink",
                volume: -24
            }).toMaster(),
            snare: new Tone.NoiseSynth({
                envelope  : {
                    attack  : 0.025 ,
                    decay  : 0.12 ,
                    sustain  : 0
                },
                volume: -18
            }).toMaster(),
            snareRim: new Tone.Synth({
                oscillator  : {
                    type  : "sine"
                }  ,
                envelope  : {
                    attack  : 0.025 ,
                    decay  : 0.25 ,
                    sustain  : 0
                },
                volume: -36
            }).toMaster(),
            kick: new Tone.Synth({
                oscillator  : {
                    type  : "sine"
                }  ,
            }).toMaster(),
            my16s: [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15],
            finalInst: this.inst,
            finalIsStart: this.isStart
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
        setObj: function(key,e){
            //console.log( e.offsetY )
            //let gridNum = Math.floor(e.offsetY / 32)
            //console.log(gridNum)

            this.objs.push( new Obj( key ) )
            

        },
        delObj: function(key, e){
            this.objs.splice(key, 1)
            e.preventDefault();
            e.stopPropagation();
        }
    },
    computed:{
        snareOrElse: function(){
            if( this.finalInst == "snare" ){
                return 1
            }
            return -1
        }
    }
}
</script>

<style>
.myQubeyWrap{
    width: auto;
    height: auto;
}
.myQubeyLane{
    box-sizing: border-box;
    height: calc(32px * 16 + 16px + 6px);
    width: calc(32px + 16px + 6px);
    padding: 8px;
    margin: 1px;
    border: dashed #ccc 3px;
    position: relative;
}
.myQubey{
    height: 32px;
    width: 32px;
    background-image: url("~assets/Qubey.png");
    background-position: center center;
    position: absolute;
    top: -32px;
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
    color: rgb(185,225,0);
    background-color: rgba(185,225,0,0.25);
    line-height: 32px;
    font-size: 32px;
    background-position: center center;
    position: absolute;
    top: 0;
}
.myGuide{
    color: #ccc;
    height: 32px;
    width: 32px;
    font-size: 16px;
    line-height: 32px;
    position: absolute;
    top: 0;
}

.myGuide:nth-of-type( 4n +1 ){
    color: #aaa;
}
</style>
