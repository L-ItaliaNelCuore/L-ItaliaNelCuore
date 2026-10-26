<script>
	import { i18n, t, tl } from '$lib/i18n.svelte.js';
	import blog from '$lib/content/blog.json';
	import ui from '$lib/content/ui.json';
	import community from '$lib/assets/brand/community.jpg';

	// Posts live in src/lib/content/blog.json, under "posts". Each one looks like:
	// { "slug": "first-meeting", "date": "2026-10-26",
	//   "title": { "es": "…", "it": "…", "en": "…" },
	//   "text":  { "es": ["paragraph", …], "it": […], "en": […] } }
	// Newest first, whatever the order in the file.
	/** @type {{ slug: string, date: string, title: any, text: any }[]} */
	const posts = [...blog.posts].sort((a, b) => b.date.localeCompare(a.date));

	const LOCALES = { es: 'es-MX', it: 'it-IT', en: 'en-US' };
	/** @param {string} date */
	const formatDate = (date) =>
		new Date(`${date}T12:00:00`).toLocaleDateString(LOCALES[i18n.lang], {
			day: 'numeric',
			month: 'long',
			year: 'numeric'
		});
</script>

<svelte:head>
	<title>{t(ui.nav.blog)} | {ui.listName}</title>
	<meta name="description" content={t(blog.page.metaDescription)} />
</svelte:head>

<section class="hero">
	<img src={community} alt="" />
	<div class="wrap hero-copy">
		<h1 class="display title">{t(blog.title)}</h1>
		<p class="lead">{t(blog.lead)}</p>
	</div>
</section>

<div class="wrap posts">
	{#each posts as post (post.slug)}
		<article id={post.slug} class="post">
			<time class="date" datetime={post.date}>{formatDate(post.date)}</time>
			<h2 class="post-title">{t(post.title)}</h2>
			<div class="post-text">
				{#each tl(post.text) as p}
					<p>{p}</p>
				{/each}
			</div>
		</article>
	{:else}
		<p class="empty">{t(blog.empty)}</p>
	{/each}
</div>

<style>
	.hero {
		position: relative;
		overflow: hidden;
		background: var(--navy);
		color: var(--on-dark);
	}
	.hero img {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		object-fit: cover;
		filter: grayscale(1) contrast(1.05);
		opacity: 0.38;
		mix-blend-mode: luminosity;
	}
	.hero-copy {
		position: relative;
		padding-block: clamp(4rem, 12vw, 8rem) clamp(3rem, 8vw, 5rem);
		display: grid;
		gap: 1.25rem;
	}
	.title {
		font-size: var(--step-4);
	}
	.lead {
		font-size: var(--step-2);
		font-style: italic;
		font-weight: 600;
		color: var(--sun);
		max-width: 32ch;
	}

	.posts {
		display: grid;
		gap: clamp(2.5rem, 6vw, 4rem);
		padding-block: clamp(2.5rem, 6vw, 4rem) clamp(3rem, 7vw, 5rem);
		max-width: calc(var(--measure) + 2 * var(--gutter));
	}
	.post {
		display: grid;
		gap: 0.6rem;
		padding-bottom: clamp(2.5rem, 6vw, 4rem);
		border-bottom: 1px solid var(--rule);
	}
	.post:last-child {
		padding-bottom: 0;
		border-bottom: 0;
	}
	.date {
		color: var(--ink-soft);
		font-size: var(--step--1);
		font-weight: 600;
		text-transform: uppercase;
		letter-spacing: 0.08em;
	}
	.post-title {
		color: var(--navy);
		font-size: var(--step-3);
		font-weight: 900;
		letter-spacing: -0.02em;
		line-height: 1.05;
	}
	.post-text {
		display: grid;
		gap: 1em;
		margin-top: 0.4rem;
	}
	.empty {
		color: var(--navy);
		font-size: var(--step-1);
		font-style: italic;
		font-weight: 600;
	}
</style>
