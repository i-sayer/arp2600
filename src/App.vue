<template>
  <div id="app">
    <svgdefs/>
    <vco tag="VCO-1" :jg='[{"title":"FM CONTROL","jacks":["KYBD CV","S/H OUT","#adsr ADSR","LFO #tri"]}]'/>
    <vco tag="VCO-2" haspw="1" delux="1" :jg='[{"title":"FM CONTROL","jacks":["KYBD CV","S/H OUT","#adsr ADSR","VCO 1 #square"]},{"title":"PWM","jacks":["NOISE GEN"]}]' />
    <vco tag="VCO-3" haspw="1" delux="1" :jg='[{"title":"FM CONTROL","jacks":["KYBD CV","NOISE GEN","#adsr ADSR","VCO 2 #sin"]},{"title":"PWM","jacks":["#adsr ADSR"]}]' />
    <vcf tag="VCF" :jg='[{"title":"AUDIO","jacks":["RING MOD","VCO 1 #square","VCO 2 #pulse","VCO 3 #saw","NOISE GEN"]},{"title":"CONTROL","jacks":["KYBD CV","#adsr ADSR","VCO 2 #sin"]}]' />
    <!-- <env tag="ADSR" :jg='[{"title":"FM CONTROL","jacks":["KYBD CV","S/H OUT","#adsr ADSR","LFO #tri"]}]'/> -->
    <datalist id="ticks">
      <option>0</option>
      <option>25</option>
      <option>50</option>
      <option>75</option>
      <option>100</option>
    </datalist>
    <canvas id="canvas"></canvas>
  </div>
</template>

<script>
import vco from './components/vco'
import vcf from './components/vcf'
// import env from './components/env'
import svgdefs from './components/svgdefs'
export default {
  components:{vco,vcf,/*env,*/svgdefs},
  data: function () {
      return {
          canvas:undefined,
          wire: undefined,
          wires:[],
          point:undefined,
          key_down: false,
          mouse_down: false,
          mouse: undefined,
          sockets:[]
      }
  },
  methods: {
        socketClick: function(e){
            let br = e.target.getBoundingClientRect();
            alert([br.x,br.y].join(" - "));
        },
        setPoint: function(inv_mass){
            if (!this.point) return;
            if (this.mouse) {
                this.point.setCurrent(this.mouse);
                this.point.setPrevious(this.mouse);
            }
            this.point.inv_mass = inv_mass;
        },
        keystate: state=>this.keydown=state,
        position: function(event){
            return this.canvas.adjust({
                x: event.pageX,
                y: event.pageY
            });
        }
  },
  mounted: function () {
        this.sockets = Array.from(document.querySelectorAll("svg.socket"));
        this.sockets.forEach(u=>u.addEventListener("click", this.socketClick));
        this.canvas = new Canvas(document.getElementById('canvas'),window.innerWidth,window.innerHeight);
        ///////////////////////////
        let s1 = this.sockets[0];
        let s2 = this.sockets[13];
        let p = [s1,s2].map(u=>u.getBoundingClientRect());
        p[0].x += (p[0].width/2+2);
        p[0].y += (p[0].height/2+2);
        p[1].x += (p[1].width/2+2);
        p[1].y += (p[1].height/2+2);

		this.wire = new Wire(this.canvas, p);
	
	var ev = {
		'keydown': function(){
			this.key_down = true;
		},
		
		'keyup': function(){
			this.key_down = false;
		},
		
		'mousedown': function(event){
            this.mouse_down = true;
            this.mouse = this.position(event);

            if (!this.mouse) return;

            this.point = this.wire.getClosestPoint(this.mouse);
            if (this.point) this.setPoint(0);
		},
		
		'mouseup': function(){
			this.mouse_down = false;
			if (this.mouse) this.setPoint( this.key_down ? 0 : 1);
		},
		
		'mousemove': function(event){
			if (!this.mouse_down) return;
			
			this.mouse = this.position(event);
			this.setPoint(this.mouse ? 0 : 1);
		}
	};
	for (var e in ev)
		document.addEventListener(e, ev[e], false);
	
	requestAnimationFrame(this.wire.update.bind(this.wire));
  }
}

var FastVector  = function(x,y){
	this.x = x;
	this.y = y;
};

FastVector.prototype = {
	
	add: function (B) {
		var nx, ny;
		if (typeof(B)=='number'){
			nx = this.x+B;
			ny = this.y+B;
		}else{
			nx = this.x+B.x;
			ny = this.y+B.y;
		}
		return new FastVector(nx,ny);
	},
	dot: function(B) {
		return ((this.x*B.x)+(this.y*B.y));
	},
	length: function() {
		return Math.sqrt((this.x*this.x)+(this.y*this.y));
	},
	multiply: function(B) {
		var nx, ny;
		if (typeof(B)=='number'){
			nx = this.x*B; ny = this.y*B;
		}else{ 
			nx = this.x*B.x; ny = this.y*B.y;
		}
		return new FastVector(nx,ny);
	},
	squaredLength: function() {
		return (this.x*this.x)+(this.y*this.y);
	},
	sum: function(){
		return this.x+this.y;
	},
	subtract: function(B) {
		var nx, ny;
		if (typeof(B) == 'number'){
			nx = this.x-B; ny = this.y-B;
		}else{
			nx = this.x-B.x; ny = this.y-B.y;
		}
		return new FastVector(nx,ny);
	},
	toString: function() {
		return "["+this.x+","+this.y+"]";
	}
};

