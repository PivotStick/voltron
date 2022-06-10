<script>
	let members = [
		{
			fullName: 'Maxime Di Natale',
			avatar: 'https://api.oddiris.com/628ded89b8bdefafce87f1bc.jpeg'
		},
		{
			fullName: 'Guillaume Cirillo',
			avatar: 'https://api.oddiris.com/628defa9b8bdefafce87f1bf.jpeg'
		}
	];
</script>

<div class="members">
	{#each members as member}
		<div class="member">
			<img src={member.avatar} alt="" />
			<p>{member.fullName}</p>
		</div>
	{/each}
</div>

<style lang="scss">
	.members {
		display: flex;
		justify-content: center;
		gap: 2em;
		margin-top: 2em;
	}

	.member {
		text-align: center;

		img {
			$size: 5em;
			width: $size;
			height: $size;

			object-fit: cover;
			border-radius: 0.25em;
		}
	}
</style>
