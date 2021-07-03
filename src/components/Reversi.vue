<template>
	<div class="content">
		<button class="close-btn" @click="receiveReversiEvent">Close</button>
		<table>
			<tr v-for="(row,idxR) in grids" :key="idxR">
				<td v-for="(r, idxC) in row" :key="idxC">
					<div :class="{'circle': true, 'candidate': r.candidate}" :row="idxR" :col="idxC" @click="gridClicked" :style="'background:' + r.color">{{r.name === "blank" ? "" : r.name}}</div>
				</td>
			</tr>
		</table>
	</div>
</template>
<script>
export default {
	name: "Stone",
	data: function(){
		return {
			grids:[],
			size:0,
		}
	},
	props:{
		receiveReversiEvent: Function,
		selectLanguage: String,
		selectLanguageColor: String,
	},
	methods:{
		gridClicked(v){
			const n = this.grids.length;
			let row = parseInt(v.currentTarget.getAttribute("row"));
			let col = parseInt(v.currentTarget.getAttribute("col"));
			const dfs = (i, j, di, dj, dist)=>{
				i += di;
				j += dj;
				dist++;
				if(i >= n || j >=n || i < 0 || j < 0)return false;
				if(this.grids[i][j].name === "blank")return false;
				if(this.grids[i][j].name === this.selectLanguage){
					return true;
				}else{
					if(dfs(i,j,di,dj,dist)){
						this.grids[i][j].color = this.selectLanguageColor;
						this.grids[i][j].name = this.selectLanguage;
					}
				}
			}
			if(v.currentTarget.classList.contains("candidate")){
				let candidate_tmp = document.getElementsByClassName("candidate");
				while(candidate_tmp.length){
					candidate_tmp[0].classList.remove("candidate");
				}
				this.grids[row][col].name = this.selectLanguage;
				this.grids[row][col].color = this.selectLanguageColor;
				dfs(row,col,-1,-1, 0);
				dfs(row,col,-1,0, 0);
				dfs(row,col,-1,1, 0);
				dfs(row,col,0,-1, 0);
				dfs(row,col,0,1, 0);
				dfs(row,col,1,-1, 0);
				dfs(row,col,1,0, 0);
				dfs(row,col,1,1, 0);
			}
			this.axios
			.post("http://localhost:3000/api/set-reversi", this.grids)
			.then(() => {
				alert("石をおきました");
			})
			.catch((e) => {
				console.log(e);
			});

		}
	},
	created() {
		this.axios
		.get("http://localhost:3000/api/get-reversi")
		.then((res) => {
			this.grids = res.data;
			search_locatable();
		})
		.catch((e) => {
			console.log(e);
		});

		let n = 0;
		let noCandidate = true;
		const dfs = (i, j, di, dj, dist)=>{
			i += di;
			j += dj;
			dist++;
			if(i >= n || j >=n || i < 0 || j < 0)return;
			if(this.grids[i][j].name == 'blank'){
				if(dist >= 2){
					this.grids[i][j].candidate = true;
					noCandidate = false;
				}
			}else{
				dfs(i,j,di,dj,dist);
			}
		}
		const search_locatable = ()=>{
			n = this.grids.length;
			for(let i=0;i<n;i++){
				for(let j=0;j<n;j++){
					if(this.grids[i][j].name === this.selectLanguage){
						dfs(i,j,-1,-1, 0);
						dfs(i,j,-1,0, 0);
						dfs(i,j,-1,1, 0);
						dfs(i,j,0,-1, 0);
						dfs(i,j,0,1, 0);
						dfs(i,j,1,-1, 0);
						dfs(i,j,1,0, 0);
						dfs(i,j,1,1, 0);
					}
				}
			}
			const dirs = [[0,1],[1,0],[-1,0],[0,-1]];
			if(noCandidate){
				for(let i=0;i<n;i++){
					for(let j=0;j<n;j++){
						if(this.grids[i][j].name !== "blank")continue;
						for(let d of dirs){
							let a = d[0] + i;
							let b = d[1] + j;
							if(a >= n || b >=n || a < 0 || b < 0)continue;
							if(this.grids[a][b].name !== "blank"){
								this.grids[i][j].candidate = true;
							}
						}
					}
				}

			}
			console.log(this.grids);
		}
	},
}
</script>

<style scoped>
.content{
	width: 90%;
	height: 360px;
	position:absolute;
	position:relative;
	background-color:#eee;
	margin-left:5%;
	margin-right:5%;
}
.close-btn{
	position:absolute;
	right:0;
}
.circle{
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-size:10px;
  overflow:hidden;
  text-align: center;
  line-height: 40px;
  color:#fff;
}
table{
	margin:0 auto;
}
.candidate {
  animation: flash 1s linear infinite;
}
@keyframes flash {
  0%,
  100% {
	background-color: #0091ea;
  }

  50% {
	background-color: #0090ea41;
  }
}
</style>