class Canvas {
    constructor(canvas, w, h) {
        canvas.width = w;
        canvas.height = h;
        this.canvas = canvas;
        this.ctx = this.canvas.getContext('2d');
        this.ctx.strokeStyle = 'rgba(255,255,0,0.5)';
        this.ctx.lineWidth = 8;
        this.ctx.lineCap = "round";
        this.ctx.shadowBlur = 4;
        this.ctx.shadowColor = "#00000080";
        this.ctx.shadowOffsetY = 3;
        
        this.width = w;
        this.height = h;
    }
	adjust(pos) {
		return new FastVector(pos.x / this.canvas.width, pos.y / this.canvas.height);
	}
	clear(){
		this.ctx.clearRect(0, 0, this.width, this.height);
	}
}

class Point {
    constructor (canvas, x, y) {
        this.canvas = canvas;
        this.current = this.previous = new FastVector(x, y);

        this.mass = this.inv_mass = 1;
        const mass = 0.05;
        this.force = new FastVector(0.0,mass).multiply(0.3 * 0.3);
        this.radius = 3;
    }
	setCurrent(p) {
		this.current = p;
	}
	setPrevious(p) {
		this.previous = p;
	}
	getCurrent() {
		return this.current;
	}
	getPrevious() {
		return this.previous;
	}
	move() {
		if (this.inv_mass!=0){
			var new_pos = this.current.multiply(1.99).subtract(this.previous.multiply(0.99)).add(this.force);
			new_pos.x = (new_pos.x < 0) ? 0 : ((new_pos.x > 1) ? 1 : new_pos.x);
			new_pos.y = (new_pos.y < 0) ? 0 : ((new_pos.y > 1) ? 1 : new_pos.y);
			this.previous = this.current;
			this.current = new_pos;
		}
	}
}

////////////////////////////////// constraint.js

class Constraint {
    constructor (p1, p2, rl){
        this.p1 = p1;
        this.p2 = p2;
        this.rest_length = rl || p1.getCurrent().subtract(p2.getCurrent()).length();
        this.squared_rest_length = this.rest_length * this.rest_length;
    }
	satisfy() {
		var p1 = this.p1.getCurrent();
		var p2 = this.p2.getCurrent();
		var delta = p2.subtract(p1);
		
		var p1_im = this.p1.inv_mass;
		var p2_im = this.p2.inv_mass;
		
		var d = delta.squaredLength();
		
		var diff = (d - this.squared_rest_length) / ((this.squared_rest_length + d) * (p1_im + p2_im));
		
		if (p1_im != 0){
			this.p1.setCurrent(p1.add(delta.multiply(p1_im * diff)));
		}
		
		if (p2_im != 0){
			this.p2.setCurrent( p2.subtract(delta.multiply(p2_im*diff)) );
		}
	}
}

////////////////////////////////// wire.js

document.addEventListener("DOMContentLoaded", function(){
}, false);

class Wire {
	constructor (canvas, p){
        this.max_points = 4;
        this.width = canvas.width;
        this.height = canvas.height;

        this.canvas = canvas;
        this.points = [];
        this.constraints = [];

        this.num_x_points = 4;
	
        /**
         * start and end are locations of the two sockets
         * middle points same but have small y offset
         */
        let dx = Math.abs(p[0].x - p[1].x)/3;
        let _p = canvas.adjust(new FastVector(p[0].x,p[0].y));
        this.points[0] = new Point(canvas, _p.x, _p.y);
        _p = canvas.adjust(new FastVector(p[0].x-dx,p[0].y+130));
        this.points[1] = new Point(canvas, _p.x, _p.y);
        _p = canvas.adjust(new FastVector(p[1].x+dx,p[1].y+130));
        this.points[2] = new Point(canvas, _p.x, _p.y);
        _p = canvas.adjust(new FastVector(p[1].x,p[1].y));
        this.points[3] = new Point(canvas, _p.x, _p.y);
        //add horizontal constraints
        for (var i = 1; i < 4; i++)
            this.constraints.push(new Constraint(this.points[i - 1], this.points[i], 0.22));
        //pin the top right and top left.
        this.points[0].inv_mass = this.points[3].inv_mass = 0;
    }
	update() {
		this.canvas.clear();
		
		//move each point with a pull from gravity
        this.points.forEach(u=>u.move());
		
		//make sure all the constraints are satisfied.
        this.constraints.forEach(u=>u.satisfy());
		
		var w = this.canvas.width;
		var h = this.canvas.height;

		this.canvas.ctx.beginPath();
		this.canvas.ctx.moveTo(this.points[0].current.x * w, this.points[0].current.y * h);
		// this.canvas.ctx.quadraticCurveTo(this.points[1].current.x * w, this.points[1].current.y * h,
		// 	// this.points[2].current.x * w, this.points[2].current.y * h,
		// 	this.points[3].current.x * w, this.points[3].current.y * h);
		this.canvas.ctx.bezierCurveTo(this.points[1].current.x * w, this.points[1].current.y * h,
			this.points[2].current.x * w, this.points[2].current.y * h,
			this.points[3].current.x * w, this.points[3].current.y * h);
		this.canvas.ctx.stroke();
		requestAnimationFrame(this.update.bind(this));
	}
	getClosestPoint(pos) {
		var min_dist = 1,
			min_point = null,
			num_x = this.num_x_points,
			dist, j;
		
		for (j = 0; j < num_x; j++){
			dist = pos.subtract(this.points[j].getCurrent()).length();
			
			if (dist < min_dist){
				min_dist = dist;
				min_point = this.points[j];
			}
		}
		// return min_point;
		return min_dist<0.1?min_point:null;
	}
}
/*
*/

</script>

<style>
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
.narrow {
    padding: 1ch 0.5ch;
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
.socket:hover {
    box-shadow: 19px 0 0 -5px orange;
}
#canvas {
    position:absolute;
    left:0;
    top:0;
    pointer-events: none;
}
#app {
    user-select: none;
}
</style>
