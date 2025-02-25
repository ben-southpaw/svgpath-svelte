<script>
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let copy = `Fatema has the mind of a celestial being and the heart of a seven year old. She will say I love you on the second date and she will mean it. Every year I hope Fatema is my secret santa. In middle school Fatema was the goat. She is like if a personality hire was overqualified. I would trust Fatema to cut my bangs. Being on her close friends story is like watching Michelangelo paint the Sistine Chapel. I've never met anyone who cares more about whales. We call her FatGPT.  `;
	let repeatedCopy = copy;

	onMount(async () => {
		// Add more copies and extra spaces between them for smoother coverage
		repeatedCopy = `${copy} ${copy} ${copy} ${copy} ${copy} ${copy} ${copy}`;

		// Create two identical text paths for seamless looping
		const createTimeline = () => {
			const tl = gsap.timeline();
			tl.fromTo(
				'#mainTextPath',
				{ attr: { startOffset: '-50%' } },
				{ ease: 'none', duration: 100, attr: { startOffset: '50%' } },
				0
			);
			return tl;
		};

		// Create master timeline
		const masterTl = gsap.timeline({
			repeat: -1,
			onRepeat: () => {
				// Smoothly reset to start position
				gsap.set('#mainTextPath', { attr: { startOffset: '-50%' } });
			},
		});

		// Add the animation
		masterTl.add(createTimeline());
	});
</script>

<div class="container">
	<img src="/assets/left.png" alt="Left hand" class="hand left-hand" />
	<img src="/assets/right.png" alt="Right hand" class="hand right-hand" />
	<svg
		width="800"
		height="340"
		viewBox="0 0 1062 454"
		fill="none"
		xmlns="http://www.w3.org/2000/svg"
	>
		<!-- Combined Path -->
		<path
			class="path-section"
			id="combined-path"
			d="M911.766 300.926C811.688 364.583 656.67 430.546 435.718 449.043C163.442 471.836 63.3713 405.942 49.9973 381.295C39.3929 361.753 44.1723 347.421 44.1723 347.421C51.3725 325.83 82.6843 318.302 88.4144 316.924C101.371 313.81 112.488 314.455 119.391 315.36 M205.5 358.126C235.778 357.794 275.801 356.706 334.915 352.769C334.915 352.769 784.291 322.84 920.807 164.031C929.067 154.422 936.809 143.125 933.288 134.887C930.008 127.215 917.947 124.512 900.143 124.845 M840.894 131.62C798.899 138.522 748.772 148.939 703.867 154.371C617.135 164.864 514.963 173.901 359 120.32 M308.5 104.5C263.617 103.313 246.174 112.517 243.078 120.32C227.749 158.959 391.224 273.701 565.202 325.203C699.515 364.963 793.898 353.358 806.076 332.223C812.052 321.849 810.167 312.913 810.167 312.913C806.841 297.153 786.121 290.047 781.482 288.557 M717.5 257.518C501.512 156.495 358.352 87.7084 290 66 M36 68.5C41.56 71.9215 42.2947 72.9903 46.5 75 M823.5 181.5C837.086 178.345 847.113 177.23 860 173.854 M135.04 20.8871C117.988 14.9878 103.428 10.9689 92.67 8.28206C47.1509 -3.08526 30.76 -0.327471 19.7641 3.98928C4.70627 9.90048 1.7291 17.9638 1.16367 19.6642C0.0601073 22.9831 0.0907751 26.2041 0.504311 28.8833 M217.5 42.8521C213.326 41.5789 211.638 40.8839 207 39.5 M168 156.5C175.025 161.373 179.747 164.354 186.773 169.227 M922.5 155C941.866 149.016 955.254 144.325 972.56 138.217 M268.134 204.915C366.841 219.864 499.826 229.708 654.812 211.755C698.875 206.651 730.388 199.783 769.5 191.766 M1030.83 124.485C1039.12 124.98 1053.01 127.136 1059.17 137.089C1059.17 137.089 1065.94 148.027 1057.6 168.6C1053.35 179.078 1026.69 214.405 973.688 256.814"
			stroke="black"
			stroke-width="2"
			stroke-miterlimit="10"
		/>

		<text
			id="mainText"
			fill="black"
			style="font-size: 16px; letter-spacing: 2px; word-spacing: 0.2em; font-family: MaisonMono-Medium, helvetica, sans-serif;"
			dy="0"
		>
			<textPath
				id="mainTextPath"
				xlink:href="#combined-path"
				startOffset="-50%"
				method="align"
				spacing="auto">{repeatedCopy}</textPath
			>
		</text>
	</svg>
</div>

<style lang="scss">
	.container {
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		position: relative;
		background: wheat;
		display: block;
		margin: 0;
		padding: 0;

		.hand {
			position: absolute;
			height: auto;
			z-index: 1;
		}

		.left-hand {
			width: 40vw; /* 797.72px/1440px */
			height: 100%; /* 421.1px/812px */
			left: 0; /* -148px/1440px */
			top: -2vh; /* 401.5px/812px */
			transform: rotate(9.22deg);
		}

		.right-hand {
			width: 32vw;
			right: 1%; /* 92px/1440px */
			bottom: 0;
		}

		svg {
			position: absolute;
			overflow: visible;
			z-index: 2;
			width: 79.75vw; /* 1062px/1440px */
			height: 90.5vh; /* 451px/812px */
			left: 10.86%; /* 286px/1440px */
			top: 50%;
			transform: translateY(-55%);
		}
	}
</style>
