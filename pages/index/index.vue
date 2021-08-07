<template>
	<view >
		<luyj-choose-img @imgChange="imgChange"></luyj-choose-img>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		methods:{
			imgChange : function(imgList){
				console.log("变换的图片" , imgList);
			}
		}
			
		
	}
</script>

<style>
	
</style>
