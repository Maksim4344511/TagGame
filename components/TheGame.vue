<template>
	<h1 class="name-game">
		Пятнашки
	</h1>
	<div class="game__btn">
		<button v-on:click="restart">Restart</button>
		<button v-on:click="start" :class="styles">Start</button>
		<button v-on:click="easyStart" :class="styles">Easy</button>
		<button v-on:click="test" :class="styles">Test</button>
	</div>
	<div class="game-field" >
		<div>
			<div v-for="(cube, index) in cubes" :key="index"
				v-on:click="swap"
			  class="game-field__cube" 
			  :class="{ 'pos0': index === 0, 'pos1': index === 1, 'pos2': index === 2, 'pos3': index === 3,
									'pos4': index === 4, 'pos5': index === 5, 'pos6': index === 6, 'pos7': index === 7,	
				 					'pos8': index === 8, 'pos9': index === 9, 'pos10': index === 10, 'pos11': index === 11,
									'pos12': index === 12, 'pos13': index === 13, 'pos14': index === 14, 'pos15': index === 15, 							
			}" 
			>{{ cube }} </div>
		</div>		
	</div>
	<div class="game-click">
			Clicks: {{ clicks }}		
	</div>
	<div class="game-records">
			Record: {{ record }}		
	</div>

</template>

<script>
export default {
	data() {
		return {
			cubes: [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "],
			styles:{
				hidden: false,
			},
			clicks: 0,
			record: 500,
		}	
	},

	computed: {
		addClassCube() {							
			return 	this.cubes.index;			
		},		
	},

	methods: {

		start()  {			
			this.clicks = 0;
			for (let i = 1000; i > 0; i--) {   //Тасование Фишера — Йетса 		  
				this.x = Math.floor(Math.random() * 16);
				this.y = this.cubes.indexOf(" ");
				this.z = this.cubes[this.x];

				if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {
					this.cubes.splice(this.x, 1, " ");
					this.cubes.splice(this.y, 1, this.z);
					this.styles.hidden = true;					
				}	   					
 	 		}			
		},

		easyStart()  {			
			this.clicks = 0;
			for (let i = 33; i > 0; i--) {   //Тасование Фишера — Йетса 		  
				this.x = Math.floor(Math.random() * 16);
				this.y = this.cubes.indexOf(" ");
				this.z = this.cubes[this.x];

				if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {
					this.cubes.splice(this.x, 1, " ");
					this.cubes.splice(this.y, 1, this.z);
					this.styles.hidden = true;					
				}	   					
 	 		}			
		},

		test() {
			this.cubes = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "],
			this.cubes.splice(14, 1, " ");
			this.cubes.splice(15, 1, 15);
			this.styles.hidden = true;
			this.clicks = 0;
		},
		
		restart() {
			this.cubes = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "];
			this.styles.hidden = false;
			this.clicks = 0;
		},

    swap(event) {			
			this.x = this.cubes.indexOf(+event.target.innerHTML);
			this.y = this.cubes.indexOf(" ");
			this.z = this.cubes[this.x]
			if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {
				this.cubes.splice(this.x, 1, " ");
				this.cubes.splice(this.y, 1, this.z);
				this.clicks += 1;
			}		
			if (this.finish()){
				alert("Победа");
				if (this.clicks < this.record){
					this.record = this.clicks;
					alert(`'Новый рекорд: ' ${this.record}`);
				}
			};
		},

		finish(){
			return  this.cubes.every((value, index) => value === [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "][index]);
		}
	}
}	
</script>



<style>

.name-game{
	text-align: center;
}

.game-field {
	position: relative;	
	margin: auto;	
	width: 370px;
	height: 370px;
	border: 5px solid darkblue;
	user-select: none;
  touch-action: none;    
	
}

.game-field__cube {
	position: absolute;
	font-size: 40px;
	font-weight: bold;
	text-align: center;
	width: 80px;
	height: 80px;
	border: 2px solid black;
	user-select: none;
  touch-action: none;	
	color: black;
}

.game__btn{
	width: 370px;
	height: 50px;
	display: flex;
	justify-content: space-between;
	margin: auto;	
	margin-bottom: 20px;
}

.game__btn button{
	width: 80px;
}

.game-click{
	margin-top: 30px;
	font-size: 40px;
}

.game-records{
	margin-top: 30px;
	font-size: 40px;
}

.pos0{
	top: 10px;
	left: 10px;
}
.pos1{
	top: 10px;
	left: 100px;
}
.pos2{
	top: 10px;
	left: 190px;
}

.pos3{
	top: 10px;
	left: 280px;
}

.pos4{
	top: 100px;
	left: 10px;
}

.pos5{
	top: 100px;
	left: 100px;
}

.pos6{
	top: 100px;
	left: 190px;
}

.pos7{
	top: 100px;
	left: 280px;
}

.pos8{
	top: 190px;
	left: 10px;
}

.pos9{
	top: 190px;
	left: 100px;
}

.pos10{
	top: 190px;
	left: 190px;
}

.pos11{
	top: 190px;
	left: 280px;
}

.pos12{
	top: 280px;
	left: 10px;
}

.pos13{
	top: 280px;
	left: 100px;
}

.pos14{
	top: 280px;
	left: 190px;
}

.pos15{
	top: 280px;
	left: 280px;
}



.hidden{
	display: none;
}

</style>