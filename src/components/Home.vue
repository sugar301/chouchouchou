<script setup>
	import {
		ref
	} from 'vue'

	defineProps({
		msg: String
	})

	const count = ref(0)

	const getImageSize = (event)=> {
      const img = event.target;
	  console.log(img.naturalWidth)
	  console.log(img.naturalHeight)
      // this.imageWidth = img.naturalWidth;
      // this.imageHeight = img.naturalHeight;
    }
	
</script>

<template>

 <div class="container">
    <div class="content" >
      <!-- 内容区域 -->
	  <div class="content-z">
		  <!-- <div class="img-con">
			  <img  src="/src/assets/bg.jpg"  @load="getImageSize" style="width: 1600px;height: 900px;"/>
		  </div>
		  <div class="img-ms">
			  <div class="ms-title">
					miaoshu
			  </div>
		  </div> -->
	  </div>
    </div>
  </div>

</template>

<style scoped>


	.container {
		width: 100%;
		height: 100vh;
		overflow: hidden;
		position: relative;
	}

	.content {
		width: 100%;
		height: 100%;
		background-image: url('/src/assets/bg.jpg');
		background-size: cover;
		background-position: center;
		/* filter: blur(10px); */
		/* cursor: none; */
		/* 隐藏默认的指针样式 */
		z-index: 1;
	    /* filter: blur(50px); */
	}

	.content:after{
	    content: "";
	    width:100%;
	    height:100%;
	    position: absolute;
	    left:0;
	    top:0;
	    background: inherit;
	    filter: blur(10px);
	    z-index: 2;
		
		/* background-color: rgba(0, 0, 0, 0.8); */
	}
	.content-z{
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.1);
		position: absolute;
		z-index: 3;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-wrap: wrap;
	}
	.img-con{
	/* 	width: 1600px;
		height: 900px; */
		/* border: 6px solid #fff; */
		/* background-image: url('/src/assets/bg.jpg'); */
		box-shadow: 0px 0px 25px rgba(0,0,0,0.5);
	}
	.img-ms{
		width: 100%;
		/* border: 1px solid; */
		height: 100px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.ms-title{
		color: #FFF;
		font-weight: bold;
		font-size: 30px;
	}

	
</style>