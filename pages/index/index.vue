<template>
	<view class="content">
		<template v-if="weather">
			<view class="weather_box">
				<view class="weather_cont">
					<view class="weather_right">
						<view class="weather_top">
							<uni-icons color="#FFF" type="location-filled" size="20"></uni-icons>
							<view class="weather_address">
								<text>{{position}}</text>
							</view>
						</view>
						<view class="weather_center">
							<view class="big_degree">
								{{weather.temp}}°
							</view>
							<view class="small_degree">
								<view class="weather_img">
									<image :src="weather.img" mode=""></image>
									<text>{{weather.weather}}</text>
								</view>
								<text class="degree_text">{{weather.templow}}°/{{weather.temphigh}}°</text>
							</view>
						</view>
						<view class="weather_bottom">
							<view class="weather_date">
								{{current_date}} {{weather.week}}
							</view>
							<view class="other_text">
								<text class="aqi_text">空气质量 {{weather.aqi.quality}}</text>
								<text class="humidity_text">湿度 {{weather.humidity}}%</text>
							</view>
						</view>
					</view>
					<view class="weather_left">
						查看近7天预报
						<uni-icons color="#FFF" type="forward" size="20"></uni-icons>
					</view>
				</view>
				<view class="realtime_card">
					<view class="card_title">
						<text>实时预报</text>
						<view class="title_hr"></view>
					</view>
					<view class="sunrise_sunset">
						<view class="cont_box">
							<image src="../../static/richu.png" mode=""></image>
							<text>日出 {{weather.daily[0].sunrise}}</text>
						</view>
						<view class="cont_box">
							<image src="../../static/riluo.png" mode=""></image>
							<text>日落 {{weather.daily[0].sunset}}</text>
						</view>
					</view>
					<view class="realtime_box">
						<view class="cont_box">
							<scroll-view class="scroll_box" scroll-x="true">
								<template v-for="item in weather.hourly">
									<view class="scroll_cont">
										<view class="scroll_details">
											<text>{{item.time}}</text>
											<image :src="item.img" mode=""></image>
											<text class="realtime_degree">{{item.temp}}°</text>
											<text>{{item.weather}}</text>
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
										<text>{{item.iname}}</text>
									</view>
									<text class="item_ivalue">{{item.ivalue}}</text>
									<text class="item_detail">{{item.detail}}</text>
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
							<text class="day">{{new Date().getDate()}}</text>
							<text class="month">{{new Date().getMonth()+1}}月</text>
						</view>
						<view class="lunar_calendar">
							七月初七
						</view>
					</view>
					<view class="calendar_hr"></view>
					<view class="calendar_taboo">
						<view class="taboo_box">
							<view class="taboo_round should">
								宜
							</view>
							<text>嫁娶、订盟、纳采、出行、开市、出行、开市出行、开市出行、开市</text>
						</view>
						<view class="taboo_box">
							<view class="taboo_round avoid">
								忌
							</view>
							<text>动土、破土</text>
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
				<view class="happy_text" :class="{'text_hidden':happy}">有一天晚上我俩一起吃西瓜，老大把西瓜籽很整洁的吐在了一张纸上很整洁的吐在了一张纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上纸上很整洁的吐在了一张纸上</view>
			</view>
			<view class="happy_box" @click="Unfoldtext">
				<view class="happy_text" :class="{'text_hidden':happy}">有一天晚上我俩一起吃西瓜，老大把西瓜籽很整洁的吐在了一张纸上</view>
			</view>
			<view class="happy_box" @click="Unfoldtext">
				<view class="happy_text" :class="{'text_hidden':happy}">有一天晚上我俩一起吃西瓜，老大把西瓜籽很整洁的吐在了一张纸上</view>
			</view>
			<view class="pull_up">
				<uni-icons type="refreshempty" size="25"></uni-icons>
				上拉继续刷新更多鸭
			</view>
		</view>
	</view>
</template>

