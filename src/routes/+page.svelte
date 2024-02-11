<script lang="ts">
	type player_points = { [player: string]: number };
	type round = {
		[player: string]: { bet: number | undefined; won: number | undefined; points: number };
	};

	let players: string[] = [];
	let points: player_points = {};
	let rounds: round[] = [];

	function new_round(): round {
		let round: round = {};
		players.forEach((player) => {
			round[player] = {
				bet: undefined,
				won: undefined,
				points: 0
			};
		});
		return round;
	}

	let current_round = new_round();

	function get_points(bet: number | undefined, won: number | undefined) {
		if (!bet || !won) return 0;
		if (bet == won) {
			return 10 + 2 * won;
		}
		return -2 * Math.abs(bet - won);
	}

	function evaluate_round() {
		players.forEach((player) => {
			let point = get_points(current_round[player].bet, current_round[player].won);
			points[player] += point;
			current_round[player].points = point;
		});

		rounds = [...rounds, current_round];
		current_round = new_round();
	}

	function reset_scores() {
		rounds = [];
		players.forEach((player) => {
			points[player] = 0;
		});
		current_round = new_round();
	}
	function full_reset() {
		rounds = [];
		players = [];
		current_round = new_round();
	}

	let new_player_name: string;
	function add_player(name: string) {
		players = [...players, name];
		points[name] = 0;
		current_round = new_round();

		new_player_name = '';
	}

	function remove_player(index: number) {
		players.splice(index, 1);
		players = players;
	}
</script>

<form on:submit|preventDefault={() => add_player(new_player_name)} class="w-full max-w-xl">
	<input
		type="text"
		class="input input-bordered input-primary placeholder:opacity-40 w-full"
		placeholder="Add player"
		bind:value={new_player_name}
	/>
</form>
{#if players.length > 0}
	<div class="flex gap-4 w-full max-w-xl flex-wrap">
		<button on:click={evaluate_round} class="btn btn-success grow">Evaluate</button>
		<button on:click={reset_scores} class="btn btn-warning grow">Clear</button>
		<button on:click={full_reset} class="btn btn-error grow">Reset</button>
	</div>

	<div class="flex flex-wrap gap-4 w-full justify-center">
		{#each players as player, i}
			<div class="join join-vertical indicator">
				<div class="flex justify-between gap-4 join-item p-4 bg-base-200 border border-neutral">
					<p class="font-bold">{player}</p>
					<span class="badge badge-info badge-lg">{points[player]}</span>
				</div>
				<button
					on:click={() => remove_player(i)}
					class="indicator-item badge badge-error badge-sm aspect-square">×</button
				>
				<div class="join-item grid grid-cols-2">
					<input
						type="number"
						class="bet input input-bordered"
						placeholder="Bet"
						bind:value={current_round[player].bet}
					/>
					<input
						type="number"
						class="won input input-bordered"
						placeholder="Won"
						bind:value={current_round[player].won}
					/>
				</div>
			</div>
		{/each}
	</div>
{/if}

<style>
	input.bet {
		border-radius: 0 0 0 var(--rounded-btn);
	}
	input.won {
		border-radius: 0 0 var(--rounded-btn) 0;
	}

	input::placeholder {
		opacity: 0.4;
	}
</style>
