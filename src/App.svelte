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
	let directDomains = '';
	let tunnelDomains = '';
	let fastFail = '';
	let basicAuthUser = '';
	let basicAuthPwd = '*****';
	let basicAuthHostPort = '80';
	// logging
	let verbosity = 'normal';
	let logfile = '';
	// network config
	let parentProxy = '';
	let parentProxyUser = 'username';
	let parentProxyPwd = 'password';
	let pacUrl = '';
	let tunnelThruProxy = false;
	let autoDetectPac = true;
	let dnsServer = '';
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

		<!-- SSL Config -->
		<div class="flag-section">
			<p class="hint">SSL Config</p>
			<p class="hint">Read about SSL Bumping for more info</p>
			<label class="flags">
				No Bump Domains
				<input id="csv-input" type=text bind:value={noSslBump} placeholder="Comma separated list of domains that should NOT be bumped">
			</label>
		</div>

		<!-- Domain Routing & Config -->
		<div class="flag-section">
			<p class="hint">Domain Routing & Config</p>
			<label class="flags">
				Direct Domains
				<input id="csv-input" type=text bind:value={directDomains} placeholder="CSV list that should go through the public internet">
			</label>

			<label class="flags">
				Tunnel Domains
				<input id="csv-input" type=text bind:value={tunnelDomains} placeholder="CSV list that should ONLY be routed through tunnel">
			</label>

			<label class="flags">
				Fast Fail
				<input id="csv-input" type=text bind:value={fastFail} placeholder="CSV list of domains that should be ignored (speeds up tests)">
			</label>

			<label class="flags">
				Basic Auth User
				<input type="text" bind:value={basicAuthUser} placeholder="username for URL basic auth">
			</label>

			<label class="flags">
				Basic Auth User
				<input type="text" bind:value={basicAuthHostPort} placeholder="Port used by server, probaly 80">
			</label>
		</div>

		<div class="flag-section">
			<p class="hint">Network Config</p>
			{#if pacUrl.length === 0}
			<label class="flags">
				Parent Proxy
				<input type="text" bind:value={parentProxy} placeholder="someproxy.yourdomain.com">
			</label>
			{/if}

			{#if parentProxy.length === 0}
			<label class="flags">
				Parent Proxy
				<input type="text" bind:value={pacUrl} placeholder="file path or URL for PAC">
			</label>
			{/if}

			<label class="flags">
				{#if parentProxy.length > 0 || pacUrl.length > 0}
					<input type="checkbox" bind:value={tunnelThruProxy}>
				{:else}
					<input type="checkbox" bind:value={tunnelThruProxy} disabled="disabled">
				{/if}
				Tunnel through Proxy
			</label>

			<label class="flags">
				<input type="checkbox" checked bind:value={autoDetectPac}>
				Auto Detect PAC				
			</label>

			<label class="flags">
				DNS Server(s)
				<input type="text" bind:value={dnsServer} placeholder="DNS min. two recommended">
			</label>
		</div>
	<!-- end of flags section -->
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
			<code>-x {scDc.get(dc_choice)}</code>
	</div>
<!-- 	
	// ssl config
	let noSslBump = [];
	// domain based routing + config
	let directDomains = '';
	let tunnelDomains = '';
	let fastFail = '';
	let basicAuthUser = '';
	let basicAuthPwd = '*****';
	let basicAuthHostPort = '80';
	// logging
	let verbosity = 'normal';
	let logfile = '';
	// network config
	let parentProxy = '';
	let parentProxyUser = 'username';
	let parentProxyPwd = 'password';
	let pacUrl = '';
	let tunnelThruProxy = false;
	let autoDetectPac = true;
	let dnsServer = '';
 -->

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
	<p>If you just downloaded sauce connect you can find the binary at a path like this:</p>
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
		/* border: 1px solid;
		border-color: darkgrey; */
		background-color:#f6f9fc;
		margin-top: 20px;
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

	#csv-input {
		width: 480px;
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

		#csv-input {
			width: 100%;
		}
	}

	@media screen {
		main {
			max-width: none;
		}
	}
</style>