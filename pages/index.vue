<template>
	<div id="app">
		<div class="sections-menu">
			<div
				class=""
				v-on:click="scrollToSection(index)"
				v-for="(offset, index) in offsets"
				v-bind:key="index"
			>
				<span
					class="menu-point"
					v-bind:class="{ active: activeSection == index }"
				></span>
				<span class="label" v-if="!activeSection">
					{{ labels[index] }}
				</span>
			</div>
		</div>
		<section class="sec1 fullpage white">
			<img src="nahrain-logo.png" class="logo" />
			<img src="/shoes/sec1.png" class="shoe" />
			<div class="rotated-rect gray"></div>
			<img src="/shoes/sec1.png" class="shoe" />
		</section>
		<section class="sec2 fullpage gray">
			<div class="rect white"></div>
			<img src="/shoes/sec2.png" class="shoe" />
		</section>
		<section class="sec3 fullpage ">
			<div class="rect gray"></div>
			<img src="/shoes/sec3.png" class="shoe" />
		</section>
	</div>
</template>

<script>
export default {
	data() {
		return {
			inMove: false,
			activeSection: 0,
			offsets: [],
			labels: [],
			touchStartY: 0,
		};
	},
	computed: {},
	methods: {
		calculateSectionOffsets() {
			let sections = document.getElementsByTagName("section");
			let length = document.getElementsByTagName("section").length;

			for (let i = 0; i < length; i++) {
				let sectionOffset = sections[i].offsetTop;
				this.offsets.push(sectionOffset);
			}
			this.labels = ["معرفی کوتاه", "بخش دوم"];
		},
		//todo
		handleMouseWheel: function (e) {
			if (!((this.activeSection + 1) > (this.offsets.length - 1))  && e.wheelDelta < 30 && !this.inMove) {
				this.moveUp();
			} else if (!((this.activeSection - 1) < 0) && e.wheelDelta > 30 && !this.inMove) {
				this.moveDown();
			}

			e.preventDefault();
			return false;
		},
		//todo
		handleMouseWheelDOM: function (e) {
			if (!((this.activeSection + 1) > (this.offsets.length - 1))  && e.detail > 0 && !this.inMove) {
				this.moveUp();
			} else if (!((this.activeSection - 1) < 0) && e.detail < 0 && !this.inMove) {
				this.moveDown();
			}

			return false;
		},
		moveDown() {
			this.inMove = true;
			this.activeSection--;

			if (this.activeSection < 0)
				this.activeSection = this.offsets.length - 1;

			this.scrollToSection(this.activeSection, true);
		},
		moveUp() {
			this.inMove = true;
			this.activeSection++;

			if (this.activeSection > this.offsets.length - 1)
				this.activeSection = 0;

			this.scrollToSection(this.activeSection, true);
		},
		scrollToSection(id, force = false) {
			if (this.inMove && !force) return false;

			this.activeSection = id;
			this.inMove = true;

			document
				.getElementsByTagName("section")
				[id].scrollIntoView({ behavior: "smooth" });

			setTimeout(() => {
				this.inMove = false;
			}, 400);
		},
		//todo
		touchStart(e) {
			e.preventDefault();

			this.touchStartY = e.touches[0].clientY;
		},
		//todo
		touchMove(e) {
			if (this.inMove) return false;
			e.preventDefault();

			const currentY = e.touches[0].clientY;

			if (this.touchStartY < currentY) {
				this.moveDown();
			} else {
				this.moveUp();
			}

			this.touchStartY = 0;
			return false;
		},
	},
	mounted() {
		this.calculateSectionOffsets();

		window.addEventListener("DOMMouseScroll", this.handleMouseWheelDOM); // Mozilla Firefox
		window.addEventListener("mousewheel", this.handleMouseWheel, {
			passive: false,
		}); // Other browsers

		window.addEventListener("touchstart", this.touchStart, {
			passive: false,
		}); // mobile devices
		window.addEventListener("touchmove", this.touchMove, {
			passive: false,
		}); // mobile devices
	},
	destroyed() {
		window.removeEventListener("mousewheel", this.handleMouseWheel, {
			passive: false,
		}); // Other browsers
		window.removeEventListener("DOMMouseScroll", this.handleMouseWheelDOM); // Mozilla Firefox

		window.removeEventListener("touchstart", this.touchStart); // mobile devices
		window.removeEventListener("touchmove", this.touchMove); // mobile devices
	},
};
</script>

<style>
.rotated-rect {
	display: inline-block;
	position: absolute;
	top: 65%;
	left: -30%;
	width: 150%;
	height: 100vh;
	transform: rotate(15deg);
}
.gray {
	background-color: gray;
}
.sec1{
	position: relative;
	float: left;
	margin-right: 60%;
}
.sec1 img.shoe {
	top: 30%;
	left: -5%;
	position: absolute;
	width: 90vh;
	transform: rotate(-90deg);
}
.sec1 img.shoe:nth-child(2) {
	transform: scaleX(-1) rotate(-90deg);
	left: 18%;
	/* z-index: -3; */
}
.sec1 img.logo {
	position: absolute;
	top: 5%;
	right: 5%;
	width: 20%;
}

.sec2{
	position: relative;
}
.sec2 .rect {
	display: inline-block;
	position: absolute;
	top: 65%;
	left: -30%;
	width: 150%;
	height: 100vh;
}
.sec2 img.shoe{
	position: absolute;
	top: 30%;
	right: 15%;
	height: 50vh;
}

.sec3{
	position: relative;
}
.sec3 .rect {
	display: inline-block;
	position: absolute;
	top: 65%;
	left: -30%;
	width: 150%;
	height: 100vh;
}
.sec3 img.shoe{
	position: absolute;
	top: 30%;
	left: 10%;
	height: 50vh;
	transform: scaleX(-1) ;
}

</style>
