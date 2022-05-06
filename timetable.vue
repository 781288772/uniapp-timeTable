<template>
	<view class="timetable">
		<view class="header">
			<view class="header-item" v-for="(item,index) in week" :key="item">
				<view :style="{'writing-mode':index==0?'vertical-rl':'','margin-top':index==0?'15rpx':'0','line-height':index==0?'80rpx':'45rpx'}">
					{{todayWeekIndex === index?'今天':item}}</view>
				<view :style="{'fontWeight':600,'color':'#000'}">{{index==0?'':index}}</view>
			</view>
		</view>
		<view class="main">
			<view class="row" v-for="(item,index) in timetableType" :key="index">
				<view class="time-item">
					<view class="time">
						<view>{{ item.time1 }}</view>
						<view>-</view>
						<view>{{item.time2}}</view>
					</view>


				</view>
			</view>

			<view class="container">
				<view class="week" v-for="(week, weekIndex) in courseData" :key="weekIndex">
					<view class="courseList" v-for="(course, courseIndex) in week" :key="courseIndex">
						<view @click="handleCourseClick(course, weekIndex, courseIndex)" class="course"
							:style="{ height: 90 + 'rpx', background: course.backgroundColor }"
							v-if="course.length > 0">{{ course.name?'空闲':'' }}</view>
					</view>
				</view>
			</view>
		</view>
		<br>
		<br>
		<br>
	</view>
</template>

<script>
	export default {
		name: 'Timetable',
		props: {
			timetableType: {
				type: Array,
				default: () => {
					return [{
							index: '1',
							time1: '09:00',
							time2: '10:00',
						},
						{
							index: '2',
							time1: '10:00',
							time2: '11:00',
						},
						{
							index: '3',
							time1: '11:00',
							time2: '12:00',
						},
						{
							index: '4',
							time1: '14:00',
							time2: '15:00',
						},
						{
							index: '5',
							time1: '15:00',
							time2: '16:00',
						},
						{
							index: '6',
							time1: '16:00',
							time2: '17:00',
						},
						{
							index: '7',
							time1: '17:00',
							time2: '18:00',
						},
						{
							index: '8',
							time1: '18:00',
							time2: '19:00',
						},

					]
				}
			},
			week: {
				type: Array,
				default: () => {
					return ['排班', '周日', '周一', '周二', '周三', '周四', '周五', '周六']
				}
			},
			timetables: {
				type: Array,
				default: () => {
					return []
				}
			},
			palette: {
				type: Array,
				default: () => {
					return []
				}
			}
		},
		data() {
			return {
				allPalette: ['#10CFAB']
		
			}
		},
		computed: {
			courseData() {
				// 为数据标记背景颜色的函数
				let paletteIndex = 0
				const getBackgroundColor = () => {
					const backgroundColor = this.allPalette[paletteIndex]
					paletteIndex++
					if (paletteIndex >= this.allPalette.length) {
						paletteIndex = 0
					}
					return backgroundColor
				}

				// 数据处理
				const listMerge = []
				this.timetables.forEach(function(list, i) {
					if (!listMerge[i]) {
						listMerge[i] = []
					}
					list.forEach(function(item, index) {
						if (!index) {
							return listMerge[i].push({
								name: item,
								length: 1,
								backgroundColor: item !== true ? '#f2f2f2' : getBackgroundColor()
							})
						}
						return listMerge[i].push({
							name: item,
							length: 1,
							backgroundColor: item !== true ? '#f2f2f2' : getBackgroundColor()
						})
					})
				})
				return listMerge
			},
			todayWeekIndex() {
				let weekIndex = new Date().getDay() + 1
				if (weekIndex === -1) {
					weekIndex = 6
				}
				return weekIndex
			}
		},
		methods: {
			handleCourseClick(course, weekIndex, courseIndex) {
				const data = {
					index: courseIndex + 1,
					length: course.length,
					week: this.week[weekIndex],
					weekIndex: weekIndex,
					name: course.name
				}
				console.log(`星期${data.week}; 第${data.index}节课; 课程名:${data.name}; 课节:${data.length}`)
				console.log(data)
				this.$emit('courseClick', data)
			}
		}
	}
</script>

<style scoped lang="scss">
	.timetable {
		background: white;
		// border: 1px solid #E4E7ED;
		border-radius: 8rpx;
		overflow: auto;
		margin: 0 30rpx;
		box-sizing: content-box;

		.header {
			// padding-left: 88rpx;
			height: 90rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			// background: #F5F7FA;
			overflow-x: auto;
			width: 139%;

			.header-item {
				// flex: 1;
				justify-content: space-between;
				text-align: center;
				background-color: #F2F2F2;
				margin: 5rpx;
				width: 150rpx;
				height: 90rpx;
				line-height: 45rpx;
				color: #9C9C9C;

				&:first-child {
					width: 80rpx;
				}
			}

		}

		.main {
			position: relative;
			// margin-top: 10rpx;
			background-color: #fff;
			width: 100%;

			.row {
				height: 90rpx;
				position: relative;
				box-sizing: border-box;
				margin-top: 10rpx;
				margin-bottom: 10rpx;
				width: 105rpx;

				// &:after {
				// 	content: '';
				// 	height: 0;
				// 	width: 100%;
				// 	position: absolute;
				// 	bottom: 0;
				// 	left: 0;
				// 	border-bottom: 1rpx dashed #E4E7ED;
				// }

				.time-item {
					height: 100%;
					width: 90rpx;
					text-align: center;
					padding: 0 5rpx;

					.time {
						background: #F2F2F2;

						width: 100%;
						height: 100%;
						font-size: 20rpx;
						display: flex;
						flex-direction: column;
						align-items: center;
						line-height: 30rpx;

					}

					.index {
						color: #909399;
						padding-bottom: 8rpx;
						padding-top: 16rpx;
					}
				}

			}

			.container {
				position: absolute;
				top: 0;
				left: 88rpx;
				// width: calc(100% - 88rpx);
				height: 100%;
				width: 100%;
				display: flex;
				// margin-left: 5rpx;
				justify-content: space-between;
				padding-left: 8rpx;
				// padding-right: 90rpx;



				.week {
					// flex: 1;
					// width: 0;
					// flex-grow: 1;
					display: flex;
					flex-direction: column;
					margin-right: 10rpx;
					// margin: 5rpx;


					.courseList {
						// word-break: break-all;
						// color: white;
						// overflow: hidden;
						box-sizing: border-box;
						width: 115rpx;
						// margin: 5rpx;
						margin-bottom: 10rpx;

						.course {
							// padding: 8rpx;
							border-radius: 3px;
							text-align: center;
							// width: 110rpx;
							// height: 90rpx;
							line-height: 90rpx;
							box-sizing: border-box;

						}
					}
				}
			}
		}
	}
</style>
