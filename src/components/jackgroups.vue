<template>
  <div class="jacks">
      <div v-for="(g,i) in jg" :key="i" class="grp">
          <span><i>{{g.title}}</i></span>
          <div v-for="t in g.jacks" :key="t" class="jackgrpcell">
            <svg class="icon socket"><use href="#socket"></use></svg>
              <svg><use href="#swjack"></use></svg>
              <small v-html="gettxtsvg(t)"></small></div>
      </div>
  </div>
</template>

<script>
export default {
    data: function(){
        return {
            jgroups:[]
        }
    },
    props: ["jg"],
    methods: {
        gettxtsvg: function(t){
            // get text for normal box, if a word begins with # then generate an svg>using #href
            let ary = t.split(" ");
            return ary.map(u=>u[0]=="#"?`<svg><use href="${u}" /></svg>`:u).join(" ");
        }
    },
    mounted: function () {
        this.jgroups = JSON.parse(this.jg);
    }
}
</script>

<style>
.jacks {
    margin-top: 2.5rem;
    margin-left: -17px;
    display: flex;
}
.grp>span::before {
    content: '';
    display: block;
    width: calc(100% - 56px);
    height: 9px;
    border: 2px solid currentColor;
    border-width: 2px 2px 0 2px;
    position: absolute;
    top: 5px;
    left: 28px;
}
.grp>span>i {
    position: relative;
    background-color: #2c333e;
    padding: 4px 8px;
}
.grp>span {
    font-size: small;
    text-align: center;
    position: absolute;
    display: inline-block;
    width: 100%;
    top: -1.25rem;
}
.grp>span>i {
    font-style: normal;
}
.grp {
    display: flex;
    flex-wrap: wrap;
    position: relative;
}
.jackgrpcell>svg {
    width:24px;
    height:24px;
}
.jackgrpcell>small>svg {
    width:24px;
    height:9px;
    fill: none;
    stroke: currentColor;
    stroke-width: 1.75;
}
.jackgrpcell>small {
    display: block;
    font-size: 0.6rem;
    width: 33px;
    padding: 4px;
    border: 2px solid currentColor;
    margin: 0 8px;
}
.jackgrpcell {
    display: flex;
    flex-direction: column;
    width: min-content;
    align-items: center;
    text-align: center;
}


</style>