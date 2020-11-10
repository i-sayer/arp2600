<template>
<div class='panel wide'>
    <small>INITIAL FILTER FREQUENCY</small>
    <p class='legf'>
        <span>10</span><i></i><span>100</span><i></i><span>1KHz</span><i></i><span>10KHz</span>
    </p>
    <input type="range"/>
    <p></p>
    <small style="margin-bottom: 11px">FINE TUNE</small>
    <input type="range"/>
    <p></p>
    <small>RESONANCE</small>
    <p class='legop'>
        <span>MIN</span><i></i><span>MAX</span>
    </p>
    <input type="range"/>
    <p></p>
    <div class="sp1f">
        <div class="textbox">VOLTAGE CONTROLLED FILTER/RESONATOR {{tag}}</div>
        <svg style="grid-row:4;grid-column:2" class="pointer-line"><use href="#vcfOut"></use></svg>
        <small style="grid-row:5;grid-column:3;align-self:end">OUTPUT</small>
        <svg style="grid-row:4;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
    </div>
    <div class="sp2f">
        <svg class="pointer-line inp" style="transform: translateX(248px)"><use href="#vcoIn3"></use></svg>
        <svg class="pointer-line inp" style="transform: translateX(248px)"><use href="#vcoIn4"></use></svg>
        <svg class="pointer-line inp"><use href="#vcfIn1"></use></svg>
        <svg class="pointer-line inp"><use href="#vcfIn2"></use></svg>
        <svg class="pointer-line inp"><use href="#vcfIn3"></use></svg>
        <svg class="pointer-line inp"><use href="#vcfIn4"></use></svg>
        <svg class="pointer-line inp"><use href="#vcfIn5"></use></svg>
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <div>sw</div>
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
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
.sp2f {
    position: relative;
    display: grid;
    margin-left: -270px;
    width: 240px;
    justify-items: center;
    grid-template-columns: repeat(8, 62px);
    margin-top: 5em;
}
.legf, .legop {
    display: grid;
    text-align: center;
    width: 240px;
    font-size: 0.7rem;
    margin-bottom: 0;
    margin-top: 4px;
    grid-template-columns: min-content 1fr min-content 1fr min-content 1fr min-content;
}
.legf {
    margin-bottom: 11px;
}
.legop {
    grid-template-columns: min-content 1fr min-content;
}
.sp1f {
    display: grid;
    grid-template-columns: 1fr 12px 40px;
    grid-template-rows: 1em 42px 1.5em 1em 1.5em 42px 1em;
}
.sp1f>.textbox {
    grid-row: 1 / -1;
    align-self: center;
    justify-self: end;
}
.sp1f>small{
    font-size: 0.6em;
    align-self: center;
    justify-self: end;
}
.sp1f>.icon{
    align-self: center;
    justify-self: end;
}
</style>