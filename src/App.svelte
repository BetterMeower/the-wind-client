<!-- said Moore Swift -->

<script>
	import Main from "./screens/Main.svelte";
	import Setup from "./screens/Setup.svelte";
	
	import Modal from "./lib/Modal.svelte";
	import LoginModal from "./lib/modals/Login.svelte";
	import SignupModal from "./lib/modals/Signup.svelte";
	import BannedModal from "./lib/modals/Banned.svelte";
	import IPBannedModal from "./lib/modals/IPBanned.svelte";
	import DeletePostModal from "./lib/modals/DeletePost.svelte";
	import ReportPostModal from "./lib/modals/ReportPost.svelte";
	import ReportUserModal from "./lib/modals/ReportUser.svelte";
	import GC_MemberModal from "./lib/modals/GC_Member.svelte";
	import AddMemberModal from "./lib/modals/AddMember.svelte";
	import RemoveMemberModal from "./lib/modals/RemoveMember.svelte";
	import CreateChatModal from "./lib/modals/CreateChat.svelte";
	import SetQuoteModal from "./lib/modals/SetQuote.svelte";
	import ChangePasswordModal from "./lib/modals/ChangePassword.svelte";
	import DeleteAccountModal from "./lib/modals/DeleteAccount.svelte";
	import LogoutModal from "./lib/modals/Logout.svelte";
	import ThemeModal from "./lib/modals/Theme.svelte";
	import ErrorModal from "./lib/modals/Error.svelte";

	import Spinner from "./lib/Spinner.svelte";
	import {link} from "./lib/clmanager.js";
	// import {newNotification} from "./lib/notifications.js";

	import {
		screen, setupPage,
		modalShown, modalPage,
		disconnected, disconnectReason,
		user, spinner
	} from "./lib/stores.js";
	import {tick} from "svelte";
    import SetQuote from "./lib/modals/SetQuote.svelte";

	// let notificationRequest = Notification.requestPermission();
</script>

<main
	id="main"
	class:theme-orange={$user.theme==="orange"}
	class:theme-blue={$user.theme==="blue"}
	class:theme-green={$user.theme==="green"}
	class:theme-magenta={$user.theme==="magenta"}
	class:theme-red={$user.theme==="red"}
	class:theme-pink={$user.theme==="pink"}
	class:theme-gray={$user.theme==="gray"}
	class:theme-purple={$user.theme==="purple"}
	class:mode-light={!($user.mode === false)}
	class:mode-dark={$user.mode === false}

	class:layout-old={$user.layout === "old"}
