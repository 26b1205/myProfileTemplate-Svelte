<script>
	// Home Page logic
</script>

<svelte:head>
	<title>My Profile | Welcome</title>
	<meta name="description" content="Welcome to my profile website powered by SvelteKit and Tailwind CSS" />
</svelte:head>

<main class="min-h-screen flex items-center justify-center p-4 md:p-8">
	<div class="relative w-full max-w-2xl">
		<!-- Background glow effects -->
		<div class="absolute -top-12 -left-12 w-64 h-64 bg-blue-500 rounded-full blur-3xl opacity-30 animate-pulse"></div>
		<div class="absolute -bottom-12 -right-12 w-64 h-64 bg-cyan-500 rounded-full blur-3xl opacity-20 animate-pulse" style="animation-delay: 2s;"></div>

		<!-- Main Card -->
		<div class="relative bg-white/5 backdrop-blur-xl border border-white/10 rounded-3xl p-8 md:p-12 shadow-2xl shadow-blue-950/50 overflow-hidden">
			<!-- Top decorative light beam -->
			<div class="absolute top-0 left-0 right-0 h-[1px] bg-gradient-to-r from-transparent via-blue-400/50 to-transparent"></div>
			
			<div class="flex flex-col items-center md:items-start md:flex-row gap-8">
				<!-- Profile Avatar Placeholder -->
				<div class="relative group">
					<div class="absolute -inset-1 rounded-full bg-gradient-to-r from-blue-500 to-cyan-500 blur opacity-75 group-hover:opacity-100 transition duration-1000 group-hover:duration-200"></div>
					<div class="relative w-28 h-28 md:w-32 md:h-32 rounded-full bg-slate-900 flex items-center justify-center border border-white/10 text-4xl md:text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-tr from-blue-400 to-cyan-300">
						ME
					</div>
				</div>

				<!-- Info Section -->
				<div class="flex-1 text-center md:text-left space-y-4">
					<div>
						<span class="inline-flex items-center gap-1.5 px-3 py-1 rounded-full text-xs font-medium bg-blue-500/10 text-blue-300 border border-blue-500/20 mb-3">
							<span class="w-1.5 h-1.5 rounded-full bg-blue-400 animate-ping"></span>
							Available for Projects
						</span>
						<h1 class="text-3xl md:text-4xl font-extrabold tracking-tight text-white">
							Hi, I'm <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-400 via-cyan-300 to-blue-500">Developer</span>
						</h1>
						<p class="text-slate-400 text-sm mt-1">Fullstack Engineer & UI/UX Enthusiast</p>
					</div>

					<p class="text-slate-300 leading-relaxed text-sm md:text-base">
						Welcome to my profile template! This page is powered by SvelteKit and styled using Tailwind CSS v4. 
						Edit <code class="px-1.5 py-0.5 rounded bg-slate-950 text-cyan-400 text-xs font-mono border border-white/5">src/routes/+page.svelte</code> to customize this page.
					</p>

					<!-- Tags/Skills -->
					<div class="flex flex-wrap justify-center md:justify-start gap-2 pt-2">
						<span class="px-3 py-1 text-xs rounded-lg bg-white/5 border border-white/10 text-slate-300 hover:bg-white/10 hover:border-white/20 transition cursor-default">SvelteKit</span>
						<span class="px-3 py-1 text-xs rounded-lg bg-white/5 border border-white/10 text-slate-300 hover:bg-white/10 hover:border-white/20 transition cursor-default">Tailwind CSS v4</span>
						<span class="px-3 py-1 text-xs rounded-lg bg-white/5 border border-white/10 text-slate-300 hover:bg-white/10 hover:border-white/20 transition cursor-default">Vite</span>
					</div>

					<!-- Navigation Links -->
					<div class="flex flex-col sm:flex-row gap-3 pt-6 w-full justify-center md:justify-start">
						<a href="https://svelte.dev/docs/kit" target="_blank" rel="noopener noreferrer" class="inline-flex items-center justify-center gap-2 px-5 py-2.5 rounded-xl bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-500 hover:to-cyan-500 text-white font-medium text-sm transition-all duration-200 shadow-lg shadow-blue-500/20 hover:scale-[1.02]">
							SvelteKit Docs
							<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-4 h-4">
								<path fill-rule="evenodd" d="M3 10a.75.75 0 0 1 .75-.75h10.638L10.23 5.29a.75.75 0 1 1 1.04-1.08l5.5 5.25a.75.75 0 0 1 0 1.08l-5.5 5.25a.75.75 0 1 1-1.04-1.08l4.158-3.96H3.75A.75.75 0 0 1 3 10Z" clip-rule="evenodd" />
							</svg>
						</a>
						<a href="https://tailwindcss.com/" target="_blank" rel="noopener noreferrer" class="inline-flex items-center justify-center gap-2 px-5 py-2.5 rounded-xl bg-white/5 hover:bg-white/10 border border-white/10 hover:border-white/20 text-slate-200 font-medium text-sm transition-all duration-200 hover:scale-[1.02]">
							Tailwind CSS
						</a>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>
