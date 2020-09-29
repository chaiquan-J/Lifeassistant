<template>
	<view class="content">
		<template v-if="weather">
			<view class="weather_box">
				<view class="weather_cont">
					<view class="weather_right">
						<view class="weather_top">
							<uni-icons color="#FFF" type="location-filled" size="20"></uni-icons>
							<view class="weather_address">
								<text>{{ position }}</text>
							</view>
						</view>
						<view class="weather_center">
							<view class="big_degree">{{ weather.temp }}°</view>
							<view class="small_degree">
								<view class="weather_img">
									<image :src="weather.img" mode=""></image>
									<text>{{ weather.weather }}</text>
								</view>
								<text class="degree_text">{{ weather.templow }}°/{{ weather.temphigh }}°</text>
							</view>
						</view>
						<view class="weather_bottom">
							<view class="weather_date">{{ current_date }} {{ weather.week }}</view>
							<view class="other_text">
								<text class="aqi_text">空气质量 {{ weather.aqi.quality }}</text>
								<text class="humidity_text">湿度 {{ weather.humidity }}%</text>
							</view>
						</view>
					</view>
					<view class="weather_left" @click="openPopup">
						查看近7天预报
						<uni-icons color="#FFF" type="forward" size="20"></uni-icons>
					</view>
					<wyb-popup ref="sevendays" height="600" width="500" type="bottom" radius="6">
						<view class="popup-content">
							<view class="sevendays_cont">
								<view class="content"><mckou-weather :weatherData="weather.daily" ref="mckouWeather"></mckou-weather></view>
							</view>
						</view>
					</wyb-popup>
				</view>
				<view class="realtime_card">
					<view class="card_title">
						<text>实时预报</text>
						<view class="title_hr"></view>
					</view>
					<view class="sunrise_sunset">
						<view class="cont_box">
							<image src="../../static/richu.png" mode=""></image>
							<text>日出 {{ weather.daily[0].sunrise }}</text>
						</view>
						<view class="cont_box">
							<image src="../../static/riluo.png" mode=""></image>
							<text>日落 {{ weather.daily[0].sunset }}</text>
						</view>
					</view>
					<view class="realtime_box">
						<view class="cont_box">
							<scroll-view class="scroll_box" scroll-x="true">
								<template v-for="item in weather.hourly">
									<view class="scroll_cont">
										<view class="scroll_details">
											<text>{{ item.time }}</text>
											<image :src="item.img" mode=""></image>
											<text class="realtime_degree">{{ item.temp }}°</text>
											<text>{{ item.weather }}</text>
										</view>
									</view>
								</template>
							</scroll-view>
						</view>
					</view>
				</view>
				<view class="life_card">
					<view class="card_title">
						<text>生活卡片</text>
						<view class="title_hr"></view>
					</view>
					<view class="cont_box">
						<swiper class="life_swiper" :indicator-dots="false" :autoplay="true" :circular="true" :interval="5000" :duration="1000">
							<template v-for="item in weather.index">
								<swiper-item class="life_item">
									<view class="item_box">
										<view class="item_title">
											<image v-if="item.iname == '空调指数'" :src="life_src[0]" mode=""></image>
											<image v-else-if="item.iname == '运动指数'" :src="life_src[1]" mode=""></image>
											<image v-else-if="item.iname == '紫外线指数'" :src="life_src[2]" mode=""></image>
											<image v-else-if="item.iname == '感冒指数'" :src="life_src[3]" mode=""></image>
											<image v-else-if="item.iname == '洗车指数'" :src="life_src[4]" mode=""></image>
											<image v-else-if="item.iname == '空气污染扩散指数'" :src="life_src[5]" mode=""></image>
											<image v-else :src="life_src[6]" mode=""></image>
											<text>{{ item.iname }}</text>
										</view>
										<text class="item_ivalue">{{ item.ivalue }}</text>
										<text class="item_detail">{{ item.detail }}</text>
									</view>
								</swiper-item>
							</template>
						</swiper>
					</view>
				</view>
				<view class="weather_bg"></view>
			</view>
		</template>
		<view class="yellow_calendar">
			<view class="card_title">
				<text>黄历宜忌</text>
				<view class="title_hr"></view>
			</view>
			<view class="content_box">
				<view class="calendar_box">
					<view class="calendar_date">
						<view class="new_calendar">
							<text class="day">{{ new Date().getDate() }}</text>
							<text class="month">{{ new Date().getMonth() + 1 }}月</text>
						</view>
						<view class="lunar_calendar">{{ yellow_calendar.yinli }}</view>
					</view>
					<view class="calendar_hr"></view>
					<view class="calendar_taboo">
						<view class="taboo_box">
							<view class="taboo_round should">宜</view>
							<text>{{ yellow_calendar.yi }}</text>
						</view>
						<view class="taboo_box">
							<view class="taboo_round avoid">忌</view>
							<text>{{ yellow_calendar.ji }}</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="happy_moment">
			<view class="happy_title">
				<text>开心一刻</text>
				<view class="title_hr"></view>
			</view>
			<view class="happy_box" @click="Unfoldtext">
				<text class="box_title">标题标题</text>
				<view class="happy_text" :class="{ text_hidden: happy }">
					有一天晚上我俩一起吃西瓜，老大把西瓜籽很整洁的吐在了一张纸上很整洁的吐在了一张纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上
				</view>
			</view>
			<view class="pull_up">
				<uni-icons type="refreshempty" size="25"></uni-icons>
				上拉继续刷新更多鸭
			</view>
		</view>
	</view>