>
	{#if $disconnected}
		<Modal>
			<h2 slot="header">
				Me-owch.
			</h2>
			<div slot="default">
				<p>
					{#if $disconnectReason === ""}
						<!-- {#if !(document.hasFocus())} {
							newNotification("You were disconnected. Click on this notification to reconnect.", "Disconnected")
							};
						{/if} -->
						a sick cat
					{:else if $disconnectReason === "Failed to load userdata"}
						An unexpected error occurred while loading user data. Visit this page for more details.
					{:else if $disconnectReason === "E:119 | IP Blocked"}
						<!-- {#if !(document.hasFocus())} {
							newNotification("Your IP address has been blocked. If reconnecting does not work, try disabling any ad blockers and try again.", "IP Address blocked")
							};
						{/if} -->
						Your server's IP address has been blocked. ({link.ip}).
					{:else if $disconnectReason == "E:110 | ID conflict"}
						<!-- {#if !(document.hasFocus())} {
							newNotification("It looks like you signed in from another device. Log back into this device to continue recieving notifications, or change your password immedietely if this was not you.", "Sign in from another device detected")
							};
						{/if} -->
						There is none! It looks like you are using Peacock on another computer.
						<br /><br />
						Make sure your phone is connected to Meower and try again.
					{:else if $disconnectReason == "E:018 | Account Banned"}
						<!-- {#if !(document.hasFocus())} {
							newNotification("Your account has been banned from Meower. Click this notification to learn more.", "Account Banned")
							};
						{/if} -->
						helpless
					{:else if $disconnectReason == "E:020 | Kicked"}
						<!-- {#if !(document.hasFocus())} {
							newNotification("Your account was kicked by a moderator. Click on this notification to reconnect.", "Kicked")
							};
						{/if} -->
						The player pays
					{:else}
						No se puede conectar de servidor a servidor.
						<pre><code>{$disconnectReason}</code></pre>
					{/if}
				</p>
				<button on:click={async () => {
					screen.set("setup");
					disconnected.set(false);
					await tick();
					setupPage.set("reconnect");
				}}>¿El uno al otro?</button>
			</div>
		</Modal>
	{/if}

	{#if $modalShown}
		{#if $modalPage === "login"}
			<LoginModal />
		{:else if $modalPage === "signup"}
			<SignupModal />
		{:else if $modalPage === "banned"}
			<BannedModal />
		{:else if $modalPage === "ipBanned"}
			<IPBannedModal />
		{:else if $modalPage === "deletePost"}
			<DeletePostModal />
		{:else if $modalPage === "reportPost"}
			<ReportPostModal />
		{:else if $modalPage === "reportUser"}
			<ReportUserModal />
		{:else if $modalPage === "createChat"}
			<CreateChatModal />
		{:else if $modalPage === "setQuote"}
			<SetQuoteModal />
		{:else if $modalPage === "changePassword"}
			<ChangePasswordModal />
		{:else if $modalPage === "deleteAccount"}
			<DeleteAccountModal />
		{:else if $modalPage === "GC_Member"}
			<GC_MemberModal />
		{:else if $modalPage === "AddMember"}
			<AddMemberModal />
		{:else if $modalPage === "removeMember"}
			<RemoveMemberModal />
		{:else if $modalPage === "logout"}
			<LogoutModal />
		{:else if $modalPage === "theme"}
			<ThemeModal />
		{:else}
			<ErrorModal />
		{/if}
	{/if}

	{#if $screen === "setup"}
		<Setup />
	{:else}
		<Main />
	{/if}

	{#if $spinner}
		<div class="spinner-container">
			<Spinner />
		</div>
	{/if}
</main>

<style>
	.spinner-container {
		position: fixed;
		right: 27px;
		bottom: 25px;
		z-index: 10000;
	}

	#main {
		width: 100%;
		height: 100%;

		font-family: Simvoni, sans-serif;

		--orange: #f9a636;
		--orange-button: var(--orange);
		--orange-light: #ffce8c;
		--orange-dark: #b46d34;
		--orange-scrollbar-back: #a15d04;
		--background: white;
		--foreground: black;
		--foreground-orange: white;

		--pfp-bg: white;
		--pfp-outline: #d9d9d9;

		background-color: var(--background);
		color: var(--foreground);
		scrollbar-color: var(--orange) var(--orange-scrollbar-back);
		font-size: 15pt;
	}

	#main.theme-orange.mode-dark {
		--orange: #b35305;
		--orange-light: #dd7f14;
		--orange-dark: #ac4718;
		--orange-scrollbar-back: rgb(131, 45, 5);
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-blue {
		/*--orange: #aa0000;
		--orange-light: #aa0000;
		--orange-dark: #aa0000;
		--orange-scrollbar-back: #aa0000;*/
		--orange: #4d97ff;
		--orange-light: #79b7ff;
		--orange-dark: #3685eb;
		--orange-scrollbar-back: #374eb1;
		--background: white;
		--foreground: black;
		--foreground-orange: white;
		scrollbar-color: unset;
	}
	#main.theme-blue.mode-dark {
		--background: #202020;
		--foreground: white;
		--foreground-orange: white;
	}
	#main.theme-green {
		--orange: #2cbe00;
		--orange-light: #30ce00;
		--orange-dark: #26a300;
		--orange-scrollbar-back: #229200;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-green.mode-dark {
		--orange: #2cbe00;
		--orange-light: #30ce00;
		--orange-dark: #26a300;
		--orange-scrollbar-back: #229200;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-magenta {
		--orange: #a85ff5;
		--orange-light: #b97cfa;
		--orange-dark: #9c59e4;
		--orange-scrollbar-back: #9c59e4;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-magenta.mode-dark {
		--orange: #a85ff5;
		--orange-light: #b97cfa;
		--orange-dark: #9c59e4;
		--orange-scrollbar-back: #9c59e4;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-red {
		--orange: #ff3838;
		--orange-light: #ff6f6f;
		--orange-dark: #e03030;
		--orange-scrollbar-back: #e03030;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-red.mode-dark {
		--orange: #ff3838;
		--orange-light: #ff6f6f;
		--orange-dark: #e03030;
		--orange-scrollbar-back: #e03030;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-pink {
		--orange: #ff8af7;
		--orange-light: #fcaef7;
		--orange-dark: #df79d8;
		--orange-scrollbar-back: #df79d8;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-pink.mode-dark {
		--orange: #ff8af7;
		--orange-light: #fcaef7;
		--orange-dark: #df79d8;
		--orange-scrollbar-back: #df79d8;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-gray {
		--orange: #575656;
		--orange-light: #a0a0a0;
		--orange-dark: #3d3c3c;
		--orange-scrollbar-back: #3d3c3c;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-gray.mode-dark {
		--orange: #575656;
		--orange-light: #a0a0a0;
		--orange-dark: #3d3c3c;
		--orange-scrollbar-back: #3d3c3c;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
	#main.theme-purple {
		--orange: #731bc4;
		--orange-light: #9665c4;
		--orange-dark: #6016a6;
		--orange-scrollbar-back: #6016a6;
		--background: #ffffff;
		--foreground: black;
		--foreground-orange: #eef;
	}
	#main.theme-purple.mode-dark {
		--orange: #731bc4;
		--orange-light: #9665c4;
		--orange-dark: #6016a6;
		--orange-scrollbar-back: #6016a6;
		--background: #202020;
		--foreground: #eef;
		--foreground-orange: #eef;
	}
</style>
