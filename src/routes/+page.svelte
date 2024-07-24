<script lang="ts">
	import { onMount } from 'svelte';
	
    import available_langs from "../assets/langs/available_langs.json";
    import guide_en from "../assets/langs/en.json";
	
    import logo from '../assets/logo.png';
    import logo_extension from '../assets/logo_extension.png';
	
    import mobile__langs_selector from '../assets/mobile__langs_selector.webp';
    import mobile__toggle_captions from '../assets/mobile__toggle_captions.webp';
	
    import overlay__customize_box from '../assets/overlay__customize_box.webp';
    import overlay__customize_settings_background from '../assets/overlay__customize_settings_background.png';
    import overlay__customize_settings_text from '../assets/overlay__customize_settings_text.png';
    import overlay__langs_selector from '../assets/overlay__langs_selector.webp';
    import overlay__open_settings from '../assets/overlay__open_settings.webp';
    import overlay__toggle_captions from '../assets/overlay__toggle_captions.webp';
	
    import TutoItem from '../components/TutoItem.svelte';

    const MAIN_SITE_URL = 'https://ultimatecc.net';
    const CONTRIBUTE_URL = 'https://github.com/UltimateCC/guide';
    const DISCORD_URL = '';

    type LanguageCodes = keyof typeof available_langs;
    const AVAILABLE_LANGS: Record<LanguageCodes, string> = available_langs;

    // The different devices that the user can have
    enum Device {
        OVERLAY,
        MOBILE
    }

    let selectedDevice: Device = Device.OVERLAY;
    let selectedLang: LanguageCodes = 'en';
    let guide_content = guide_en;

    let isOpenOverlayCaptions: boolean = true;
    let isOpenOverlayLangs: boolean = true;
    let isOpenOverlayCustomize: boolean = true;

    let isOpenMobileCaptions: boolean = true;
    let isOpenMobileLangs: boolean = true;

    let isOpenCommonIssues: boolean = true;

    // Switch the device function
    function switchDevice(newDevice: Device) {
        selectedDevice = newDevice;

        // Reset the states of the different sections
        isOpenOverlayCaptions = true;
        isOpenOverlayLangs = true;
        isOpenOverlayCustomize = true;

        isOpenMobileCaptions = true;
        isOpenMobileLangs = true;

        isOpenCommonIssues = true;
    }

    function closeAllOverlayItems(except: string) {
        isOpenOverlayCaptions = except === 'toggle_captions';
        isOpenOverlayLangs = except === 'choose_language';
        isOpenOverlayCustomize = except === 'customize';
        isOpenCommonIssues = except === 'common_issues';
    }

    function closeAllMobileItems(except: string) {
        isOpenMobileCaptions = except === 'toggle_captions';
        isOpenMobileLangs = except === 'choose_language';
        isOpenCommonIssues = except === 'common_issues';
    }

    onMount(() => {
        // Get the user's device type
        if (window.innerWidth < 768) selectedDevice = Device.MOBILE;

        // Get the user's language
        if (navigator.languages) {
            for (let i = 0; i < navigator.languages.length; i++) {
                const currentLang: string = navigator.languages[i].substring(0, 2);
                // Check if the the language is the same as the type LanguageCodes
                if (AVAILABLE_LANGS.hasOwnProperty(currentLang)) {
                    selectedLang = currentLang as LanguageCodes;
                    break;
                }
            }
        }

        // Load the guide content based on the selected language
        loadGuideContent(selectedLang);
    });

    // Load the guide content based on the new language code
    async function loadGuideContent(newLang: LanguageCodes) {
        // Check if the language is available 
        if (!AVAILABLE_LANGS.hasOwnProperty(newLang)) {
            console.error(`Invalid language code: ${newLang}`);
            guide_content = guide_en;
            return;
        }

        // If newLang is 'en' then we don't need to load the content
        if (newLang === 'en') {
            guide_content = guide_en;
            return;
        }

        // Load the content
        try {
            const module = await import(`../assets/langs/${newLang}.json`);
            guide_content = module.default;
        } catch (e) {
            console.error(`Failed to load guide content for language: ${newLang}`, e);
            guide_content = guide_en;
        }
    }

    $: loadGuideContent(selectedLang);
</script>

