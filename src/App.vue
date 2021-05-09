<template>
  <div id="app">
    <svgdefs/>
    <vco tag="VCO-1" :jg='[{title:"FM CONTROL",jacks:["KYBD CV","S/H OUT","ADSR","LFO /\\"]}]'/>
    <vco tag="VCO-2" haspw="1" delux="1" :jg='[{title:"FM CONTROL",jacks:["KYBD CV","S/H OUT","ADSR","VCO 1 SQR"]},{title:"PWM",jacks:["NOISE GEN"]}]' />
    <vco tag="VCO-3" haspw="1" delux="1" :jg='[{title:"FM CONTROL",jacks:["KYBD CV","NOISE GEN","ADSR","VCO 2 SIN"]},{title:"PWM",jacks:["ADSR"]}]' />
    <vcf tag="VCF" :jg='[{title:"AUDIO",jacks:["RING MOD","VCO 1 SQR","VCO 2 SQR","VCO 3 SAW","NOISE GEN"]},{title:"CONTROL",jacks:["KYBD CV","ADSR","VCO 2 SIN"]}]' />
    <env tag="ADSR" :jg='[{title:"FM CONTROL",jacks:["KYBD CV","S/H OUT","ADSR","LFO /\\"]}]'/>
    <datalist id="ticks">
      <option>0</option>
      <option>25</option>
      <option>50</option>
      <option>75</option>
      <option>100</option>
    </datalist>
  </div>
</template>

<script>
import vco from './components/vco'
import vcf from './components/vcf'
import env from './components/env'
import svgdefs from './components/svgdefs'
export default {
  components:{vco,vcf,env,svgdefs}
}
</script>

<style>
/* @import url(//db.onlinewebfonts.com/c/6c6db39a56fe8e8f19ba47c6a0770a18?family=Helvetica+Neue); */
@font-face {
  font-family: 'Helvetica Neue';
  src: url('~@/assets/helvetica.woff2');
  font-weight: normal;
  font-style: normal;
}
:root{
  font-family: 'Helvetica Neue', serif;
  background-color: #2c333e;
  /* background: linear-gradient(black 2px, #2c333e 2px); */
  background-size: 10px 10px;
  color: white;
}
.pointer-line {
  width: 200px;
  height: 1em;
  overflow: visible;
}
.panel {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  padding: 1ch 3.5ch;
  border-right: 2px solid;
}
.wide {
  width: 480px;
}
label {
  font-size: small;
}
input {
  width: 240px;
}
.vertical {
    height: 220px;
    width: 8px;
    -webkit-appearance: slider-vertical;
    position: relative;
}
.vertical::after {
  content:"";
  position: absolute;
  display: block;
  top:0;
  right: -24px;
  height: 100%;
  width: 16px;
  background: linear-gradient(white 1px, transparent 1px);
  background-size: 100% calc(calc(100% - 2px) / 4);
  background-repeat: repeat-y;
}
.textbox {
  border: 3px solid;
  padding: .25ch .5ch;
  text-align: center;
  width: min-content;
  line-height: 1.1;
}
</style>
