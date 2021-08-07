<template>
	<view class="bg-white">
		<view v-if="showTitleBar" class=" cu-bar  padding-sm ">
			<view class="action text-bold text-xl">
				{{ titleText }}
			</view>
			<view class="action">
				{{imgList.length}}/{{imgMaxCount}}
			</view>
		</view>
		<view class="cu-form-group padding-left-xl solid-bottom margin-top">
			<view class="grid col-4 grid-square flex-sub">
				<view class="bg-img" v-for="(item,index) in imgList" :key="index" @tap="ViewImage" :data-url="imgList[index]">
				 <image :src="imgList[index]" mode="aspectFill"></image>
					<view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
						<text class='cuIcon-close'></text>
					</view>
				</view>
				<!-- 添加图片 -->
				<view class="solids" @tap="ChooseImage" v-if="imgList.length< imgMaxCount">
					<text class='cuIcon-cameraadd'></text>
				</view>
			</view>
		</view>
	</view>
</template>
<script>
	/**
	 * luyj-choose-img 基础ColorUI的选择图片组插件
	 * @description  基于Color UI的选择图片组插件
	 * @tutorial url https://ext.dcloud.net.cn/plugin?id=5724
	 * @property {String} titleText 默认标题（默认值图片组）
	 * @property {Boolean} showTitleBar 是否展示标题栏（默认值true）
	 * @property {Number} imgMaxCount 最大上传图片数（默认9）
	 * @event {Function()} imgChange 修改图片后当前的图片组 
	 */
	export default {
		name :"luyj-choose-img",
		props:{
			// 默认标题
			titleText :{
				type: String,
				default:'图片组'
			},
			// 是否展示标题栏
			showTitleBar :{
				type: Boolean,
				default: true
			},
			// 最大图片数
			imgMaxCount : {
				type : Number,
				default: 9,
			}
		},
		data(){
			return {
				imgList:[],				// 用于上传的图片组
			}
		},
		methods:{
			/**
			 * 选择图片
			 */
			ChooseImage : function() {
				uni.chooseImage({
					count: this.imgMaxCount - this.imgList.length,  // 默认上传图片数
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						if (this.imgList.length != 0) {
							this.imgList = this.imgList.concat(res.tempFilePaths);
						} else {
							this.imgList = res.tempFilePaths;
						}
						this.$emit("imgChange", this.imgList);		// 变换图片
					}
				});
			},
			/** 查看图片
			 * @param {Object} e
			 */
			ViewImage : function (e) {
				uni.previewImage({
					urls: this.imgList,
					current: e.currentTarget.dataset.url
				});
			},
			/** 删除图片
			 * @param {Object} e
			 */
			DelImg: function(e) {
				this.imgList.splice(e.currentTarget.dataset.index, 1); // 删除当前图片
				this.$emit("imgChange", this.imgList);				   // 变换图片
			},
		}
	}
</script>
<style lang="scss" scoped>
</style>