</template>

<script>
import uniIcons from '@/components/uni-icons/uni-icons.vue';
import wybPopup from '@/components/wyb-popup/wyb-popup.vue';
import mckouWeather from '@/components/mckou-weather/mckouWeather.vue';
export default {
	data() {
		return {
			happy: true,
			life_src: [
				'../../static/kongtiao.png',
				'../../static/yundong.png',
				'../../static/ziwaixian.png',
				'../../static/ganmao.png',
				'../../static/xiche.png',
				'../../static/kongqiwuran.png',
				'../../static/chuanyi.png'
			],
			weather: null,
			position: '',
			current_date: null,
			yellow_date: null,
			// 黄历假数据
			yellow_calendar: {
				baiji: '丙不修灶必见灾殃 午不苫盖屋主更张',
				chongsha: '冲鼠(庚子)煞北',
				id: '3780',
				ji: '入宅 作梁 安门 伐木 修造 上梁 入殓 盖屋',
				jishen: '月空 天马 时阳 生气 玉宇 鸣犬',
				wuxing: '天河水 开执位',
				xiongshen: '灾煞 天火 白虎',
				yangli: '2020-08-31',
				yi: '嫁娶 祭祀 祈福 求嗣 出行  动土 安床 掘井 破土 启钻',
				yinli: '庚子(鼠)年七月十三'
			}
		};
	},
	onLoad() {
		this.getpPosition();
		this.current_date = this.currentDate();
		// this.getYellow()
	},
	methods: {
		// 获取地址信息
		getpPosition() {
			let _this = this;
			uni.getLocation({
				type: 'wgs84',
				success: function(res) {
					// console.log(res);
					// console.log(JSON.stringify(res))
					// console.log('当前位置的经度：' + res.longitude);
					// console.log('当前位置的纬度：' + res.latitude);
					_this.getMap(res.latitude, res.longitude);
					// _this.$nextTick(() => {
					// 	_this.$refs.mckouWeather.init();
					// });
				},
				fail: err => {
					console.log(err);
				}
			});
		},
		// 获取黄历
		getYellow() {
			uni.request({
				url: 'http://v.juhe.cn/laohuangli/d',
				data: {
					key: '60e7cb332193de0a341c8415d8f88638',
					date: this.yellow_date
				},
				header: {
					'custom-header': 'json' //自定义请求头信息
				},
				success: res => {
					// console.log(res);
					this.yellow_calendar = res.data.result;
				}
			});
		},
		// 获取天气
		getMap(latitude, longitude) {
			let that = this;
			uni.request({
				url: 'http://apis.map.qq.com/ws/geocoder/v1/',
				data: {
					location: latitude + ',' + longitude,
					key: 'P6IBZ-OWZ3U-OE4VW-BEUKF-LKMH5-PDFJK'
				},
				header: {
					'custom-header': 'json' //自定义请求头信息
				},
				success: res => {
					// console.log(res.data.result.ad_info)
					this.position = res.data.result.ad_info.city;
					uni.request({
						url: 'https://way.jd.com/jisuapi/weather',
						data: {
							city: res.data.result.ad_info.city,
							appkey: '513c2d5bee6ec6a22531d764cf394ada'
						},
						success: res => {
							this.weather = this.modifyImg(res.data.result.result);
							console.log(this.weather);
							// this.weather = res.data.result.result
							// this.$nextTick(() => {
							// 	this.$refs.mckouWeather.init();
							// });
						}

						// }),
						// fail: err => {
						// 	console.log(err);
						// }
					});
				},
				fail: err => {
					console.log(err);
				}
			});
		},
		// 处理天气数据
		modifyImg(weather) {
			let oldweather = weather;
			let newWeather;
			// 处理当天天气图标
			if (oldweather.weather == '晴') {
				oldweather.img = '../../static/qing-0.png';
			} else if (oldweather.weather == '多云') {
				oldweather.img = '../../static/duoyun-0.png';
			} else if (oldweather.weather == '阴') {
				oldweather.img = '../../static/yin-0.png';
			} else if (oldweather.weather == '阵雨') {
				oldweather.img = '../../static/zhenyu-0.png';
			} else if (oldweather.weather == '冻雨') {
				oldweather.img = '../../static/dongyu-0.png';
			} else if (oldweather.weather == '雨') {
				oldweather.img = '../../static/xiaoyu-0.png';
			} else if (oldweather.weather == '中雨') {
				oldweather.img = '../../static/zhongyu-0.png';
			} else if (oldweather.weather == '大雨') {
				oldweather.img = '../../static/dayu-0.png';
			} else if (oldweather.weather == '暴雨') {
				oldweather.img = '../../static/baoyu-0.png';
			} else if (oldweather.weather == '大暴雨') {
				oldweather.img = '../../static/dabaoyu-0.png';
			} else if (oldweather.weather == '特大暴雨') {
				oldweather.img = '../../static/tedabaoyu-0.png';
			} else if (oldweather.weather == '雷阵雨') {
				oldweather.img = '../../static/leizhenyu-0.png';
			} else if (oldweather.weather == '雷阵雨伴冰雹') {
				oldweather.img = '../../static/leizhenyubanbingbao-0.png';
			} else if (oldweather.weather == '阵雪') {
				oldweather.img = '../../static/zhenxue-0.png';
			} else if (oldweather.weather == '小雪') {
				oldweather.img = '../../static/xiaoxue-0.png';
			} else if (oldweather.weather == '中雪') {
				oldweather.img = '../../static/zhongxue-0.png';
			} else if (oldweather.weather == '大雪') {
				oldweather.img = '../../static/daxue-0.png';
			} else if (oldweather.weather == '暴雪') {
				oldweather.img = '../../static/yujiaxue-0.png';
			} else if (oldweather.weather == '雨夹雪') {
				oldweather.img = '../../static/yujiaxue-0.png';
			} else if (oldweather.weather == '雾') {
				oldweather.img = '../../static/wu-0.png';
			} else if (oldweather.weather == '沙尘暴') {
				oldweather.img = '../../static/shachenbao-0.png';
			} else if (oldweather.weather == '强沙尘暴') {
				oldweather.img = '../../static/qiangshachenbao-0.png';
			} else if (oldweather.weather == '浮尘') {
				oldweather.img = '../../static/fuchen-0.png';
			} else {
				oldweather.img = '../../static/yangsha-0.png';
			}
			// 处理24小时图标
			for (let i = 0; i < oldweather.hourly.length; i++) {
				if (oldweather.hourly[i].weather == '晴') {
					oldweather.hourly[i].img = '../../static/qing-0.png';
				} else if (oldweather.hourly[i].weather == '多云') {
					oldweather.hourly[i].img = '../../static/duoyun-0.png';
				} else if (oldweather.hourly[i].weather == '阴') {
					oldweather.hourly[i].img = '../../static/yin-0.png';
				} else if (oldweather.hourly[i].weather == '阵雨') {
					oldweather.hourly[i].img = '../../static/zhenyu-0.png';
				} else if (oldweather.hourly[i].weather == '冻雨') {
					oldweather.hourly[i].img = '../../static/dongyu-0.png';
				} else if (oldweather.hourly[i].weather == '小雨') {
					oldweather.hourly[i].img = '../../static/xiaoyu-0.png';
				} else if (oldweather.hourly[i].weather == '中雨') {
					oldweather.hourly[i].img = '../../static/zhongyu-0.png';
				} else if (oldweather.hourly[i].weather == '大雨') {
					oldweather.hourly[i].img = '../../static/dayu-0.png';
				} else if (oldweather.hourly[i].weather == '暴雨') {
					oldweather.hourly[i].img = '../../static/baoyu-0.png';
				} else if (oldweather.hourly[i].weather == '大暴雨') {
					oldweather.hourly[i].img = '../../static/dabaoyu-0.png';
				} else if (oldweather.hourly[i].weather == '特大暴雨') {
					oldweather.hourly[i].img = '../../static/tedabaoyu-0.png';
				} else if (oldweather.hourly[i].weather == '雷阵雨') {
					oldweather.hourly[i].img = '../../static/leizhenyu-0.png';
				} else if (oldweather.hourly[i].weather == '雷阵雨伴冰雹') {
					oldweather.hourly[i].img = '../../static/leizhenyubanbingbao-0.png';
				} else if (oldweather.hourly[i].weather == '阵雪') {
					oldweather.hourly[i].img = '../../static/zhenxue-0.png';
				} else if (oldweather.hourly[i].weather == '小雪') {
					oldweather.hourly[i].img = '../../static/xiaoxue-0.png';
				} else if (oldweather.hourly[i].weather == '中雪') {
					oldweather.hourly[i].img = '../../static/zhongxue-0.png';
				} else if (oldweather.hourly[i].weather == '大雪') {
					oldweather.hourly[i].img = '../../static/daxue-0.png';
				} else if (oldweather.hourly[i].weather == '暴雪') {
					oldweather.hourly[i].img = '../../static/yujiaxue-0.png';
				} else if (oldweather.hourly[i].weather == '雨夹雪') {
					oldweather.hourly[i].img = '../../static/yujiaxue-0.png';
				} else if (oldweather.hourly[i].weather == '雾') {
					oldweather.hourly[i].img = '../../static/wu-0.png';
				} else if (oldweather.hourly[i].weather == '沙尘暴') {
					oldweather.hourly[i].img = '../../static/shachenbao-0.png';
				} else if (oldweather.hourly[i].weather == '强沙尘暴') {
					oldweather.hourly[i].img = '../../static/qiangshachenbao-0.png';
				} else if (oldweather.hourly[i].weather == '浮尘') {
					oldweather.hourly[i].img = '../../static/fuchen-0.png';
				} else {
					oldweather.hourly[i].img = '../../static/yangsha-0.png';
				}
			}
			// 处理天天气日期
			for (let i = 0; i < oldweather.daily.length; i++) {
				oldweather.daily[i].nwedate = oldweather.daily[i].date.substring(5,10)
			}
			newWeather = oldweather;
			return newWeather;
		},
		// 当前日期
		currentDate() {
			let date = new Date();
			let dateMonth = date.getMonth() + 1;
			let dateDay = date.getDate();
			let dateYer = date.getFullYear();
			if (dateMonth < 10) {
				dateMonth = '0' + dateMonth;
			}
			if (dateDay < 10) {
				dateDay = '0' + dateDay;
			}
			this.yellow_date = dateYer + '-' + dateMonth + '-' + dateDay;
			return dateMonth + '月' + dateDay + '日';
		},
		// 展开收起笑话 有bug
		Unfoldtext(e) {
			this.happy = false;
		},
		// 7天预报弹出
		openPopup() {
			this.$refs.sevendays.show();
		}
	},
	components: {
		uniIcons,
		wybPopup,
		mckouWeather
	}
};
</script>

<style lang="less">
@import url('../../less/index.less');
</style>