<div class="main-page">
    <div class="main-header container">
        <div class="main-logo">
            <a href={MAIN_SITE_URL} class="logo-box">
                <img src={logo} alt="main website" />
            </a>
        </div>
        <div class="main-title">
            <h1>{guide_content["HEADER__TITLE"]}</h1>
        </div>
    </div>
    <div class="main-container">
        <div class="navigation container">
            <div class="first-nav">
                <div class="quick-nav">
                    <h2>{guide_content["NAVIGATION__QUICK_NAV"]}</h2>
                    <div>
                        <a href={"#"} on:click={() => {
                            (selectedDevice === Device.OVERLAY) ? closeAllOverlayItems('toggle_captions') : closeAllMobileItems('toggle_captions');
                        }}>
                            {guide_content["NAVIGATION__TOGGLE_SUBTITLE"]}
                        </a>
                        <a href={"#"} on:click={() => {
                            (selectedDevice === Device.OVERLAY) ? closeAllOverlayItems('choose_language') : closeAllMobileItems('choose_language');
                        }}>
                            {guide_content["NAVIGATION__CHOOSE_LANGUAGE"]}
                        </a>
                        {#if selectedDevice === Device.OVERLAY}
                            <a href={"#"} on:click={() => {
                                closeAllOverlayItems('customize');
                            }}>
                                {guide_content["NAVIGATION__CUSTOMIZE"]}
                            </a>
                        {/if}
                        <a href={"#"} on:click={() => {
                            (selectedDevice === Device.OVERLAY) ? closeAllOverlayItems('common_issues') : closeAllMobileItems('common_issues');
                        }}>
                            {guide_content["NAVIGATION__COMMON_ISSUES"]}
                        </a>
                    </div>
                </div>
                
                <div class="langs">
                    <h2>{guide_content["NAVIGATION__YOUR_LANGUAGE"]}</h2>
                    <div>
                        <select id="language" class="theme-select" bind:value={selectedLang}>
                            {#each Object.entries(AVAILABLE_LANGS) as [lang_code, lang_name]}
                                <option value={lang_code}>{lang_name}</option>
                            {/each}
                        </select>
                    </div>
                </div>

                <div class="devices">
                    <h2>{guide_content["NAVIGATION__YOUR_DEVICE"]}</h2>
                    <div>
                        <button id="overlay" class:active={selectedDevice === Device.OVERLAY} on:click={() => switchDevice(Device.OVERLAY)}>
                            <svg xmlns="http://www.w3.org/2000/svg" height="1em" width="1em" viewBox="0 -960 960 960">
                                <path d="M20-193.85v-60h80v-555.38h760v555.38h80v60H20Zm380-60h160v-35.38H400v35.38Zm-240-95.38h640v-400H160v400Zm320-200Z"/>
                            </svg>
                        </button>
                        <button id="mobile" class:active={selectedDevice === Device.MOBILE} on:click={() => switchDevice(Device.MOBILE)}>
                            <svg xmlns="http://www.w3.org/2000/svg" height="1em" width="1em" viewBox="0 -960 960 960">
                                <path d="M404.62-166.92h150.76v-35.39H404.62v35.39ZM292.31-60Q262-60 241-81q-21-21-21-51.31v-695.38Q220-858 241-879q21-21 51.31-21h375.38Q698-900 719-879q21 21 21 51.31v695.38Q740-102 719-81q-21 21-51.31 21H292.31ZM280-309.23h400V-730H280v420.77Z"/>
                            </svg>
                        </button>
                    </div>
                </div>
            </div>

            <div class="seconde-nav">
                <div class="contribute">
                    <a href={CONTRIBUTE_URL}>
                        {guide_content["NAVIGATION__CONTRIBUTE"]}
                        <svg xmlns="http://www.w3.org/2000/svg" height="1em" width="1em" viewBox="0 0 14.78 13.8">
                            <path d="M7.39,1.14C4.15,1.14,1.53,3.78,1.53,7.05c0,2.61,1.68,4.82,4.01,5.6.29.06.4-.13.4-.28,0-.14,0-.61,0-1.1-1.63.35-1.97-.7-1.97-.7-.26-.68-.65-.86-.65-.86-.53-.36.04-.36.04-.36.59.04.9.61.9.61.52.9,1.37.65,1.71.49.05-.38.2-.65.37-.79-1.3-.14-2.67-.65-2.67-2.91,0-.65.23-1.17.6-1.58-.06-.15-.26-.75.06-1.56,0,0,.49-.16,1.61.61.48-.13.97-.2,1.47-.2.49,0,1,.07,1.47.2,1.12-.76,1.61-.61,1.61-.61.32.81.12,1.42.06,1.56.38.41.6.94.6,1.58,0,2.27-1.37,2.77-2.68,2.91.21.19.4.54.4,1.1,0,.79,0,1.43,0,1.62,0,.16.11.34.4.28,2.33-.78,4.01-2.99,4.01-5.6,0-3.27-2.62-5.91-5.85-5.91Z"/>
                        </svg>
                    </a>
                </div>
    
                <div class="streamer">
                    <a href={MAIN_SITE_URL}>
                        {guide_content["NAVIGATION__STREAMER"]}
                    </a>
                </div>
            </div>
        </div>
        <div class="tuto container" id={selectedDevice === Device.OVERLAY ? '' : ''}>
            {#if selectedDevice === Device.OVERLAY}
                <TutoItem title={guide_content["NAVIGATION__TOGGLE_SUBTITLE"]} htmlId="overlay__toggle_captions" bind:expanded={isOpenOverlayCaptions}>
                    <div class="toggle-captions-hover">
                        <p>{guide_content["OVERLAY"]["TOGGLE_SUBTITLE__HOVER"]}</p>
                    </div>
                    <div class="toggle-captions-show">
                        <h4>{guide_content["OVERLAY"]["TOGGLE_SUBTITLE__SHOW_TITLE"]}</h4>
                        <p>
                            {guide_content["OVERLAY"]["TOGGLE_SUBTITLE__SHOW_CONTENT_PART_1"]}
                            <svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 -960 960 960">
                                <path d="M200-160q-33 0-56.5-23.5T120-240v-480q0-33 23.5-56.5T200-800h560q33 0 56.5 23.5T840-720v480q0 33-23.5 56.5T760-160H200Zm80-200h120q17 0 28.5-11.5T440-400v-40h-60v20h-80v-120h80v20h60v-40q0-17-11.5-28.5T400-600H280q-17 0-28.5 11.5T240-560v160q0 17 11.5 28.5T280-360Zm280 0h120q17 0 28.5-11.5T720-400v-40h-60v20h-80v-120h80v20h60v-40q0-17-11.5-28.5T680-600H560q-17 0-28.5 11.5T520-560v160q0 17 11.5 28.5T560-360Z"/>
                            </svg>
                            {guide_content["OVERLAY"]["TOGGLE_SUBTITLE__SHOW_CONTENT_PART_2"]}
                        </p>
                    </div>
                    <div class="toggle-captions-img">
                        <img src={overlay__toggle_captions} alt="Toggle captions" />
                    </div>
                    <div class="toggle-captions-hide">
                        <h4>{guide_content["OVERLAY"]["TOGGLE_SUBTITLE__HIDE_TITLE"]}</h4>
                        <p>
                            {guide_content["OVERLAY"]["TOGGLE_SUBTITLE__HIDE_CONTENT_PART_1"]}
                            <svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 -960 960 960">
                                <path d="M200-160q-33 0-56.5-23.5T120-240v-480q0-33 23.5-56.5T200-800h560q33 0 56.5 23.5T840-720v480q0 33-23.5 56.5T760-160H200Zm0-80h560v-480H200v480Zm80-120h120q17 0 28.5-11.5T440-400v-40h-60v20h-80v-120h80v20h60v-40q0-17-11.5-28.5T400-600H280q-17 0-28.5 11.5T240-560v160q0 17 11.5 28.5T280-360Zm280 0h120q17 0 28.5-11.5T720-400v-40h-60v20h-80v-120h80v20h60v-40q0-17-11.5-28.5T680-600H560q-17 0-28.5 11.5T520-560v160q0 17 11.5 28.5T560-360ZM200-240v-480 480Z"/>
                            </svg>
                            {guide_content["OVERLAY"]["TOGGLE_SUBTITLE__HIDE_CONTENT_PART_2"]}
                        </p>
                    </div>
                    <div class="toggle-captions-not-visible">
                        <a href={"#"} on:click={() => {
                            (selectedDevice === Device.OVERLAY) ? closeAllOverlayItems('common_issues') : closeAllMobileItems('common_issues');
                        }}>{guide_content["OVERLAY"]["COMMON_ISSUES__NOT_VISIBLE_TITLE"]}</a>
                    </div>
                </TutoItem>
            {:else}
                <TutoItem title={guide_content["NAVIGATION__TOGGLE_SUBTITLE"]} htmlId="mobile__toggle_captions" bind:expanded={isOpenMobileCaptions}>
                    <div class="mobile-text">
                        <p>
                            {guide_content["MOBILE"]["TOGGLE_SUBTITLE__CONTENT_PART_1"]}
                            <img class="logo-extension" src={logo_extension} alt="Extension logo" />
                            {guide_content["MOBILE"]["TOGGLE_SUBTITLE__CONTENT_PART_2"]}
                        </p>
                    </div>
                    <div class="mobile-img">
                        <img src={mobile__toggle_captions} alt="Toggle captions" />
                    </div>
                </TutoItem>
            {/if}

            {#if selectedDevice === Device.OVERLAY}
                <TutoItem title={guide_content["NAVIGATION__CHOOSE_LANGUAGE"]} htmlId="overlay__choose_language" bind:expanded={isOpenOverlayLangs}>
                    <div>
                        <p>{guide_content["OVERLAY"]["CHOOSE_LANGUAGE__CONTENT_1"]}</p>
                        <img src={overlay__open_settings} alt="Open settings" />
                    </div>
                    <div>
                        <p class="mt">{guide_content["OVERLAY"]["CHOOSE_LANGUAGE__CONTENT_2"]}</p>
                        <img src={overlay__langs_selector} alt="Langs selector for overlay" />
                    </div>
                </TutoItem>
            {:else}
                <TutoItem title={guide_content["NAVIGATION__CHOOSE_LANGUAGE"]} htmlId="mobile__choose_language" bind:expanded={isOpenMobileLangs}>
                    <div class="mobile-text">
                        <p>{guide_content["MOBILE"]["CHOOSE_LANGUAGE__CONTENT"]}</p>
                    </div>
                    <div class="mobile-img">
                        <img src={mobile__langs_selector} alt="Langs selector for mobile" />
                    </div>
                </TutoItem>
            {/if}

            {#if selectedDevice === Device.OVERLAY}
                <TutoItem title={guide_content["NAVIGATION__CUSTOMIZE"]} htmlId="overlay__customize" bind:expanded={isOpenOverlayCustomize}>
                    <div class="customize-open-settings">
                        <p>{guide_content["OVERLAY"]["CHOOSE_LANGUAGE__CONTENT_1"]}</p>
                        <img src={overlay__open_settings} alt="Open settings" />
                    </div>
                    <div class="customize-settings-container">
                        <div class="customize-text_content">
                            <h4>{guide_content["OVERLAY"]["CUSTOMIZE__TEXT_TITLE"]}</h4>
                            <p>{guide_content["OVERLAY"]["CUSTOMIZE__TEXT_CONTENT"]}</p>
                        </div>
                        <div class="customize-text_img">
                            <div>
                                <img src={overlay__customize_settings_text} alt="Customize text" />
                            </div>
                        </div>
                        <div class="customize-background_content">
                            <h4>{guide_content["OVERLAY"]["CUSTOMIZE__BACKGROUND_TITLE"]}</h4>
                            <p>{guide_content["OVERLAY"]["CUSTOMIZE__BACKGROUND_CONTENT"]}</p>
                        </div>
                        <div class="customize-background_img">
                            <div>
                                <img src={overlay__customize_settings_background} alt="Customize background" />
                            </div>
                        </div>
                    </div>
                    <div class="customize-box">
                        <h4>{guide_content["OVERLAY"]["CUSTOMIZE__BOX_TITLE"]}</h4>
                        <p>{guide_content["OVERLAY"]["CUSTOMIZE__BOX_CONTENT"]}</p>
                        <img src={overlay__customize_box} alt="Customize box" />
                    </div>
                </TutoItem>
            {/if}

            <TutoItem title={guide_content["NAVIGATION__COMMON_ISSUES"]} htmlId="common_issues" bind:expanded={isOpenCommonIssues}>
                <div class="common_issues-content">
                    <h4>Waiting for broadcaster speech</h4>
                    <p>{guide_content["COMMON_ISSUES__WAITING_FOR_SPEECH"]}</p>
                </div>
                
                {#if selectedDevice === Device.OVERLAY}
                    <div class="common_issues-content">
                        <h4>{guide_content["OVERLAY"]["COMMON_ISSUES__NOT_VISIBLE_TITLE"]}</h4>
                        <p>
                            {guide_content["OVERLAY"]['COMMON_ISSUES__NOT_VISIBLE_CONTENT_1']}
                            <img class="logo-extension" src={logo_extension} alt="Extension logo" />
                            {guide_content["OVERLAY"]["COMMON_ISSUES__NOT_VISIBLE_CONTENT_2"]}
                        </p>
                        <p>{guide_content["OVERLAY"]["COMMON_ISSUES__NOT_VISIBLE_NO_ICON"]}</p>
                    </div>
                {/if}

                <div class="common_issues-content">
                    <h4>{guide_content["COMMON_ISSUES__DISCORD_TITLE"]}</h4>
                    <p>
                        {guide_content["COMMON_ISSUES__DISCORD_CONTENT"]}
                        <a href={DISCORD_URL} target="_blank">{guide_content["COMMON_ISSUES__DISCORD_LINK"]}</a>
                    </p>
                </div>
            </TutoItem>
        </div>
    </div>
</div>