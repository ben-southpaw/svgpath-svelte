<script>
	import { onMount } from 'svelte';
	import gsap from 'gsap';

	let textPathElement;
	let computedWidth;

	let svgDoc;

	async function handleSvgLoad(event) {
		console.log('SVG loaded');
		svgDoc = event.target.contentDocument;
		if (!svgDoc) {
			console.error('No SVG document found');
			return;
		}

		// Find the SVG element and the path
		const svgElement = svgDoc.querySelector('svg');
		const path = svgDoc.querySelector('path');
		if (!path || !svgElement) {
			console.error('Required elements not found');
			return;
		}

		// Set ID for the path and make it visible with a unique color
		path.id = 'svgPath';
		path.style.stroke = '#FF0000'; // Bright red for visibility
		path.style.strokeWidth = '2';
		path.style.fill = 'none';

		// Split the path into sections and color them uniquely
		const colors = [
			'#FF0000', // Red
			'#00FF00', // Green
			'#0000FF', // Blue
			'#FF00FF', // Magenta
			'#00FFFF', // Cyan
			'#FFA500', // Orange
			'#800080', // Purple
			'#008000', // Dark Green
			'#FFC0CB', // Pink
			'#FFD700'  // Gold
		];

		// Create multiple paths from the original
		const pathData = path.getAttribute('d');
		const pathSegments = pathData.split('M').filter(Boolean);
		
		pathSegments.forEach((segment, index) => {
			const newPath = svgDoc.createElementNS('http://www.w3.org/2000/svg', 'path');
			newPath.setAttribute('d', 'M' + segment);
			newPath.style.stroke = colors[index % colors.length];
			newPath.style.strokeWidth = '2';
			newPath.style.fill = 'none';
			path.parentNode.appendChild(newPath);
		});

		// Hide original path
		path.style.stroke = 'none';

		// Find and adjust any rectangles to be full size
		const rect = svgDoc.querySelector('rect');
		if (rect) {
			rect.setAttribute('width', '100%');
			rect.setAttribute('height', '100%');
			rect.setAttribute('x', '0');
			rect.setAttribute('y', '0');
		}

		// Wait a moment for SVG to be fully loaded
		await new Promise(resolve => setTimeout(resolve, 100));

		// Get and log all relevant dimensions
		const bbox = svgElement.getBBox();
		const viewBox = svgElement.getAttribute('viewBox');
		const clientRect = svgElement.getBoundingClientRect();

		console.log('Original bbox:', bbox);
		console.log('Original viewBox:', viewBox);
		console.log('Client rect:', clientRect);
		console.log('Window dimensions:', window.innerWidth, window.innerHeight);

		// Lock SVG dimensions
		const containerWidth = window.innerWidth;
		const containerHeight = window.innerHeight;
		
		// Remove any existing attributes that might cause auto-scaling
		svgElement.removeAttribute('width');
		svgElement.removeAttribute('height');
		svgElement.removeAttribute('preserveAspectRatio');
		
		// Set fixed dimensions
		svgElement.setAttribute('width', containerWidth);
		svgElement.setAttribute('viewBox', '0 0 1440 812');
		svgElement.style.width = containerWidth + 'px';
		svgElement.style.maxWidth = 'none';
		svgElement.style.position = 'absolute';
		svgElement.style.left = '0';
		svgElement.style.top = '50%';
		svgElement.style.transform = 'translateY(-50%)';
		
		// Calculate and set height to maintain aspect ratio
		const aspectRatio = 812 / 1440; // viewBox height / width
		const height = containerWidth * aspectRatio;
		svgElement.style.height = height + 'px';

		// Add font definition to SVG
		const defs = svgDoc.createElementNS('http://www.w3.org/2000/svg', 'defs');
		const style = svgDoc.createElementNS('http://www.w3.org/2000/svg', 'style');
		style.textContent = `
			@font-face {
				font-family: 'ApercuMono';
				src: url('/assets/ApercuMono.ttf') format('truetype');
			}
		`;
		defs.appendChild(style);
		svgElement.insertBefore(defs, svgElement.firstChild);

		// Create text element with textPath
		const text = svgDoc.createElementNS('http://www.w3.org/2000/svg', 'text');
		const textPath = svgDoc.createElementNS('http://www.w3.org/2000/svg', 'textPath');
		textPath.setAttributeNS(null, 'href', '#svgPath');
		textPath.setAttributeNS(null, 'style', 'fill: black; font-size: 1rem; letter-spacing: 0.02em; word-spacing: 0.2em; font-family: ApercuMono;');
		textPath.textContent = repeatedText + repeatedText + repeatedText;

		// Add elements to SVG
		text.appendChild(textPath);
		svgElement.appendChild(text);

		// Store reference
		textPathElement = textPath;

		// Start animation with a small delay to ensure font is loaded
		setTimeout(animateTextPath, 100);
	}

	// Create text content with variations
	const baseText =
		'Fatema has the mind of a celestial being and the heart of a seven year old. She will say I love you on the second date and she will mean it. Every year I hope Fatema is my secret santa. In middle school Fatema was the goat. She is like if a personality hire was overqualified. I would trust Fatema to cut my bangs. Being on her close friends story is like watching Michelangelo paint the Sistine Chapel. I’ve never met anyone who cares more about whales. We call her FatGPT.  ';
	const repeatedText = Array(4).fill(baseText).join('');

	// No longer need font loading here as it's handled in the SVG

	function animateTextPath() {
		console.log('Starting animation');

		// Wait for text path to be ready
		if (!textPathElement) {
			console.error('Text path element not found');
			return;
		}

		// Get the path
		const path = svgDoc.getElementById('svgPath');
		if (!path) {
			console.error('Path not found');
			return;
		}

		// Set initial position
		textPathElement.setAttribute('startOffset', '0');
		textPathElement.style.opacity = '1';

		// Create timeline for smooth animation
		gsap.to(textPathElement, {
			duration: 60,
			attr: { startOffset: -3000 }, // Use a fixed offset
			ease: 'none',
			repeat: -1,
			repeatDelay: 0
		});

		console.log('Animation started');
	}

	// No longer need onMount handler for font loading
</script>

<div class="container">
	<object
		data="/assets/pathwithimages.svg"
		type="image/svg+xml"
		class="svg-object"
		on:load={handleSvgLoad}
	/>
</div>

<style>
	.container {
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		position: relative;
		background: white;
		display: block;
		margin: 0;
		padding: 0;
		min-width: 100vw;
		max-width: 100vw;
	}

	.svg-object {
		width: 100vw !important;
		position: absolute;
		left: 0;
		margin: 0;
		padding: 0;
		display: block;
		max-width: none !important;
		top: 50%;
		transform: translateY(-50%) !important;
	}

	.text-overlay {
		width: 100%;
		height: 100%;
		position: absolute;
		top: 0;
		left: 0;
		overflow: visible;
		z-index: 10;
	}

	.text-overlay path {
		fill: none;
		stroke: none;
	}

	

	textPath {
		opacity: 0;
	}
</style>
