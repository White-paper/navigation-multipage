<template>
	<div id="app">
		<m-header></m-header>
		<router-view></router-view>
		<foot></foot>
		<centerTips ref='centerTips'>
			<div class="tips-class flex ellipsis">{{centerTips}}</div>
		</centerTips>
	</div>
</template>

<script>
	// require('common/css/reset.css')
	import MHeader from 'components/header/header'
	import foot from 'components/footer/footer'
	import centerTips from 'base/centerTips/centerTips'
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
			centerTips
		},
		methods: {
			_open(res) {
				if (this.$refs.centerTips && res) {
					this.centerTips = res
					this.$refs.centerTips._open()
				}
			}
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