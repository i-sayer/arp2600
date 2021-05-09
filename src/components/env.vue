<template>
<div class='panel wide'>
    <p class='lege'>
        <span>ATTACK TIME</span><span>DECAY TIME</span><span>SUS LEVEL</span><span>REL TIME</span>
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
    </p>
    <p></p>
    <div class="sp1e">
        <div class="textbox">ADSR<br/>ENVELOPE GENERATOR</div>
        <svg style="grid-row:4;grid-column:2" class="pointer-line"><use href="#vcfOut"></use></svg>
        <small style="grid-row:5;grid-column:3;align-self:end">OUTPUT</small>
        <svg style="grid-row:4;grid-column:3" class="icon socket"><use href="#socket"></use></svg>
    </div>
    <div class="sp2e">
        <svg class="pointer-line inp" style="transform: translateX(248px)"><use href="#vcoIn3"></use></svg>
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
        <input type="range" class="vertical" value=0 />
    </div>
    <jackgroups :jg='jg' />
</div>
</template>

<script>
import jackgroups from './jackgroups.vue'
export default {
    components: { jackgroups },
    name: 'ENV',
    props: ['tag','jg']
}
</script>

<style>
.inp {
    position: absolute;
    stroke: currentColor;
    fill: none;
    stroke-width: 2;
}
.sp2e {
    position: relative;
    display: grid;
    margin-left: -270px;
    width: 240px;
    justify-items: center;
    grid-template-columns: repeat(8, 62px);
    margin-top: 5em;
}
.lege>span {
    transform: translateX(-16px);
}

.lege, .legop {
    display: grid;
    text-align: center;
    width: 180px;
    font-size: 0.7rem;
    margin-bottom: 0;
    margin-top: 4px;
    grid-template-columns: repeat(4, 25%);
}
.lege {
    margin-bottom: 11px;
}
.legop {
    grid-template-columns: min-content 1fr min-content;
}
.sp1e {
    display: grid;
    grid-template-columns: 1fr 12px 40px;
    grid-template-rows: 1em 42px 1.5em 1em 1.5em 42px 1em;
}
.sp1e>.textbox {
    grid-row: 1 / -1;
    align-self: center;
    justify-self: end;
}
.sp1e>small{
    font-size: 0.6em;
    align-self: center;
    justify-self: end;
}
.sp1e>.icon{
    align-self: center;
    justify-self: end;
}
</style>