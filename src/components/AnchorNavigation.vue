<template>
	<div class="sideAnchorNav">
		<div class="left-nav" :class="navText?'navHover':''" @mouseenter="navText=true" @mouseleave="navText=false"></div>
		<ul class="nav-link" @mouseenter="navText=true" @mouseleave="navText=false">
			<li class="nav" :class="index==activeMenu?'anchorNavSelect':''" v-for="(item,index) in allMenuList" :key='index'
			 @click="jump(index)"><a href="javascript:void(0)"><span :class="index==activeMenu||navText?'spanHover':''">{{item}}</span></a></li>
		</ul>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				navText: false,
				scroll: null,
				activeMenu: null,
			}
		},
		props: ['allMenuList'],
		mounted() {
			window.addEventListener('scroll', this.dataScroll);
		},
		watch: {
			scroll: function() {
				this.loadSroll()
			}
		},
		methods: {
			dataScroll: function() {
				this.scroll = document.documentElement.scrollTop || document.body.scrollTop;
			},
			jump(index) {
				this.activeMenu = index
				let jump = document.getElementsByClassName('section');
				// 获取需要滚动的距离
				let total = jump[index].offsetTop;
				document.body.scrollTop = total;
				document.documentElement.scrollTop = total - 70;
				window.pageYOffset = total;
			},
			loadSroll: function() {
				var self = this;
				var navs = document.getElementsByClassName('nav')
				var sections = document.getElementsByClassName('section');
				for (var i = 0; i < sections.length; i++) {
					if (self.scroll >= sections[i].offsetTop - 200) {
						this.activeMenu = i
					}
					if (self.scroll == 0) {
						this.activeMenu = null
					}
				}

			},
		}
	}
</script>

<style lang="scss">
	.sideAnchorNav {
		.left-nav {
			height: 100%;
			width: 500px;
			position: fixed;
			top: 0;
			left: -250px;
			z-index: 10;
		}

		.navHover {
			background: -moz-linear-gradient(to right, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0.2));
			background: -o-linear-gradient(to right, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0.2));
			background: -webkit-linear-gradient(to right, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0.2));
			background: -ms-linear-gradient(to right, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0.2));
			background: linear-gradient(to right, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0.2));
			-webkit-filter: blur(40px);
			filter: blur(40px);
		}

		.nav-link {
			position: fixed;
			left: 3%;
			top: 35%;
			color: white;
			z-index: 20;
		}

		.nav {
			a {
				color: white;
				font-size: 16px;
			}

			opacity: 0.6;
			line-height: 1;

			span {
				opacity: 0;
				-moz-transition: all .5s;
				-webkit-transition: all .5s;
				-o-transition: all .5s;
				-ms-transition: all .5s;
				transition: all 0.5s;
			}

			span:hover {
				text-decoration: underline;

			}

			.spanHover {
				line-height: 3;
				opacity: 1;
			}
		}

		.anchorNavSelect {
			opacity: 1;
			border: none;

			a {
				color: orange;
			}
		}
	}
</style>
