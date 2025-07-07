<script>

	let {
		style = "",
		gradientColor = null,
		gradientWidth = null,
		scrollLeft = $bindable(0),
		children = null,
	} = $props();
	

	$effect(() => {
		if (scrollLeft > marqueeWidth) {
			scrollContainer.scrollTo({
				left: scrollContainer.scrollLeft,
				behavior: "auto",
			});
			scrollLeft = scrollLeft % marqueeWidth;
		}
		
		scrollContainer.scrollTo({
			left: scrollLeft,
			behavior:  "auto",
		});


	});
	
	let scrollContainer;

	let containerWidth;
	let marqueeWidth;
	let gradient = $derived(!!gradientColor || !!gradientWidth);

</script>

<div
	style={style}
	class="marquee-container"
	bind:clientWidth={containerWidth}
	style:--gradientColor={gradientColor}
	style:--gradientWidth={gradientWidth}	
	bind:this={scrollContainer}
>
	{#if gradient}
		<div class="gradient" data-testid="marquee-gradient" ></div>
	{/if}
	<div class="marquee" bind:clientWidth={marqueeWidth} data-testid="marquee-slot"
	
	>
		
		{@render children?.()}
	</div>
</div>

<style>
	.marquee-container {
		display: flex;
		width: 100%;
		overflow-x: hidden;
		flex-direction: row;
		position: relative;
		height: 100%;
	}

	.marquee-container:hover .marquee {
		animation-play-state: var(--pause-on-hover);
	}

	.marquee-container:active .marquee {
		animation-play-state: var(--pause-on-click);
	}

	.marquee {
		flex: 0 0 auto;
		min-width: 100%;
		z-index: 1;
		display: flex;
		flex-direction: row;
		align-items: center;
	}



	.initial-child-container {
		flex: 0 0 auto;
		display: flex;
		min-width: auto;
		flex-direction: row;
	}

	.gradient::after,
	.gradient::before {
		background: linear-gradient(
			to right,
			var(--gradientColor, white),
			transparent
		);
		content: "";
		height: 100%;
		position: absolute;
		width: var(--gradientWidth, 10%);
		z-index: 2;
	}

	.gradient::before {
		left: 0;
		top: 0;
	}

	.gradient::after {
		right: 0;
		top: 0;
		transform: rotateZ(180deg);
	}
</style>