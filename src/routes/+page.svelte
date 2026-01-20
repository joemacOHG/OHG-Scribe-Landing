<script lang="ts">
    import { onMount } from "svelte";

    let isMac = $state(true);
    let macArm64Url = $state(
        "https://github.com/joemacOHG/ohg-scribe/releases/latest",
    );
    let macIntelUrl = $state(
        "https://github.com/joemacOHG/ohg-scribe/releases/latest",
    );
    let winDownloadUrl = $state(
        "https://github.com/joemacOHG/ohg-scribe/releases/latest",
    );
    let selectedMacChip = $state<"arm64" | "intel">("arm64");
    let showMacSelector = $state(false);

    onMount(async () => {
        isMac = navigator.platform.toUpperCase().indexOf("MAC") >= 0;

        // Fetch latest release from GitHub API
        try {
            const res = await fetch(
                "https://api.github.com/repos/joemacOHG/ohg-scribe/releases/latest",
            );
            if (res.ok) {
                const release = await res.json();
                for (const asset of release.assets) {
                    if (
                        asset.name.endsWith(".dmg") &&
                        asset.name.includes("aarch64")
                    ) {
                        macArm64Url = asset.browser_download_url;
                    }
                    if (
                        asset.name.endsWith(".dmg") &&
                        asset.name.includes("x86_64")
                    ) {
                        macIntelUrl = asset.browser_download_url;
                    }
                    if (asset.name.endsWith("-setup.exe")) {
                        winDownloadUrl = asset.browser_download_url;
                    }
                }
            }
        } catch (e) {
            console.warn("Could not fetch latest release:", e);
        }
    });

    // Computed download URL based on platform and chip selection
    const macDownloadUrl = $derived(
        selectedMacChip === "arm64" ? macArm64Url : macIntelUrl,
    );
    const downloadUrl = $derived(isMac ? macDownloadUrl : winDownloadUrl);
</script>

