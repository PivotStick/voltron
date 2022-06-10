<script>
	import { onMount } from 'svelte';

	let time = 0;
	let duration = 60 * 5;
	let state = 'pause';
	let dragging = false;
	/**
	 * @type {HTMLElement}
	 */
	let line;

	let pos = { x: 0, y: 0 };

	const lerp = (a, b, t) => ({
		x: a.x + (b.x - a.x) * t,
		y: a.y + (b.y - a.y) * t
	});

	const quadCurve = (a, b, c, t) => {
		const p0 = lerp(a, b, t);
		const p1 = lerp(b, c, t);
		return lerp(p0, p1, t);
	};

	const handleClick = () => {
		switch (state) {
			case 'pause':
				state = 'playing';
				break;
			case 'playing':
				state = 'pause';
				break;
			case 'finished':
				time = 0;
				state = 'playing';
				break;

			default:
				break;
		}
	};

	onMount(() => {
		const interval = setInterval(() => {
			if (state === 'playing' && !dragging) {
				time += 1;
			}
		}, 10);

		return () => clearInterval(interval);
	});

	const setTime = (e) => {
		if (!dragging) return;
		const rect = line.getBoundingClientRect();
		const left = e.clientX - rect.left;
		const right = rect.right - rect.left;
		time = Math.max(Math.min(left / right, 1), 0) * duration;
	};

	$: percent = time / duration;
	$: state = percent >= 1 ? 'finished' : state === 'finished' ? 'pause' : state;

	$: pos = quadCurve(
		{
			x: 200,
			y: -16 * 2
		},
		{
			x: 200,
			y: 200
		},
		{
			x: 600,
			y: 200
		},
		percent
	);
</script>

<svelte:window on:mouseup={() => (dragging = false)} on:mousemove={setTime} />

<div class="frame">
	<div class="object" style="--x: {pos.x}px; --y: {pos.y}px" />
	<div class="controls">
		<button class={state} on:click={handleClick} />
		<div
			class="line"
			style="--percent: {percent}"
			bind:this={line}
			on:mousedown={(e) => {
				dragging = true;
				setTime(e);
			}}
		>
			<div class="handle" />
		</div>
	</div>
</div>

<style lang="scss">
	.frame {
		position: relative;
		font-size: 1rem;
		border: 1px #eee solid;
		overflow: hidden;
		max-width: 50em;
		width: 100%;
		margin: 0 auto;
		border-radius: 0.5em;
		background-color: white;
		aspect-ratio: 16 / 9;

		&:active {
			cursor: grabbing;
			.controls {
				.handle {
					cursor: grabbing;
				}
			}
		}
	}

	.object {
		position: absolute;

		width: 6em;
		height: 6em;
		background-color: #333;
		border-radius: 35%;
		transform: translate(-50%, -50%);
		z-index: 1;

		top: var(--y);
		left: var(--x);
	}

	.controls {
		border-top: inherit;
		padding: 0.75em 1em;
		background-color: inherit;
		z-index: 10;

		display: flex;
		gap: 1em;
		align-items: center;
		position: absolute;
		left: 0;
		bottom: 0;
		right: 0;

		.line {
			position: relative;

			background-color: rgba(black, 0.15);
			height: 0.25em;
			width: 100%;
			border-radius: 1em;
			-webkit-user-drag: none;

			// transition-property: font-size;

			&::before {
				content: '';
				position: absolute;

				left: 0;
				bottom: 0;
				top: 0;
				width: calc(var(--percent) * 100%);
				background-color: #aaa;
				border-radius: inherit;
			}

			.handle {
				cursor: grab;
				position: absolute;
				top: 50%;
				left: calc(var(--percent) * 100%);
				width: 0.5em;
				height: 0.5em;
				border-radius: 50%;
				background-color: #aaa;

				transform: translate(-50%, -50%) scale(var(--scale, 1));
				// transition-property: transform;

				&:hover {
					--scale: 1.25;
				}

				&:active {
					cursor: grabbing;
				}
			}

			&:hover {
				font-size: 1.5em;
			}
		}

		button {
			cursor: pointer;

			border: none;
			opacity: 0.15;

			background: no-repeat 50% 50% url('');
			width: 2em;
			height: 2em;

			// transition-property: transform, opacity;

			&.pause {
				background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' class='h-6 w-6' fill='none' viewBox='0 0 24 24' stroke='currentColor' stroke-width='2'%3e%3cpath stroke-linecap='round' stroke-linejoin='round' d='M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z' /%3e%3cpath stroke-linecap='round' stroke-linejoin='round' d='M21 12a9 9 0 11-18 0 9 9 0 0118 0z' /%3e%3c/svg%3e");
			}

			&.playing {
				background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' class='h-6 w-6' fill='none' viewBox='0 0 24 24' stroke='currentColor' stroke-width='2'%3e%3cpath stroke-linecap='round' stroke-linejoin='round' d='M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z' /%3e%3c/svg%3e");
			}

			&.finished {
				background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' class='h-6 w-6' fill='none' viewBox='0 0 24 24' stroke='currentColor' stroke-width='2'%3e%3cpath stroke-linecap='round' stroke-linejoin='round' d='M12.066 11.2a1 1 0 000 1.6l5.334 4A1 1 0 0019 16V8a1 1 0 00-1.6-.8l-5.333 4zM4.066 11.2a1 1 0 000 1.6l5.334 4A1 1 0 0011 16V8a1 1 0 00-1.6-.8l-5.334 4z' /%3e%3c/svg%3e");
			}

			&:hover {
				opacity: 0.5;
				transform: scale(1.1);
			}

			&:active {
				transform: scale(0.9);
			}
		}
	}
</style>
