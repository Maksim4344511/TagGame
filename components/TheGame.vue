<template >
	<div v-if="show==='start'">
		<h1 class="name-game">
			Пятнашки
		</h1>
		<div class="game__btn">
			<button v-on:click="restart">Restart</button>
			<button v-on:click="start" :class="btnStyles">Start</button>
			<button v-on:click="easyStart" :class="btnStyles">Easy</button>
			<button v-on:click="test" :class="btnStyles">Test</button>
		</div>
		<div class="game-field">
			<div>
				<div v-for="(cube, index) in cubes" :key="index"
					v-on:click="swap"				
					class="game-field__cube" 				
					:class=" {
						'pos0': index === 0, 'pos1': index === 1, 'pos2': index === 2, 'pos3': index === 3,
						'pos4': index === 4, 'pos5': index === 5, 'pos6': index === 6, 'pos7': index === 7,	
						'pos8': index === 8, 'pos9': index === 9, 'pos10': index === 10, 'pos11': index === 11,
						'pos12': index === 12, 'pos13': index === 13, 'pos14': index === 14, 'pos15': index === 15, 							
				}" 
				>{{ cube }} </div>
			</div>		
		</div>
		<div class="game-click">
				<p>Количество ходов: {{ clicks }}</p> 
		</div>
		<button v-on:click="showResult"	>Рекорды</button>
	</div>

	<div v-else-if="show==='win'" class="formWinner">
		<h1>Победа!!!</h1>
		<form v-show="show" id="v-model-basic">
			<fieldset>
				<legend>Сохранить результат?</legend>
				<label for="userName">Имя </label>
				<input v-model="userName" placeholder="Имя" name="userName" required/>
				<hr>
				<label for="userEmail">e-mail </label>
				<input v-model="userEmail" placeholder="e-mail" name="userEmail" required/>
				<hr>
				<p>Количество ходов: {{ clicks }}</p> 
				<hr>
				<button  v-on:click.prevent="saveResult" type="submit" value="Отправить">Сохранить</button>
				<button  v-on:click.prevent="closeForm"  value="Отправить">Закрыть</button>
			</fieldset>
		</form>
	</div>

	<div v-else-if="show==='result'" class="formResult">
		
		<table cellpadding="15"  >
			<caption>Результаты:</caption>
			<tr v-for="user in users" :key="user.id">
				<td>{{ user.name }}</td>
				<td>{{ user.email }}</td>
				<td>{{ user.clicks }}</td>						
			</tr>
		</table>
		<button  v-on:click.prevent="closeForm"  value="Отправить">Закрыть</button>
	</div>
</template>


<script>
export default {	
	data() {		
		return {
			show: 1,
			cubes: [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "],
			btnStyles:{
				hidden: false,
		},				
		
			clicks: 0,			
			startGame: false,		
			show: 'start',						
			userName: '',
			userEmail:'',
			newUser: {},
			users: [
				{
					id: 1,
					name: 'Имя', 
					email: 'e-mail',
					clicks: 'Ходы',					
				}
			],	
			regEmail: /^[A-Z0-9._%+-]+@[A-Z0-9-]+.+.[A-Z]{2,4}$/i,
			regName: /^[a-z0-9_-]{1,16}$/,
	
		}	
	},	

	methods: {
		start() {			
			this.clicks = 0;
			this.startGame = true;
			this.btnStyles.hidden = true;

				for (let i = 1000; i > 0; i--) {   //Тасование Фишера — Йетса 		  
					this.x = Math.floor(Math.random() * 16);
					this.y = this.cubes.indexOf(" ");
					this.z = this.cubes[this.x];

					if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {						
						this.cubes.splice(this.x, 1, " ");
						this.cubes.splice(this.y, 1, this.z);											
					}	   					
				}			
			},	

		easyStart() {			
			this.clicks = 0;
			this.startGame = true;
			this.btnStyles.hidden = true;
			
			for (let i = 33; i > 0; i--) {				
				this.x = Math.floor(Math.random() * 16);
				this.y = this.cubes.indexOf(" ");
				this.z = this.cubes[this.x];							
				
				if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {	
					this.cubes.splice(this.x, 1, " ");				
					this.cubes.splice(this.y, 1, this.z);										
				}
			}	
		},

		test() {
			this.startGame = true;
			this.btnStyles.hidden = true;
			this.cubes = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "],
			this.cubes.splice(14, 1, " ");
			this.cubes.splice(15, 1, 15);			
			this.clicks = 0;
		},
		
		restart() {
			this.startGame = false;
			this.cubes = [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "];
			this.btnStyles.hidden = false;
			this.clicks = 0;
		},

    swap(event) {		
			if (this.startGame){
				this.x = this.cubes.indexOf(+event.target.innerHTML);
				this.y = this.cubes.indexOf(" ");
				this.z = this.cubes[this.x]
				if (this.x - this.y === 1 || this.x - this.y === -1|| this.x - this.y === 4 || this.x - this.y === -4) {
					this.cubes.splice(this.x, 1, " ");
					this.cubes.splice(this.y, 1, this.z);
					this.clicks += 1;
				}		
				if (this.finish()){
					this.show = 'win';
										
				};
			};	
		},

		finish() {
			return  this.cubes.every((value, index) => value === [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, " "][index]);
		},

		saveResult() {
			if (this.regEmail.test(this.userEmail) === true){				
				if (this.regName.test(this.userName)=== true){
					this.newUser = {
						id: (this.users.at(-1).id + 1),
						name: this.userName, 
						email: this.userEmail,
						clicks: this.clicks,	
					};
					this.users.push(this.newUser);
					this.show = 'start';
					this.userName = '';
					this.userEmail = '';
				} else {
					alert('Введите Ваше имя');
				};
			} else {
				alert('Введите корректный e-mail');
			};
			
		},

		closeForm(){
			this.show = 'start';
		},

		showResult(){
			this.show = 'result';
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
	font-size: 50px;	
	text-align: center;
	width: 80px;
	height: 80px;
	border: 2px solid black;
	border-radius: 30%;
	user-select: none;
  touch-action: none;	
	color: black;		
}

.game-field__cube:hover{
	background: hsla(217, 25%, 82%, 0.849);
  transition: background 1s ease-out;
}

.game__btn{
	width: 370px;
	height: 50px;
	display: flex;
	justify-content: space-between;
	margin: auto;	
	margin-bottom: 20px;
}

button{
	width: 80px;
	height: 30px;
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

.zero{
	border: 0;
}

.hidden{
	display: none;
}

.formWinner{	
	margin: auto;
	font-size: 20px;	
	text-align: center;
	width: 500px;
	height: 300px;		
	user-select: none;
  touch-action: none;	
		
}

.formWinner input{
	margin: 10px;
	width: 200px;;
}
.formWinner button{
	width: 100px;
	margin: 10px;
}

hr{
	opacity: 10%;
}

.formResult{
	
	margin: 30px auto;
	font-size: 20px;	
	text-align: center;	
	border: 4mm ridge rgba(3, 236, 23, 0.6);
	
}
table{
	margin: auto;
}
.formResult td{
	width: auto;
	text-align:start;
	border: 0.5px solid black;
}

.formResult button{
	width: 100px;
	height: 30px;
	margin: 10px;
}


</style>


