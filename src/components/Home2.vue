<script setup>
	import {
		ref,
		reactive
	} from 'vue'
	// import { reactive } from 'vue';


	defineProps({
		msg: String
	})

	const count = ref(-1)

	const isActive = ref(-1)


	const iname = ref(null)

	const icount = ref(null)


	const kou = ref(false)
	const addShow = ref(false)
	
	

	// const isActive = computed(() => count.value > 0);

	const myArray = reactive([]);


	const total = ref(null)

	const ms = ref("添加后记得打乱哦")
	
	
	function addItemShow() {
		addShow.value = true;
	}
	

	function addItem() {

		if (iname == null || iname == '') {
			return;
		}
		if (icount == null) {
			return;
		}

		stopCount();

		for (let i = 0; i < icount.value; i++) {
			let item = {
				id: myArray.length + 1,
				name: iname.value
			}
			let dd = JSON.stringify(item)
			console.log(JSON.parse(dd))
			myArray.push(JSON.parse(dd));
		}

		count.value = -1

		itemsWithCount()
		iname.value = null
		icount.value = null
		ms.value = "添加后记得打乱哦"
		addShow.value = false
	}

	function chou() {
		stopCount();
		count.value = -1
		isActive.value = Math.floor(Math.random() * (myArray.length * 3));
		startCount();


	}

	function luan() {
		// 方法1
		const shuffle = () => {
			return myArray.sort(() => {
				return Math.random() > 0.5 ? 1 : -1
			})
		}

		myArray.value = shuffle()

		count.value = -1
		
		isActive.value = -1
		
		stopCount();
		ms.value = "可以开始抽取啦"


	}

	function del(i) {


		myArray.splice(i, 1);
		itemsWithCount()
		count.value = -1
		
		isActive.value = -1
		ms.value = "可以开始抽取啦"


	}




	const intervalId = ref(null);

	const startCount = () => {
		if (intervalId.value) return;
		intervalId.value = setInterval(() => {
			count.value += 1;
			if (count.value >= isActive.value) {
				console.log()
				ms.value = "恭喜" + (isActive.value % myArray.length + 1) + "号 " + myArray[isActive.value %
					myArray.length].name
				// kou.value = true
				stopCount();
			}
		}, 100);
	};

	const stopCount = () => {
		if (intervalId.value) {
			clearInterval(intervalId.value);
			intervalId.value = null;
		}
	};

	const itemsWithCount = () => {
		const counts = {};
		myArray.forEach(item => {
			counts[item.name] = (counts[item.name] || 0) + 1;
		});
		// 将对象转换为数组，便于在模板中遍历

		total.value = Object.keys(counts).map(name => ({
			name,
			count: counts[name]
		}))

		console.log()
	}
</script>

<template>

	<div class="container">
		<div class="content">
			<!-- 内容区域 -->
			<div class="big-con">
				<div class="con-title">
					抽抽抽
				</div>
				<div class="con-title" style="margin: 0;padding: 0;">

					<button @click="addItemShow()" class="chou-btn">添加</button>
					<button @click="luan()" class="chou-btn">打乱</button>
					<button @click="chou()" class="chou-btn">抽取</button>
				</div>
				<div class="input-con" v-if="addShow">
					<input v-model="iname"  class="input-style"  placeholder="请输入名称" />
					<input v-model="icount" type="number" class="input-style" placeholder="请输入次数" />
					<div @click="addItem()" class="input-style" style="display: flex;align-items: center;justify-content: center;background-color: #2ed573;cursor: pointer;">确定</div>
				</div>

				<div class="left-con">
					<div class="left-item" v-for="(item,i) in total">
						<div class="left-name">
							{{item.name}}
						</div>{{item.count}}次
					</div>
				</div>

				<div class="center-con">
					<div class="center-title"> 抽奖池</div>
					<div class="center-chou"> {{ms}} <button v-if="kou" class="center-chou-btn">扣除次数</button>
					</div>
					<div v-for="(item,i) in myArray" @click="del(i)">
						<div class="center-item-a" v-show="i==count%myArray.length ">
							<div class="center-name">
								{{item.name}}
							</div>
							<div class="center-name">
								{{i+1}}
							</div>
						</div>

						<div class="center-item" v-show="i!=count%myArray.length">
							<div class="center-name">
								{{item.name}}
							</div>
							<div class="center-name">
								{{i+1}}
							</div>
						</div>
					</div>

				</div>
			</div>
		</div>
	</div>

