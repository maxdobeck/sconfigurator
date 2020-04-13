<script>
	let cliRef = 'https://wiki.saucelabs.com/display/DOCS/Sauce+Connect+Proxy+Command-Line+Quick+Reference+Guide';
	let user = 'sauce_username';
	let apiKey = 'sauce_access_key';
	let noSslBump = [];
	let directDomains = [];
	let tunnelDomains = [];
	let verbose = false;
	let veryVerbose = false;
	let fastFail = [];
	let name = '';
	let logfile = '';
	let parentProxy = '';
	let parentProxyUser = '';
	let parentProxyPwd = '';
	let pacUrl = '';
	let tunnelThruProxy = false;
	let shared = false;
	let scDc = [
		'https://eu-central-1.saucelabs.com/rest/v1',
		'https://us1.api.testobject.com/sc/rest/v1',
		'https://eu1.api.testobject.com/sc/rest/v1'
	];
	let basicAuthUser = '';
	let basicAuthPwd = '';
	let basicAuthHostPort = '';
	let noAutoDetect = false;
	let dnsServer = [''];

	// OS settings
	let os = 'linux';
</script>

<main>

	<h2>Flags</h2>
	<p class="hint">See <a href={cliRef} target="_blank">Quick Refernce Doc</a> for more info</p>
	
	<label>
		<input type=checkbox bind:checked={verbose} value="verbose">
		Verbose
	</label>

	<h2>Sauce Connect Start Command</h2>
	<div class="cmd-container">
		<button id="copy-cmd-btn" title="copy to clipboard">copy</button>

		{#if os === 'linux' || os === 'mac'}
		<p>/home/jerry/saucelabs/sc-latest/bin/sc </p>
		{:else}
		<p>C:\home\jerry\saucelabs\sc-latest\bin\sc </p>
		{/if}
	</div>
	
	<h5 class="hint">Operating System</h5>
	<div class="os">
		<label>
			<input type=radio bind:group={os} value='linux'>
			Linux
		</label>

		<label>
			<input type=radio bind:group={os} value='windows'>
			Windows
		</label>

		<label>
			<input type=radio bind:group={os} value='mac'>
			Mac
		</label>
	</div>
	
	<p class="hint">
		<a href="https://wiki.saucelabs.com/display/DOCS/Downloading+Sauce+Connect+Proxy" target="_blank">Download</a>
		Sauce Connect for {os}
	</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		font-size: 4em;
		font-weight: 100;
	}

	.cmd-container {
		margin-left: auto;
		margin-right: auto;
	}

	#copy-cmd-btn {
		background-color: transparent;
		margin-left: 40%;
	}

	.hint {
		color:darkgrey;
	}

	.os label {
		width: 10%;
		border: 1px dotted;
		border-color: darkgrey;
		margin-left: auto;
		margin-right: auto;
	}

	@media (max-width: 700px) {	
		.os label {
			width: 40%;
			border: 1px dotted;
			border-color: darkgrey;
			margin-left: auto;
			margin-right: auto;
		}
	}
	@media screen {
		main {
			max-width: none;
		}
	}
</style>