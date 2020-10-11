<script>
	export let message;

</script>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}
	.container {
		width: calc(100% - 4px);
		margin: 0 2px;
		border: 1px solid grey;
		border-radius: 20px;

		user-select: none;
		word-wrap: break-word;

		margin-bottom: 3px;
	}

	.time {
		font-size: 10px;
		padding: 5px 10px;
	}
	.content {
		width: calc(100% - 10px);
		margin: 0 10px;
	}

	.username {
		font-size: 22px;
	}
	.message {
		display: block;
		padding-right: 10px;
	}
</style>

<div class="container">
	<div class="content">
		<div class="username">
			{message.username}
		</div>
		<div class="message">
			{message.content}
		</div>
	</div>	
	<div class="time">
		{message.time}
	</div>
</div>