</template>
<script>

</script>

<style scoped>
	.container {
		width: 100%;
		height: 100vh;
		/* overflow: hidden; */
		position: relative;
		background-color: #ffb8b8;


	}

	.content {
		width: 100%;
		/* height: 100%; */
		/* background-image: url('/src/assets/bg.jpg'); */
		/* background-size: cover; */
		/* background-position: center; */
		/* filter: blur(10px); */
		/* cursor: none; */
		/* 隐藏默认的指针样式 */
		z-index: 1;
		display: flex;
		justify-content: center;
		background-color: #ffb8b8;
		/* align-items: center; */

		overflow: auto;
		/* 内容超出时显示滚动条 */
	}

	.big-con {
		width: 96%;
		/* height: 100%; */
		/* height: 90%; */
		/* background-color: #FFF; */
		/* border-radius: 50px; */
		display: flex;
		flex-wrap: wrap;
		/* align-items: start; */
	}

	.con-title {
		width: 100%;
		/* height: 100px; */
		padding: 30px;
		font-size: 50px;
		color: #2f3542;
		text-align: center;
	}

	.left-con {
		width: 100%;
		/* height: 200px; */
		/* border: 1px solid; */
		/* border-right:10px solid #FFF ; */
		display: flex;
		flex-wrap: wrap;
		font-size: 40px;
		/* color: #FFF; */

		/* margin: 50px 0px; */
	}

	.left-name {
		/* width: 80%; */
		margin-right: 20px;

	}

	.left-item {
		display: flex;
		flex-wrap: wrap;
		margin: 20px;

		background-color: #FFF;

		border-radius: 30px;
		padding: 20px;
		/* justify-content: space-between; */
	}

	.center-con {
		width: 100%;
		/* height: 200px; */

		background-color: #FFF;
		border-radius: 30px;
		padding: 20px;
		margin: 50px 0px;
		display: flex;
		flex-wrap: wrap;
		color: #2f3542;
		/* justify-content: center; */
	}

	.center-item {
		width: 178px;
		border: 2px solid;
		margin: 10px;
		padding: 10px;
		border-radius: 10px;
		font-size: 30px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		cursor: pointer;
	}

	



	.center-item-a {
		width: 178px;
		border: 2px solid #2f3542;
		margin: 10px;
		padding: 10px;
		border-radius: 10px;
		font-size: 30px;
		display: flex;
		align-items: center;
		justify-content: space-between;
		color: #2f3542;
		background-color: #70a1ff;
		cursor: pointer;

	}

	.right-con {
		width: 00px;
	}

	.chou-btn {
		padding: 20px 50px;
		background-color: #70a1ff;
		margin: 20px;
		border-radius: 20px;
		font-size: 30px;
		border: 3px solid #2f3542;
		cursor: pointer;
	}

	.input-con {
		width: 100%;
		display: flex;
		justify-content: center;
	}

	.input-style {
		height: 50px;
		font-size: 30px;
		margin: 20px;
		width: 200px;
		text-align: center;

		border: 3px solid #2f3542;
		border-radius: 10px;
	}

	.center-title {
		width: 100%;
		text-align: center;
		font-size: 40px;
		font-weight: bold;
		padding: 20px;
	}

	.center-chou {
		width: 100%;
		text-align: center;
		font-size: 40px;
		font-weight: bold;
		padding: 0 20px 20px;
		display: flex;
		justify-content: center;
		align-items: center;
	}


	.center-chou-btn {

		color: #2f3542;
		background-color: #70a1ff;
		border: 0px;
		padding: 10px 50px;
		border: 3px solid #2f3542;
		margin-left: 20px;
		border-radius: 10px;
		font-weight: bold;
		font-size: 18px;
	}
</style>