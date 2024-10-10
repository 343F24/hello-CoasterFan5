<script lang="ts">
	import { writable } from 'svelte/store';
	import toast from 'svelte-french-toast';
	const score = writable(0);

	let clicksPerClick = 1;

	const increaseScore = () => {
		$score += clicksPerClick;
	};

	type Upgrade = {
		name: string;
		cost: number;
		costIncrease: number;
		desc: string;
		func: () => void;
	};

	let upgrades: Upgrade[] = [
		{
			name: 'Cursor',
			cost: 25,
			costIncrease: 1.1,
			desc: '+1 Click per Click',
			func: () => {
				clicksPerClick += 1;
			}
		}
	];

	const tryUpgrade = (upgrade: Upgrade) => {
		if ($score >= upgrade.cost) {
			$score -= upgrade.cost;
			upgrade.cost *= upgrade.costIncrease;
			upgrade.func();
			toast.success('Purchased upgrade!');
			upgrades = [...upgrades];
		} else {
			toast.error("Can't afford that");
		}
	};
</script>

<div class="wrap">
	<div class="sidebar">
		{#each upgrades as upgrade}
			<button class="upgrade" on:click={() => tryUpgrade(upgrade)}>
				<span class="name">
					{upgrade.name} - {Math.ceil(upgrade.cost)}
				</span>
				<span class="desc">
					{upgrade.desc}
				</span>
			</button>
		{/each}
	</div>
	<button class="content" on:click={increaseScore}>
		<h2>Click!</h2>
		<p>Score: {Math.floor($score)}</p>
	</button>
</div>

<style lang="scss">
	.wrap {
		width: 100%;
		height: 100%;

		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: row;
	}

	.sidebar {
		height: 100%;
		width: 15rem;
		background: rgba(0, 0, 0, 0.1);
		padding: 1rem;
	}

	.content {
		all: unset;
		cursor: pointer;
		box-sizing: border-box;
		padding: 1rem;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
	}

	.upgrade {
		all: unset;
		box-sizing: border-box;
		cursor: pointer;
		display: flex;
		flex-direction: column;
		width: 100%;
		background: rgba(0, 0, 0, 0.1);
		border-radius: 0.25rem;
		padding: 0.5rem;
	}
</style>
