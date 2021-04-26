<template>
	<view class="Home_Container">
		<view class="Top">
			<view class="top_text">
				资料库
			</view>
		</view>
		<view class="Mulu">
			<view class="function">
				<view class="left">
					<view class="function_icon">

					</view>
					<view class="function_name">
						播放列表
					</view>
				</view>
				<view class="right">
					<uni-icons type="forward" size="17" color="#999"></uni-icons>
				</view>
			</view>
			<!-- <view class="function">
				<view class="left">
					<view class="function_icon">

					</view>
					<view class="function_name">
						艺人
					</view>
				</view>
				<view class="right">
					<uni-icons type="forward" size="17" color="#999"></uni-icons>
				</view>
			</view>
			<view class="function">
				<view class="left">
					<view class="function_icon">

					</view>
					<view class="function_name">
						歌曲
					</view>
				</view>
				<view class="right">
					<uni-icons type="forward" size="17" color="#999"></uni-icons>
				</view>
			</view> -->
			<view class="function" @click="chooseFile">
				<view class="left">
					<view class="function_icon">

					</view>
					<view class="function_name">
						获取本地文件
					</view>
				</view>
				<view class="right">
					<uni-icons type="forward" size="17" color="#999"></uni-icons>
				</view>
			</view>
		</view>
		<view class="Recent_Add">
			<view class="text">
				最近添加
			</view>
			<view class="add_info">
				<view class="music_info" v-for="items in RecentlyAdd" @click="Play(src1)">
					<view class="musicimg">
						<image :src="items.imgUrl" mode="aspectFit"></image>
					</view>
					<view class="musicinfo">
						<view class="musicname">
							{{items.musicname}}
						</view>
						<view class="musicauthor">
							{{items.musicauthor}}
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import request from "../API/musicAPI.js";
	import chooseMusic from "../API/choosefile.js"
	export default {
		created: function() {
			// this.getRecPlay("烟火里的尘埃");
			// this.getRecPlay("失眠飞行");
			// this.getRecPlay("错位时空");
			// this.getRecPlay("起风了");
			// this.getRecPlay("句号");
			// this.getRecPlay("寒鸦少年");
		},
		data() {
			return {
				audio: uni.createInnerAudioContext(),
				RecentlyAdd: [],
			};
		},
		methods: {
			// 获取最近聆听
			async getRecPlay(musicName) {
				let that = this;
				let res = await request(`/search?keywords=${musicName}`, {}, "GET");
				let obj = {};
				obj.imgUrl = res.data.result.songs[0].artists[0].img1v1Url;
				obj.musicname = res.data.result.songs[0].name;
				obj.musicauthor = res.data.result.songs[0].artists[0].name;
				that.RecentlyAdd.unshift(obj);
			},

			// 播放音乐
			Play(src) {
				console.log("=====testClick=====");
				//实例化声音  
				const Audio = uni.createInnerAudioContext();
				// Audio.autoplay = true;
				Audio.src = src; //音频地址  
				Audio.play(); //执行播放  
				Audio.onError((res) => {
					console.log(res.errMsg);
				});
				Audio.onPause(function() {
					console.log('end');
					Audio.destroy();
				});
			},
			// 选择本地文件
			chooseFile() {
				console.log(plus.os.name);
				if (plus.os.name == "Android");
				chooseMusic((res) => {
					console.log('file://' + res);
					let songsrc = 'file://' + res;
					this.audio.src = songsrc;
					console.log(this.audio);
					uni.navigateTo({
						url: "../Music/MusicList/MusicList?songsrc=" + songsrc
					})
				}, "audio/*");
			}
		}
	}
</script>

<style lang="scss">
	.Home_Container {
		width: 750rpx;
		display: flex;
		flex-direction: column;
		align-items: center;
		// background: #000000;

		.Top {
			width: 700rpx;

			// border: 1px solid red;
			.top_text {
				font-size: 30px;
				font-weight: bold;
			}
		}

		.Mulu {
			align-self: flex-end;
			width: 720rpx;
			margin-top: 20rpx;
			border-top: 1px solid #ddd;
			border-bottom: 1px solid #ddd;

			.function {
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 20rpx 0;

				&:first-child {
					border-bottom: 1px solid #ddd;
				}

				&:nth-child(2) {
					border-bottom: 1px solid #ddd;
				}

				&:nth-child(3) {
					border-bottom: 1px solid #ddd;
				}

				.left {
					font-size: 20px;
				}
			}
		}

		.Recent_Add {
			width: 680rpx;
			margin-top: 50rpx;

			.text {
				font-size: 20px;
				font-weight: bold;
			}

			.add_info {
				margin: 10rpx 0;
				// border: 1px solid red;
				display: flex;
				justify-content: space-between;
				flex-wrap: wrap;

				.music_info {
					// border: 1px solid red;
					width: 320rpx;
					display: flex;
					flex-direction: column;
					align-items: center;

					.musicimg {
						// border: 1px solid red;
						width: 320rpx;
						height: 230rpx;

						image {
							// border: 1px solid red;
							width: 320rpx;
							height: 218rpx;
						}
					}

					.musicinfo {
						width: 300rpx;
						display: flex;
						flex-direction: column;
						justify-content: center;
						align-items: center;
						margin-bottom: 20rpx;
						// border: 1px solid red;

						.musicname {
							font-size: 15px;
							font-weight: bold;
						}

						.musicauthor {
							font-size: 13px;
							font-weight: bold;
							color: #aaa;
						}
					}
				}
			}
		}
	}
</style>
