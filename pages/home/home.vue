<template>
	<view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item, i) in swiperList" :key="i">
				<navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
					<!-- 传参数item.goods_id -->
					<image :src="item.image_src"></image>
				</navigator>
			</swiper-item>
		</swiper>
		<view class="nav-list">
			<view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navClickHandler(item)">
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
		<view class="floor-list">
			<!-- 楼层 item 项 -->
			<view class="floor-item" v-for="(item, i) in floorList" :key="i">
				<!-- 楼层标题 -->
				<image :src="item.floor_title.image_src" class="floor-title"></image>
				<view class="floor-img-box">
				  <!-- 左侧大图片的盒子 -->
				  <navigator class="left-img-box" :url="item.product_list[0].url">
				    <image :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
				  </navigator>
				  <!-- 右侧 4 个小图片的盒子 -->
				  <view class="right-img-box">
				    <navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0" :url="item2.url">
				      <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
				    </navigator>
				  </view>
				</view>
			</view>
		</view>
		<!-- 楼层图片区域 -->
	</view>
</template>

<script>
export default {
	data() {
		return {
			swiperList: [],
			navList: [],
			floorList: []
		};
	},
	onLoad() {
		this.getswiperList();
		this.getNavList();
		this.getFloorList();
	},
	methods: {
		async getswiperList() {
			// 3.1 发起请求
			const { data: res } = await uni.$http.get('/api/public/v1/home/swiperdata');
			// 3.2 请求失败
			if (res.meta.status !== 200) return uni.$showMsg();
			// 3.3 请求成功，为 data 中的数据赋值
			this.swiperList = res.message;
		},
		async getNavList() {
			const { data: res } = await uni.$http.get('/api/public/v1/home/catitems');
			if (res.meta.status !== 200) return uni.$showMsg();
			this.navList = res.message;
		},
		navClickHandler(item) {
			if (item.name == '分类') {
				uni.switchTab({
					url: '/pages/cate/cate'
				});
			}
		},
		async getFloorList() {
			const { data: res } = await uni.$http.get('/api/public/v1/home/floordata');
			if (res.meta.status !== 200) return uni.$showMsg();
			this.floorList = res.message;
			res.message.forEach(floor => {
			    floor.product_list.forEach(prod => {
			      prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
			    })
			  })
			console.log(res)
		},
	}
};
</script>

<style lang="scss">
swiper {
	height: 330rpx;
	.swiper-item,
	image {
		width: 100%;
		height: 100%;
	}
}
.nav-list {
	display: flex;
	justify-content: space-around;
	margin: 15px 0;

	.nav-img {
		width: 128rpx;
		height: 140rpx;
	}
}
.floor-title {
	height: 60rpx;
	width: 100%;
	display: flex;
}
.right-img-box {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-around;
}

.floor-img-box {
	display: flex;
	padding-left: 10rpx;
}
.left-img{
	
}
</style>
