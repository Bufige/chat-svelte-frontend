<script>
	import { onMount, onDestroy } from "svelte";
	import io from "socket.io-client";

	import Message from "./Message.svelte";

	// "https://bufige-chat-server.herokuapp.com"
	const socket = io('https://bufige-chat-server.herokuapp.com');

	let inputUser;
	let inputMessage;

	let user = "";
	let inputUserValue;
	let messages = [];
	let message = "";

	socket.on("message", (data) => {
		messages = [...messages, data];
	});

	socket.on('onLoad', (data) => {
		messages = [...data];
		console.log('onLoad:', data);
	});

	onMount( () => {
		inputUser.focus();
	})
	onDestroy(async () => {
		socket.disconnect();
	});

	const sendMessage = () => {
		if (message) {
			socket.emit("add-message", generateMessage(message));
		}

		message = null;
	};
	const selectUsername = () => {
		user = inputUserValue;
		
		setTimeout(() => {
			inputMessage.focus();
		}, 500);
		
	}
	const generateMessage = (msg) => {
		return {
			content: msg,
			time: new Date().toLocaleTimeString(),
			username: user
		};
	};
</script>

<style>
	.content {
		height: calc(100% - 45px);
		overflow-y: scroll;
	}
	form {
		position: relative;
		width: 100%;
	}

	form > input {
		width: 100%;

		padding-left: 10px;
		padding-right: 60px;
		border-radius: 20px;

		user-select: none;
		outline: none;
	}

	form > button {
		position: absolute;
		right: 0;
		border-radius: 20px;
		border: 1px solid grey;
	}

	.select {
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;

		font-size: 18px;
	}
</style>

{#if !user}
	<div class="select">
		Select a username
		<form on:submit|preventDefault={selectUsername}>
			<input bind:this={inputUser} bind:value={inputUserValue} placeholder='your username'/>
			<button type='submit'>Select</button>
		</form>
	</div>
{:else}
	<div class="content">
		{#each messages as msg}
			<Message message={msg} />
		{/each}
	</div>

	<form on:submit|preventDefault={sendMessage}>
		<input bind:this={inputMessage} bind:value={message} placeholder='your message'/>
		<button type="submit">Send</button>
	</form>
{/if}
