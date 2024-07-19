<script lang="ts">
    import '../main.scss';

    const SITE_NAME = "Ultimate Closed Captions";
    const MAIN_SITE_URL = 'https://ultimatecc.net';
    const CURRENT_SITE_URL = "https://guide.ultimatecc.net";
    const IMAGE_URL_PNG = MAIN_SITE_URL + '/favicon.png';
    const IMAGE_URL_FAV = MAIN_SITE_URL + '/favicon.ico';
    const LOGO = MAIN_SITE_URL + '/logo.png';
    const TITLE = "Guide - " + SITE_NAME;
    const DESCRIPTION = "Official guide for viewers, explaining the various features available on our extension in multiple languages";
</script>

<svelte:head>
    <link rel="icon" href={IMAGE_URL_PNG} type="image/png" />
    <link rel="icon" href={IMAGE_URL_FAV} type="image/x-icon" />
    <meta name="title" content={TITLE} />
    <meta name="description" content={DESCRIPTION} />
    <meta property="og:title" content={TITLE} />
    <meta property="og:description" content={DESCRIPTION} />
    <meta property="og:image" content={LOGO} />
    <meta property="og:url" content={CURRENT_SITE_URL} />
    <meta property="og:site_name" content={SITE_NAME} />
	<title>{TITLE}</title>
</svelte:head>

<slot />