<script>
	import uniIcons from "@/components/uni-icons/uni-icons.vue"
	export default {
		data() {
			return {
				happy: true,
				life_src: ["../../static/kongtiao.png", "../../static/yundong.png", "../../static/ziwaixian.png",
					"../../static/ganmao.png", "../../static/xiche.png", "../../static/kongqiwuran.png", "../../static/chuanyi.png"
				],
				weather: "",
				position: "",
				current_date: null
			}
		},
		onLoad() {
			this.getpPosition()
			this.current_date = this.currentDate()
		},
		onShow() {

		},
		methods: {
			// 获取地址信息
			getpPosition() {
				let _this = this
				uni.getLocation({
					type: 'wgs84',
					success: function(res) {
						// console.log(res)
						// console.log('当前位置的经度：' + res.longitude);
						// console.log('当前位置的纬度：' + res.latitude);
						_this.getMap(res.latitude, res.longitude)
					},
					fail: (err) => {
						console.log(err)
					}
				});
			},
			getMap(latitude, longitude) {
				uni.request({
					url: "http://apis.map.qq.com/ws/geocoder/v1/",
					data: {
						location: latitude + "," + longitude,
						key: "P6IBZ-OWZ3U-OE4VW-BEUKF-LKMH5-PDFJK",
					},
					header: {
						'custom-header': 'json' //自定义请求头信息
					},
					success: (res) => {
						this.position = res.data.result.ad_info.city
						// console.log(res.data.result.ad_info)
						uni.request({
							url: "https://way.jd.com/jisuapi/weather",
							data: {
								city: res.data.result.ad_info.city,
								appkey: "513c2d5bee6ec6a22531d764cf394ada"
							},
							success: (res) => {
								// console.log(res)
								// this.weather = res.data.result.result
								this.weather = this.modifyImg(res.data.result.result)
								console.log(this.weather)
							},
							fail: (err) => {
								console.log(err)
							}
						})
					},
					fail: (err) => {
						console.log(err)
					}
				})
			},
			// 处理数据
			modifyImg(weather) {
				let oldweather = weather
				let newWeather
				if (oldweather.weather == '晴') {
					oldweather.img = '../../static/qing-0.png'
				} else if (oldweather.weather == '多云') {
					oldweather.img = '../../static/duoyun-0.png'
				} else if (oldweather.weather == '阴') {
					oldweather.img = '../../static/yin-0.png'
				} else if (oldweather.weather == '阵雨') {
					oldweather.img = '../../static/zhenyu-0.png'
				} else if (oldweather.weather == '冻雨') {
					oldweather.img = '../../static/dongyu-0.png'
				} else if (oldweather.weather == '小雨') {
					oldweather.img = '../../static/xiaoyu-0.png'
				} else if (oldweather.weather == '中雨') {
					oldweather.img = '../../static/zhongyu-0.png'
				} else if (oldweather.weather == '大雨') {
					oldweather.img = '../../static/dayu-0.png'
				} else if (oldweather.weather == '暴雨') {
					oldweather.img = '../../static/baoyu-0.png'
				} else if (oldweather.weather == '大暴雨') {
					oldweather.img = '../../static/dabaoyu-0.png'
				} else if (oldweather.weather == '特大暴雨') {
					oldweather.img = '../../static/tedabaoyu-0.png'
				} else if (oldweather.weather == '雷阵雨') {
					oldweather.img = '../../static/leizhenyu-0.png'
				} else if (oldweather.weather == '雷阵雨伴冰雹') {
					oldweather.img = '../../static/leizhenyubanbingbao-0.png'
				} else if (oldweather.weather == '阵雪') {
					oldweather.img = '../../static/zhenxue-0.png'
				} else if (oldweather.weather == '小雪') {
					oldweather.img = '../../static/xiaoxue-0.png'
				} else if (oldweather.weather == '中雪') {
					oldweather.img = '../../static/zhongxue-0.png'
				} else if (oldweather.weather == '大雪') {
					oldweather.img = '../../static/daxue-0.png'
				} else if (oldweather.weather == '暴雪') {
					oldweather.img = '../../static/yujiaxue-0.png'
				} else if (oldweather.weather == '雨夹雪') {
					oldweather.img = '../../static/yujiaxue-0.png'
				} else if (oldweather.weather == '雾') {
					oldweather.img = '../../static/wu-0.png'
				} else if (oldweather.weather == '沙尘暴') {
					oldweather.img = '../../static/shachenbao-0.png'
				} else if (oldweather.weather == '强沙尘暴') {
					oldweather.img = '../../static/qiangshachenbao-0.png'
				} else if (oldweather.weather == '浮尘') {
					oldweather.img = '../../static/fuchen-0.png'
				} else {
					oldweather.img = '../../static/yangsha-0.png'
				}
				for (let i = 0; i < oldweather.hourly.length; i++) {
					if (oldweather.hourly[i].weather == '晴') {
						oldweather.hourly[i].img = '../../static/qing-0.png'
					} else if (oldweather.hourly[i].weather == '多云') {
						oldweather.hourly[i].img = '../../static/duoyun-0.png'
					} else if (oldweather.hourly[i].weather == '阴') {
						oldweather.hourly[i].img = '../../static/yin-0.png'
					} else if (oldweather.hourly[i].weather == '阵雨') {
						oldweather.hourly[i].img = '../../static/zhenyu-0.png'
					} else if (oldweather.hourly[i].weather == '冻雨') {
						oldweather.hourly[i].img = '../../static/dongyu-0.png'
					} else if (oldweather.hourly[i].weather == '小雨') {
						oldweather.hourly[i].img = '../../static/xiaoyu-0.png'
					} else if (oldweather.hourly[i].weather == '中雨') {
						oldweather.hourly[i].img = '../../static/zhongyu-0.png'
					} else if (oldweather.hourly[i].weather == '大雨') {
						oldweather.hourly[i].img = '../../static/dayu-0.png'
					} else if (oldweather.hourly[i].weather == '暴雨') {
						oldweather.hourly[i].img = '../../static/baoyu-0.png'
					} else if (oldweather.hourly[i].weather == '大暴雨') {
						oldweather.hourly[i].img = '../../static/dabaoyu-0.png'
					} else if (oldweather.hourly[i].weather == '特大暴雨') {
						oldweather.hourly[i].img = '../../static/tedabaoyu-0.png'
					} else if (oldweather.hourly[i].weather == '雷阵雨') {
						oldweather.hourly[i].img = '../../static/leizhenyu-0.png'
					} else if (oldweather.hourly[i].weather == '雷阵雨伴冰雹') {
						oldweather.hourly[i].img = '../../static/leizhenyubanbingbao-0.png'
					} else if (oldweather.hourly[i].weather == '阵雪') {
						oldweather.hourly[i].img = '../../static/zhenxue-0.png'
					} else if (oldweather.hourly[i].weather == '小雪') {
						oldweather.hourly[i].img = '../../static/xiaoxue-0.png'
					} else if (oldweather.hourly[i].weather == '中雪') {
						oldweather.hourly[i].img = '../../static/zhongxue-0.png'
					} else if (oldweather.hourly[i].weather == '大雪') {
						oldweather.hourly[i].img = '../../static/daxue-0.png'
					} else if (oldweather.hourly[i].weather == '暴雪') {
						oldweather.hourly[i].img = '../../static/yujiaxue-0.png'
					} else if (oldweather.hourly[i].weather == '雨夹雪') {
						oldweather.hourly[i].img = '../../static/yujiaxue-0.png'
					} else if (oldweather.hourly[i].weather == '雾') {
						oldweather.hourly[i].img = '../../static/wu-0.png'
					} else if (oldweather.hourly[i].weather == '沙尘暴') {
						oldweather.hourly[i].img = '../../static/shachenbao-0.png'
					} else if (oldweather.hourly[i].weather == '强沙尘暴') {
						oldweather.hourly[i].img = '../../static/qiangshachenbao-0.png'
					} else if (oldweather.hourly[i].weather == '浮尘') {
						oldweather.hourly[i].img = '../../static/fuchen-0.png'
					} else {
						oldweather.hourly[i].img = '../../static/yangsha-0.png'
					}
				}
				newWeather = oldweather
				return newWeather
			},
			// 当前日期
			currentDate() {
				let date = new Date()
				let dateMonth = date.getMonth() + 1
				let dateDay = date.getDate()
				if (dateMonth < 10) {
					dateMonth = '0' + dateMonth
				}
				if (dateDay < 10) {
					dateDay = '0' + dateDay
				}
				return dateMonth + '月' + dateDay + '日'
			},
			// 展开收起笑话 有bug
			Unfoldtext(e) {
				this.happy = false
			}
		},
		components: {
			uniIcons
		}
	}
</script>

<style lang="less">
	@import url('../../less/index.less');
</style>
