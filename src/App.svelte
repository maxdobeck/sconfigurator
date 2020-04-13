<script>
	import { fade } from 'svelte/transition';

	let cliRef = 'https://wiki.saucelabs.com/display/DOCS/Sauce+Connect+Proxy+Command-Line+Quick+Reference+Guide';
	let command = 'sc';
	// sauce
	let user = '-u sauce_username';
	let apiKey = '-k sauce_access_key ';
	let name = '';
	let shared = false;
	// region
	let scDc = new Map();
	scDc.set('US-WEST', 'https://us-west-1.saucelabs.com/rest/v1 ');
	scDc.set('EU', 'https://eu-central-1.saucelabs.com/rest/v1 ');
	scDc.set('US-EAST', 'https://us-east-1.saucelabs.com/rest/v1 ');
	scDc.set('RDC EU', 'https://eu1.api.testobject.com/sc/rest/v1 ');
	scDc.set('RDC US', 'https://us1.api.testobject.com/sc/rest/v1 ');
	let dc_choice = 'US-WEST';
	// ssl config
	let noSslBump = [];
	// domain based routing + config
	let directDomains = [];
	let tunnelDomains = [];
	let fastFail = [];
	let basicAuthUser = '';
	let basicAuthPwd = '';
	let basicAuthHostPort = '';
	// logging
	let verbosity = 'normal';
	let logfile = '';
	// network config
	let parentProxy = '';
	let parentProxyUser = '';
	let parentProxyPwd = '';
	let pacUrl = '';
	let tunnelThruProxy = false;
	let noAutoDetect = false;
	let dnsServer = [''];
	// UI: OS settings
	let os = 'linux';

</script>

<main>

    <h2>Flags</h2>
    <p class="hint">See <a href={cliRef} target="_blank">Quick Refernce Doc</a> for more info</p>
    <div class="flag-container">
		<!-- Logging -->
		<div class="flag-section">
			<p class="hint">Logging</p>
			<label class="flags">
				<input type=radio bind:group={verbosity} value="normal">
				Normal
			</label>

			<label class="flags">
				<input type=radio bind:group={verbosity} value="verbose">
				Verbose
			</label>

			<label class="flags">
				<input type=radio bind:group={verbosity} value="veryVerbose">
				Very Verbose
			</label>

			<label class="flags">
				Log file path
				<input type="text" bind:value={logfile} placeholder="logfile">
			</label>
		</div>

		<!-- Sauce Stuff -->
		<div class="flag-section">
			<p class="hint">Sauce Stuff</p>
			<label class="flags">
				Tunnel Name
				<input type=text bind:value={name} placeholder="tunnel name">
			</label>

			<label class="flags">
				<input type=checkbox bind:checked={shared} value="false">
				Shared
			</label>
		</div>

		<!-- Region -->
		<div class="flag-section" id="region">
			<p class="hint">Region</p>
			{#each [...scDc.keys()] as datacenter}
				<label class="flags">
					<input type="radio" bind:group={dc_choice} value="{datacenter}">
					{datacenter}
				</label>
			{/each}
		</div>
	</div>

	<h2>Sauce Connect Start Command</h2>
	<div class="cmd-container">
			<code>{command} {user} {apiKey}</code>
			{#if name.length > 0}
				<code>-i {name}</code> 
			{/if}
			
			{#if shared}
				<code in:fade="{{delay:600, duration: 1500}}" out:fade="{{duration: 800}}">-s</code> 
			{/if}
			
			{#if verbosity === 'verbose'} 
				<code in:fade="{{delay:600, duration: 1500}}" out:fade="{{duration: 800}}">-v</code>
			{/if}
			
			{#if verbosity === 'veryVerbose'} 
				<code in:fade="{{delay:600, duration: 1500}}" out:fade="{{duration: 800}}">-vv</code> 
			{/if}
			
			{#if logfile}
				<code in:fade="{{delay:600, duration: 1500}}" out:fade="{{duration: 800}}">-l {logfile}</code>	
			{/if}
			<code transition:blur={{duration:800}}>-x {scDc.get(dc_choice)}</code>
	</div>

	<div id="copy-container">
		<button id="copy-cmd-btn" title="copy to clipboard">copy</button>
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
	<pre>
		{#if os === 'linux' || os === 'mac'}
		<code>/home/you/Downloads/sc-latest-ver/bin/sc</code>
		{:else}
		<code>C:\you\Downloads\sc-latest-ver\bin\sc.exe</code>
		{/if}
	</pre>

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

	.flag-section {
		display: inline-block;
		border: 1px solid;
		border-color: darkgrey;
	}

	.flag-section label {
		padding-left: 10px;
		padding-right: 10px;
	}

	#region {
		padding-bottom: 12px;
		padding-top: 7px;
	}

	#region label {
		padding-left: 12px;
		padding-right: 12px;
	}
	
	.flags {
		display: inline-block;
	}

	.cmd-container {
		margin-left: auto;
		margin-right: auto;
		display: inline-block;
	}

	code {
		font-size: 1.5em;;
	}

	#copy-container {
		padding-top: 50px;
	}

	#copy-cmd-btn {
		background-color: transparent;
		color: darkgrey;
	}

	#copy-cmd-btn:hover {
		color:red;
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
		padding-top: 10px;
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