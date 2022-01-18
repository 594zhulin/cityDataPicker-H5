<template>
	<view class="city-picker">
		<picker mode="multiSelector" :range="regions" :value="regionIndex" range-key="name"
			@columnchange="getRegionData" @change="getRegionCode">
			<input class="city-picker-input" v-model="regionText" placeholder="请选择省市区" disabled />
		</picker>
	</view>
</template>

<script>
	import {
		provinces
	} from '../../utils/provinces.js';
	import {
		cities
	} from '../../utils/cities.js';
	import {
		areas
	} from '../../utils/areas.js';
	export default {
		data() {
			return {
				regions: [],
				regionIndex: [0, 0, 0],
				regionText: '',
				regionCode: '',
			}
		},
		onLoad() {
			this.regions[0] = provinces;
			this.regions[1] = cities.filter(item => item.provinceCode === this.regions[0][0].code);
			this.regions[2] = areas.filter(item => item.cityCode === this.regions[1][0].code);
		},
		methods: {
			getRegionData(e) {
				this.regionIndex[e.detail.column] = e.detail.value;
				const column = e.detail.column;
				switch (column) {
					case 0:
						this.regions[1] = cities.filter(item => item.provinceCode === this.regions[0][e.detail.value]
							.code);
						this.regions[2] = areas.filter(item => item.cityCode === this.regions[1][0].code);
						this.regionIndex.splice(1, 1, 0);
						this.regionIndex.splice(2, 1, 0);
						break;
					case 1:
						this.regions[2] = areas.filter(item => item.cityCode === this.regions[1][e.detail.value].code);
						this.regionIndex.splice(2, 1, 0);
						break;
					default:
						break;
				}
				this.$forceUpdate();
			},
			getRegionCode(e) {
				const provinceText = this.regions[0][e.detail.value[0]].name;
				const cityText = this.regions[1][e.detail.value[1]].name;
				const areaText = this.regions[2][e.detail.value[2]].name;
				this.regionText = [provinceText, cityText, areaText].join('/');
				this.regionCode = this.regions[2][e.detail.value[2]].code;
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
