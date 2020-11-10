<template>
<div class='panel'>
    <small>INITIAL OSCILLATOR FREQUENCY</small>
    <!-- <table>
        <tbody>
            <tr><td><span>10</span></td><td><span>100</span></td><td><span>1KHz</span></td><td><span>10KHz</span></td></tr>
            <tr><td>.03</td><td>.3</td><td>3</td><td>30</td></tr>
        </tbody>
    </table> -->
    <p class='leg'>
        <span>10</span><i></i><span>100</span><i></i><span>1KHz</span><i></i><span>10KHz</span>
        <span>.03</span><i></i><span>.3</span><i></i><span>3</span><i></i><span>30</span>
    </p>
    <input type="range"/>
    <p></p>
    <small style="margin-bottom: 11px">FINE TUNE</small>
    <input type="range"/>
    <p></p>
    <template v-if="haspw">
        <small>PULSE WIDTH</small>
        <p class='lego'>
            <span>10%</span><i></i><span>50%</span><i></i><span>90%</span>
        </p>
        <input type="range"/>
    </template>
    <template v-else>
        <div style="height:54px"><h1>ARP<span style="color:#ee6823">2600</span></h1></div>
    </template>
    <p></p>
    <div v-if="delux" class="sp1 delux">
        <div class="textbox">VOLTAGE CONTROLLED OSCILLATOR {{tag}}</div>
        <svg class="pointer-line a1"><use href="#vcoOut1"></use></svg>
        <svg class="pointer-line a2"><use href="#vcoOut1"></use></svg>
        <small style="grid-row:1;grid-column:3">TRIANGLE</small>
        <svg style="grid-row:2;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
        <svg style="grid-row:3;grid-column:3" class="wave u"><use href="#tri"></use></svg>
        <small style="grid-row:4;grid-column:2 / -1;justify-self:center;margin-left:5ch">OUTPUTS</small>
        <svg style="grid-row:5;grid-column:3" class="wave l"><use href="#sin"></use></svg>
        <svg style="grid-row:6;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
        <small style="grid-row:7;grid-column:3">SINE</small>
        <svg class="pointer-line b1"><use href="#vcoOut2"></use></svg>
        <svg class="pointer-line b2"><use href="#vcoOut2"></use></svg>
        <small style="grid-row:1;grid-column:4">SAWTOOTH</small>
        <svg style="grid-row:2;grid-column:4" class="icon socket"><use href="#socket"></use></svg>
        <svg style="grid-row:3;grid-column:4" class="wave u"><use href="#saw"></use></svg>
        <svg style="grid-row:5;grid-column:4" class="wave l"><use :href="[haspw?'#pulse':'#square']"></use></svg>
        <svg style="grid-row:6;grid-column:4" class="icon socket"><use href="#socket"></use></svg>
        <small style="grid-row:7;grid-column:4">{{haspw?'PULSE':'SQUARE'}}</small>
    </div>
    <div v-else class="sp1">
        <div class="textbox">VOLTAGE CONTROLLED OSCILLATOR {{tag}}</div>
        <svg class="pointer-line a1"><use href="#vcoOut1"></use></svg>
        <svg class="pointer-line a2"><use href="#vcoOut1"></use></svg>
        <small style="grid-row:1;grid-column:3">SAWTOOTH</small>
        <svg style="grid-row:2;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
        <svg style="grid-row:3;grid-column:3" class="wave u"><use href="#saw"></use></svg>
        <small style="grid-row:4;grid-column:3">OUTPUTS</small>
        <svg style="grid-row:5;grid-column:3" class="wave l"><use :href="[haspw?'#pulse':'#square']"></use></svg>
        <svg style="grid-row:6;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
        <small style="grid-row:7;grid-column:3">{{haspw?'PULSE':'SQUARE'}}</small>
    </div>
    <div class="sp2">
        <svg class="pointer-line inp"><use href="#vcoIn1"></use></svg>
        <svg class="pointer-line inp"><use href="#vcoIn2"></use></svg>
        <svg class="pointer-line inp"><use href="#vcoIn3"></use></svg>
        <svg class="pointer-line inp"><use href="#vcoIn4"></use></svg>
        <svg v-if="delux" class="pointer-line inp"><use href="#vcoPwm"></use></svg>
        <div>sw</div>
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input v-if="delux" type="range" class="vertical" value=0 />
    </div>
</div>
</template>

<script>
export default {
    name: 'VCO',
    props: ['tag','haspw','delux']
}
</script>

<style>
.inp {
    position: absolute;
    stroke: currentColor;
    fill: none;
    stroke-width: 2;
}
.sp1.delux+.sp2 {
    grid-template-columns: repeat(5, 62px);
    margin-left: -88px;
}
.sp2 {
    position: relative;
    display: grid;
    margin-left: -30px;
    width: 240px;
    justify-items: center;
    grid-template-columns: repeat(4, 62px);
    margin-top: 5em;
}
.leg>span:nth-child(-n+7) {
    border-bottom: 1px solid;
}
.leg, .lego {
    display: grid;
    text-align: center;
    width: 240px;
    font-size: 0.7rem;
    margin-bottom: 0;
    margin-top: 4px;
    grid-template-columns: min-content 1fr min-content 1fr min-content 1fr min-content;
}
.lego {
    grid-template-columns: min-content 1fr min-content 1fr min-content;
}
.wave {
    stroke: currentColor;
    stroke-width: 2;
    fill: none;
    width: 32px;
    height: 10px;
    justify-self: end;
}
.wave.u {
    align-self: start;
}
.wave.l {
    align-self: end;
}
.sp1 {
    display: grid;
    grid-template-columns: 124px 12px 60px;
    grid-template-rows: 1em 42px 1.5em 1em 1.5em 42px 1em;
}
.sp1.delux {
    grid-template-columns: 124px 12px 60px 60px;
}
.sp1>.textbox {
    grid-row: 1 / -1;
    align-self: center;
}
.sp1>.a1 {
    grid-row: 2;
}
.sp1>.a2 {
    grid-row: 6;
    transform: scaleY(-1) translateY(-24px);
}
.sp1>.b1 {
    grid-row: 3;
}
.sp1>.b2 {
    grid-row: 5;
    transform: scaleY(-1) translateY(-7px);
}
.sp1>small{
    font-size: 0.6em;
    align-self: center;
    justify-self: end;
}
.sp1>.icon{
    align-self: center;
    justify-self: end;
}
</style>