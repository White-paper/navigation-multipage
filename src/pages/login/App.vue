<template>
	<div id="app">
		<m-header></m-header>
		<router-view ref='body'></router-view>
		<foot></foot>
		<x-canvas></x-canvas>
		<centerTips ref='centerTips'>
			<div class="tips-class flex ellipsis">{{centerTips}}</div>
		</centerTips>
	</div>
</template>

<script>
	import MHeader from 'components/header/header'
	import foot from 'components/footer/footer'
	import XCanvas from 'components/canvas/canvas'
	import centerTips from 'base/centerTips/centerTips'
	const header = 65
	const footer = 135
	let margin = 140
	export default {
		data() {
			return {
				centerTips: false
			}
		},
		name: 'app',
		components: {
			MHeader,
			foot,
			XCanvas,
			centerTips
		},
		updated() {
			this._screenAdaptation()
		},
		methods: {
			_screenAdaptation() {
				if (this.$refs.body && !this.$refs.body.$el.style.minHeight) {
					const minheight = window.screen.height - header - footer - margin
					this.$refs.body.$el.style.minHeight = `${minheight}px`
				}
			},
			_open(res) {
				if (this.$refs.centerTips && res) {
					this.centerTips = res
					this.$refs.centerTips._open()
				}
			},
		}
	}
</script>

<style>
@import url(~common/css/reset.css);
@import url(~common/css/public.css);
@import url(~common/css/adaptation.css);
@import url(~common/css/important.css);
@import url(~common/icon/iconfont.css);

</style>