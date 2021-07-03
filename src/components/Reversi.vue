<template>
	<div class="content">
		<button class="close-btn" @click="receiveReversiEvent">Close</button>
		<table>
			<tr v-for="(row,idx) in grids" :key="idx">
				<td v-for="(r, idx) in row" :key="idx">
					<div class="circle" :style="'background:' + r.color">{{r.name === "blank" ? "" : r.name}}</div>
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
		}
	},
	props:{
		receiveReversiEvent: Function,
		selectLanguage: String
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
		alert(this.selectLanguage);
		const search_locatable = ()=>{
			const n = this.grids.length;
			let tmp = 0;
			for(let i=0;i<n;i++){
				for(let j=0;j<n;j++){
					tmp += i*j;
				}
			}
			console.log(tmp);
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
</style>
