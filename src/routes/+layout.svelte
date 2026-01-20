<script>
	let { children } = $props();
</script>

<svelte:head>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link
		rel="preconnect"
		href="https://fonts.gstatic.com"
		crossorigin="anonymous"
	/>
	<link
		href="https://fonts.googleapis.com/css2?family=Raleway:wght@400;500;600;700;800;900&display=swap"
		rel="stylesheet"
	/>
	<title>OHG Scribe — Professional Healthcare Transcription</title>
	<meta
		name="description"
		content="Turn advisory boards, interviews, and meetings into professional Word documents in minutes."
	/>
</svelte:head>

<nav class="site-nav">
	<div class="nav-container">
		<a href="/" class="nav-logo">
			<span class="logo-ohg">OHG</span><span class="logo-scribe"
				>Scribe</span
			>
		</a>
		<div class="nav-links">
			<a href="/guide" class="nav-link">Guide</a>
			<a
				href="https://github.com/joemacOHG/ohg-scribe/releases/latest"
				class="nav-link nav-download">Download</a
			>
		</div>
	</div>
</nav>

{@render children()}

<style>
	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(html) {
		scroll-behavior: smooth;
	}

	:global(body) {
		font-family: "Raleway", sans-serif;
		font-size: 16px;
		line-height: 1.6;
		color: #1e293b;
		background: #ffffff;
		overflow-x: hidden;
	}

	:global(:root) {
		/* Brand Colors */
		--purple-deep: #4a1259;
		--magenta: #9b2d7b;
		--blue-deep: #1e3a5f;
		--accent-magenta: #d946ef;
		--accent-pink: #ec4899;
		--text-dark: #1e293b;
		--text-light: #64748b;
		--bg-light: #f8fafc;
		--white: #ffffff;

		/* Gradients */
		--gradient-hero: linear-gradient(
			135deg,
			#4a1259 0%,
			#9b2d7b 50%,
			#1e3a5f 100%
		);
		--gradient-accent: linear-gradient(135deg, #d946ef 0%, #ec4899 100%);
	}

	:global(a) {
		color: inherit;
		text-decoration: none;
	}

	:global(button) {
		font-family: "Raleway", sans-serif;
		cursor: pointer;
	}

	:global(.container) {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 24px;
	}

	:global(section) {
		padding: 80px 0;
	}

	:global(h1, h2, h3, h4, h5, h6) {
		line-height: 1.2;
		font-weight: 700;
	}

	/* Navigation */
	.site-nav {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 1000;
		background: rgba(255, 255, 255, 0.95);
		backdrop-filter: blur(10px);
		border-bottom: 1px solid rgba(0, 0, 0, 0.05);
		padding: 12px 0;
	}

	.nav-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 24px;
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.nav-logo {
		font-size: 20px;
		font-weight: 700;
	}

	.nav-logo .logo-ohg {
		color: var(--purple-deep);
	}

	.nav-logo .logo-scribe {
		color: var(--accent-pink);
		margin-left: 2px;
	}

	.nav-links {
		display: flex;
		gap: 24px;
		align-items: center;
	}

	.nav-link {
		font-size: 14px;
		font-weight: 600;
		color: var(--text-dark);
		transition: color 0.2s;
	}

	.nav-link:hover {
		color: var(--accent-pink);
	}

	.nav-download {
		background: var(--gradient-accent);
		color: white !important;
		padding: 8px 16px;
		border-radius: 6px;
	}

	.nav-download:hover {
		opacity: 0.9;
	}

	/* Add padding for fixed nav */
	:global(body) {
		padding-top: 60px;
	}
</style>