<!-- Hero Section -->
<section class="hero">
    <div class="hero-bg">
        <div class="particles"></div>
    </div>
    <div class="hero-content container">
        <div class="hero-text">
            <div class="logo">
                <span class="logo-ohg">OHG</span>
                <span class="logo-scribe">Scribe</span>
            </div>
            <h1>Drop. Transcribe. Done.</h1>
            <p class="subhead">
                An OPEN Health–approved internal transcription tool for advisory
                boards, interviews, and meetings. Turn recordings into clean,
                speaker-labeled Word documents in minutes.
            </p>
            <div class="cta-buttons">
                {#if isMac}
                    <a href={downloadUrl} class="btn btn-primary">
                        Download for Mac
                        <span class="chip-label">
                            {selectedMacChip === "arm64"
                                ? "Apple Silicon"
                                : "Intel"}
                        </span>
                    </a>
                    <button
                        class="btn-text-link"
                        onclick={() => (showMacSelector = !showMacSelector)}
                    >
                        {showMacSelector
                            ? "Hide options"
                            : "Need Intel version?"}
                    </button>
                    {#if showMacSelector}
                        <div class="mac-selector">
                            <button
                                class="chip-btn"
                                class:active={selectedMacChip === "arm64"}
                                onclick={() => (selectedMacChip = "arm64")}
                            >
                                Apple Silicon (M1/M2/M3)
                            </button>
                            <button
                                class="chip-btn"
                                class:active={selectedMacChip === "intel"}
                                onclick={() => (selectedMacChip = "intel")}
                            >
                                Intel Mac
                            </button>
                        </div>
                    {/if}
                {:else}
                    <a href={downloadUrl} class="btn btn-primary">
                        Download for Windows
                    </a>
                {/if}
                <span class="btn-secondary">
                    Also available for {isMac ? "Windows" : "Mac"}
                </span>
            </div>
        </div>
        <div class="hero-image">
            <div class="hero-layer hero-back">
                <img
                    src="/screenshots/hero-transcript.png"
                    alt="Completed transcript document"
                />
            </div>
            <div class="hero-layer hero-front">
                <div class="app-frame">
                    <img
                        src="/screenshots/hero-app.png"
                        alt="OHG Scribe interface"
                    />
                </div>
            </div>
        </div>
    </div>
</section>

<!-- How It Works -->
<section class="how-it-works">
    <div class="container">
        <h2>How It Works</h2>
        <div class="steps">
            <div class="step">
                <div class="step-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
                        <polyline points="17,8 12,3 7,8" />
                        <line x1="12" y1="3" x2="12" y2="15" />
                    </svg>
                </div>
                <h3>Drop</h3>
                <p>
                    Drag any audio or video file — MP4, MOV, MP3, WAV, and 8+
                    additional formats.
                </p>
            </div>
            <div class="step-connector"></div>
            <div class="step">
                <div class="step-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path d="M12 2a10 10 0 1 0 10 10" />
                        <path d="M12 12l4-4" />
                        <path d="M12 6v6" />
                        <circle cx="18" cy="6" r="3" />
                    </svg>
                </div>
                <h3>Transcribe</h3>
                <p>
                    AI processes your recording with high accuracy,
                    automatically identifying speakers and structure.
                </p>
            </div>
            <div class="step-connector"></div>
            <div class="step">
                <div class="step-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path
                            d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"
                        />
                        <polyline points="14,2 14,8 20,8" />
                        <line x1="16" y1="13" x2="8" y2="13" />
                        <line x1="16" y1="17" x2="8" y2="17" />
                    </svg>
                </div>
                <h3>Export</h3>
                <p>
                    Download a formatted Word document with speaker labels,
                    timestamps, and optional summaries.
                </p>
            </div>
        </div>
    </div>
</section>

<!-- Video Demo -->
<section class="video-demo">
    <div class="container">
        <h2>See OHG Scribe in Action</h2>
        <div class="video-wrapper">
            <video controls preload="metadata" class="demo-video">
                <source src="/appwalkthrough.mp4" type="video/mp4" />
                Your browser does not support the video tag.
            </video>
            <p class="video-caption">Watch a quick walkthrough</p>
        </div>
    </div>
</section>

<!-- Features Grid -->
<section class="features">
    <div class="container">
        <h2>Built for Healthcare Professionals</h2>
        <div class="features-grid">
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path
                            d="M13 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V9z"
                        />
                        <polyline points="13 2 13 9 20 9" />
                    </svg>
                </div>
                <h3>11 Formats Supported</h3>
                <p>
                    MP4, MOV, MP3, WAV, and more — drop virtually any recording
                </p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2" />
                        <circle cx="9" cy="7" r="4" />
                        <path d="M23 21v-2a4 4 0 0 0-3-3.87" />
                        <path d="M16 3.13a4 4 0 0 1 0 7.75" />
                    </svg>
                </div>
                <h3>Speaker Identification</h3>
                <p>Automatically distinguishes up to 20 different speakers</p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <polygon
                            points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"
                        />
                    </svg>
                </div>
                <h3>AI Speaker Names</h3>
                <p>
                    Infers real names from conversation context — no
                    pre-training needed
                </p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20" />
                        <path
                            d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"
                        />
                    </svg>
                </div>
                <h3>Medical Vocabulary</h3>
                <p>
                    Boost recognition of drug names, acronyms, and clinical
                    terms
                </p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <path
                            d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"
                        />
                        <polyline points="14,2 14,8 20,8" />
                        <line x1="16" y1="13" x2="8" y2="13" />
                        <line x1="16" y1="17" x2="8" y2="17" />
                        <polyline points="10 9 9 9 8 9" />
                    </svg>
                </div>
                <h3>Word Export</h3>
                <p>
                    Clean .docx files with timestamps, speaker labels, and
                    optional summaries
                </p>
            </div>
            <div class="feature-card">
                <div class="feature-icon">
                    <svg
                        viewBox="0 0 24 24"
                        fill="none"
                        stroke="currentColor"
                        stroke-width="2"
                    >
                        <polygon points="5 3 19 12 5 21 5 3" />
                    </svg>
                </div>
                <h3>Interactive Playback</h3>
                <p>
                    Click any transcript line to jump to that moment in the
                    audio
                </p>
            </div>
        </div>
    </div>
</section>

<!-- Use Cases -->
<section class="use-cases">
    <div class="container">
        <h2>Made for the Work You Do</h2>
        <div class="use-cases-grid">
            <div class="use-case-card">
                <div class="use-case-icon">👥</div>
                <h3>Advisory Board Meetings</h3>
                <p>
                    Accurately capture expert discussions with clear speaker
                    attribution across large groups.
                </p>
            </div>
            <div class="use-case-card">
                <div class="use-case-icon">🎤</div>
                <h3>KOL Interviews</h3>
                <p>
                    Generate verbatim, time-stamped transcripts ready for
                    medical-legal review.
                </p>
            </div>
            <div class="use-case-card">
                <div class="use-case-icon">📋</div>
                <h3>Patient Research</h3>
                <p>
                    Document qualitative interviews with structured transcripts
                    suitable for analysis.
                </p>
            </div>
        </div>
    </div>
</section>

<!-- Get Started -->
<section class="get-started">
    <div class="container">
        <h2>Ready to Get Started?</h2>
        <p class="get-started-desc">
            OHG Scribe requires an internal API key for transcription services.
            Request access to begin transcribing.
        </p>
        <div class="get-started-action">
            <a
                href="mailto:assemblyai@openhealthgroup.com?subject=OHG%20Scribe%20API%20Key%20Request&body=Hi%2C%0A%0AI%20would%20like%20to%20request%20an%20API%20key%20for%20OHG%20Scribe.%0A%0AName%3A%20%0AEmail%3A%20%0ATeam%2FDepartment%3A%20%0A%0AThank%20you!"
                class="btn btn-primary"
            >
                Request API Key
            </a>
            <p class="get-started-note">We'll send your key within 24 hours</p>
            <a href="/guide" class="guide-link">View Setup Guide →</a>
        </div>
    </div>
</section>

<!-- Footer CTA -->
<section class="footer-cta">
    <div class="container">
        <h2>Ready to Get Started?</h2>
        <div class="cta-buttons">
            <a href={downloadUrl} class="btn btn-primary">
                {isMac ? "Download for Mac" : "Download for Windows"}
            </a>
            <span class="btn-secondary-light">
                Also available for {isMac ? "Windows" : "Mac"}
            </span>
        </div>
        <div class="contact">
            <p>Questions? Reach out:</p>
            <a href="mailto:joestevens@openhealthgroup.com"
                >joestevens@openhealthgroup.com</a
            >
        </div>
    </div>
</section>

<footer>
    <div class="container">
        <p>
            OHG Scribe — An OPEN Health internal tool • {new Date().getFullYear()}
        </p>
    </div>
</footer>

<style>
    /* Hero Section */
    .hero {
        position: relative;
        min-height: 100vh;
        display: flex;
        align-items: center;
        background: var(--gradient-hero);
        color: white;
        overflow: hidden;
        padding: 80px 0;
    }

    .hero-bg {
        position: absolute;
        inset: 0;
        overflow: hidden;
    }

    .particles {
        position: absolute;
        inset: 0;
        background-image: radial-gradient(
                circle at 20% 80%,
                rgba(217, 70, 239, 0.1) 0%,
                transparent 50%
            ),
            radial-gradient(
                circle at 80% 20%,
                rgba(236, 72, 153, 0.1) 0%,
                transparent 50%
            );
        animation: float 20s ease-in-out infinite;
    }

    @keyframes float {
        0%,
        100% {
            transform: translateY(0) rotate(0deg);
        }
        50% {
            transform: translateY(-20px) rotate(1deg);
        }
    }

    .hero-content {
        position: relative;
        z-index: 1;
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 60px;
        align-items: center;
    }

    .logo {
        font-size: 28px;
        margin-bottom: 24px;
    }

    .logo-ohg {
        font-weight: 800;
    }

    .logo-scribe {
        font-weight: 400;
        margin-left: 4px;
    }

    .hero h1 {
        font-size: 56px;
        font-weight: 800;
        margin-bottom: 20px;
        background: linear-gradient(135deg, #fff 0%, #ec4899 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
    }

    .subhead {
        font-size: 20px;
        opacity: 0.9;
        margin-bottom: 32px;
        max-width: 500px;
    }

    .cta-buttons {
        display: flex;
        flex-direction: column;
        gap: 12px;
        align-items: flex-start;
    }

    .btn {
        display: inline-flex;
        align-items: center;
        justify-content: center;
        padding: 16px 32px;
        border-radius: 12px;
        font-size: 16px;
        font-weight: 600;
        transition: all 0.2s ease;
        text-decoration: none;
    }

    .btn-primary {
        background: var(--gradient-accent);
        color: white;
        box-shadow: 0 8px 32px rgba(236, 72, 153, 0.4);
    }

    .btn-primary:hover {
        transform: translateY(-2px);
        box-shadow: 0 12px 40px rgba(236, 72, 153, 0.5);
    }

    .btn-secondary {
        color: rgba(255, 255, 255, 0.8);
        font-size: 14px;
    }

    .btn-secondary:hover {
        color: white;
    }

    .btn-secondary-light {
        color: rgba(255, 255, 255, 0.7);
        font-size: 14px;
    }

    /* Mac Chip Selector */
    .chip-label {
        font-size: 12px;
        font-weight: 400;
        opacity: 0.8;
        margin-left: 8px;
    }

    .btn-text-link {
        background: none;
        border: none;
        color: rgba(255, 255, 255, 0.7);
        font-size: 13px;
        cursor: pointer;
        padding: 0;
        text-decoration: underline;
        text-underline-offset: 3px;
    }

    .btn-text-link:hover {
        color: white;
    }

    .mac-selector {
        display: flex;
        gap: 8px;
        margin-top: 8px;
    }

    .chip-btn {
        padding: 8px 16px;
        background: rgba(255, 255, 255, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.2);
        border-radius: 8px;
        color: rgba(255, 255, 255, 0.8);
        font-size: 13px;
        cursor: pointer;
        transition: all 0.2s;
    }

    .chip-btn:hover {
        background: rgba(255, 255, 255, 0.15);
    }

    .chip-btn.active {
        background: rgba(255, 255, 255, 0.2);
        border-color: white;
        color: white;
    }

    .hero-image {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 400px;
    }

    .hero-layer {
        position: absolute;
    }

    .hero-back {
        transform: rotate(-6deg) translate(-60px, 20px);
        z-index: 1;
    }

    .hero-back img {
        width: 380px;
        border-radius: 8px;
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.25);
        border: 1px solid rgba(0, 0, 0, 0.1);
    }

    .hero-front {
        z-index: 2;
        transform: translate(60px, -10px);
    }

    .app-frame {
        background: transparent;
        border-radius: 12px;
        box-shadow: 0 40px 80px rgba(0, 0, 0, 0.4);
        overflow: hidden;
    }

    .app-frame img {
        width: 420px;
        display: block;
        border-radius: 12px;
    }

    /* How It Works */
    .how-it-works {
        background: var(--bg-light);
    }

    .how-it-works h2 {
        text-align: center;
        font-size: 40px;
        margin-bottom: 60px;
        color: var(--text-dark);
    }

    .steps {
        display: flex;
        align-items: flex-start;
        justify-content: center;
        gap: 20px;
    }

    .step {
        flex: 1;
        max-width: 280px;
        text-align: center;
        padding: 32px 24px;
        background: white;
        border-radius: 16px;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
        transition: transform 0.2s ease;
    }

    .step:hover {
        transform: translateY(-4px);
    }

    .step-icon {
        width: 64px;
        height: 64px;
        margin: 0 auto 20px;
        background: var(--gradient-accent);
        border-radius: 16px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
    }

    .step-icon svg {
        width: 32px;
        height: 32px;
    }

    .step h3 {
        font-size: 22px;
        margin-bottom: 12px;
        color: var(--text-dark);
    }

    .step p {
        color: var(--text-light);
        font-size: 15px;
    }

    .step-connector {
        width: 60px;
        height: 2px;
        background: linear-gradient(
            90deg,
            var(--accent-magenta),
            var(--accent-pink)
        );
        margin-top: 80px;
        opacity: 0.5;
    }

    /* Video Demo */
    .video-demo {
        background: var(--gradient-hero);
        color: white;
        text-align: center;
    }

    .video-demo h2 {
        font-size: 40px;
        margin-bottom: 40px;
    }

    .video-wrapper {
        max-width: 800px;
        margin: 0 auto;
    }

    .demo-video {
        width: 100%;
        border-radius: 16px;
        box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
        border: 1px solid rgba(255, 255, 255, 0.15);
    }

    .video-caption {
        margin-top: 20px;
        opacity: 0.8;
    }

    /* Features */
    .features {
        background: white;
    }

    .features h2 {
        text-align: center;
        font-size: 40px;
        margin-bottom: 60px;
        color: var(--text-dark);
    }

    .features-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 24px;
    }

    .feature-card {
        padding: 32px;
        background: var(--bg-light);
        border-radius: 16px;
        transition: all 0.2s ease;
        border: 1px solid transparent;
    }

    .feature-card:hover {
        transform: translateY(-4px);
        border-color: var(--accent-pink);
        box-shadow: 0 12px 40px rgba(236, 72, 153, 0.1);
    }

    .feature-icon {
        width: 48px;
        height: 48px;
        background: var(--gradient-accent);
        border-radius: 12px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        margin-bottom: 20px;
    }

    .feature-icon svg {
        width: 24px;
        height: 24px;
    }

    .feature-card h3 {
        font-size: 18px;
        margin-bottom: 12px;
        color: var(--text-dark);
    }

    .feature-card p {
        color: var(--text-light);
        font-size: 14px;
        line-height: 1.6;
    }

    /* Use Cases */
    .use-cases {
        background: linear-gradient(180deg, var(--bg-light) 0%, #f3e8ff 100%);
    }

    .use-cases h2 {
        text-align: center;
        font-size: 40px;
        margin-bottom: 60px;
        color: var(--text-dark);
    }

    .use-cases-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 32px;
    }

    .use-case-card {
        padding: 40px 32px;
        background: white;
        border-radius: 16px;
        text-align: center;
        box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
        transition: transform 0.2s ease;
    }

    .use-case-card:hover {
        transform: translateY(-4px);
    }

    .use-case-icon {
        font-size: 48px;
        margin-bottom: 20px;
    }

    .use-case-card h3 {
        font-size: 20px;
        margin-bottom: 12px;
        color: var(--text-dark);
    }

    .use-case-card p {
        color: var(--text-light);
        font-size: 15px;
    }

    /* Get Started */
    .get-started {
        background: white;
        text-align: center;
    }

    .get-started h2 {
        font-size: 40px;
        margin-bottom: 16px;
        color: var(--text-dark);
    }

    .get-started-desc {
        color: var(--text-light);
        font-size: 18px;
        margin-bottom: 40px;
        max-width: 600px;
        margin-left: auto;
        margin-right: auto;
    }

    .get-started-action {
        max-width: 400px;
        margin: 0 auto;
    }

    .get-started-action .btn {
        width: 100%;
        padding: 20px 40px;
        font-size: 18px;
    }

    .get-started-note {
        margin-top: 16px;
        color: var(--text-light);
        font-size: 14px;
    }

    .guide-link {
        display: inline-block;
        margin-top: 16px;
        color: var(--accent-pink);
        font-weight: 600;
        font-size: 14px;
        transition: opacity 0.2s;
    }

    .guide-link:hover {
        opacity: 0.8;
    }

    /* Footer CTA */
    .footer-cta {
        background: var(--gradient-hero);
        color: white;
        text-align: center;
        padding: 100px 0;
    }

    .footer-cta h2 {
        font-size: 40px;
        margin-bottom: 32px;
    }

    .footer-cta .cta-buttons {
        align-items: center;
        margin-bottom: 48px;
    }

    .contact {
        padding-top: 32px;
        border-top: 1px solid rgba(255, 255, 255, 0.2);
    }

    .contact p {
        opacity: 0.8;
        margin-bottom: 8px;
    }

    .contact a {
        color: var(--accent-pink);
        font-weight: 500;
    }

    footer {
        background: #1a1a1a;
        color: rgba(255, 255, 255, 0.6);
        padding: 24px 0;
        text-align: center;
        font-size: 14px;
    }

    /* Responsive */
    @media (max-width: 1024px) {
        .hero-content {
            grid-template-columns: 1fr;
            text-align: center;
        }

        .hero-text {
            order: 1;
        }

        .hero-image {
            order: 2;
            min-height: 320px;
        }

        .hero-back {
            transform: rotate(-4deg) translate(-40px, 15px);
        }

        .hero-back img {
            width: 280px;
        }

        .hero-front {
            transform: translate(40px, -5px);
        }

        .app-frame img {
            width: 320px;
        }

        .cta-buttons {
            align-items: center;
        }

        .steps {
            flex-direction: column;
            align-items: center;
        }

        .step-connector {
            width: 2px;
            height: 40px;
            margin: 0;
        }

        .features-grid,
        .use-cases-grid {
            grid-template-columns: 1fr;
        }
    }

    @media (max-width: 768px) {
        .hero h1 {
            font-size: 36px;
        }

        .subhead {
            font-size: 16px;
        }

        section {
            padding: 60px 0;
        }

        .how-it-works h2,
        .video-demo h2,
        .features h2,
        .use-cases h2,
        .get-started h2,
        .footer-cta h2 {
            font-size: 28px;
        }
    }
</style>
