<template>
	<view>
		<view class="nav-list">
		   <view class="nav-item" v-for="(item, i) in navList" :key="i">
		     <image :src="item.image_src" class="nav-img"></image>
		   </view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		 onLoad() {
		    // 2. 在 onLoad 中调用获取数据的方法
		    
		  },
		methods: {
			
		}
	}
</script>

<style>

</style>
