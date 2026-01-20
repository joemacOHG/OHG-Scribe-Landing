<script lang="ts">
    import { onMount } from "svelte";

    let isMac = $state(true);
    let macDownloadUrl = $state(
        "https://github.com/joemacOHG/ohg-scribe/releases/latest",
    );
    let winDownloadUrl = $state(
        "https://github.com/joemacOHG/ohg-scribe/releases/latest",
    );

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
                        macDownloadUrl = asset.browser_download_url;
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

    // Computed download URL based on platform
    const downloadUrl = $derived(isMac ? macDownloadUrl : winDownloadUrl);
    const altDownloadUrl = $derived(isMac ? winDownloadUrl : macDownloadUrl);
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
                Turn advisory boards, interviews, and meetings into professional
                Word documents in minutes.
            </p>
            <div class="cta-buttons">
                <a href={downloadUrl} class="btn btn-primary">
                    {isMac ? "Download for Mac" : "Download for Windows"}
                </a>
                <a href={altDownloadUrl} class="btn-secondary">
                    Also available for {isMac ? "Windows" : "Mac"}
                </a>
            </div>
        </div>
        <div class="hero-image">
            <div class="macbook-frame">
                <img
                    src="/screenshots/01-main-interface-drag-drop.png"
                    alt="OHG Scribe interface"
                />
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
                    Drag any audio or video file — MP4, MP3, WAV, and 8 more
                    formats
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
                <p>AI processes your recording with 95%+ accuracy in minutes</p>
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
                    Download a formatted Word document with speaker labels and
                    timestamps
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
            <div class="video-placeholder">
                <button class="play-button">
                    <svg viewBox="0 0 24 24" fill="currentColor">
                        <polygon points="5,3 19,12 5,21" />
                    </svg>
                </button>
            </div>
            <p class="video-caption">Watch a 2-minute walkthrough</p>
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
                    Capture every expert opinion with accurate speaker
                    attribution across 10+ participants
                </p>
            </div>
            <div class="use-case-card">
                <div class="use-case-icon">🎤</div>
                <h3>KOL Interviews</h3>
                <p>
                    Get verbatim transcripts ready for medical-legal review —
                    formatted and time-stamped
                </p>
            </div>
            <div class="use-case-card">
                <div class="use-case-icon">📋</div>
                <h3>Patient Research</h3>
                <p>
                    Document qualitative interviews with optional sentiment
                    analysis and topic detection
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
            OHG Scribe requires an API key for transcription services. Request
            yours to start transcribing.
        </p>
        <div class="get-started-action">
            <a
                href="mailto:assemblyai@openhealthgroup.com?subject=OHG%20Scribe%20API%20Key%20Request&body=Hi%2C%0A%0AI%20would%20like%20to%20request%20an%20API%20key%20for%20OHG%20Scribe.%0A%0AName%3A%20%0AEmail%3A%20%0ATeam%2FDepartment%3A%20%0A%0AThank%20you!"
                class="btn btn-primary"
            >
                Request API Key
            </a>
            <p class="get-started-note">We'll send your key within 24 hours</p>
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
            <a href={altDownloadUrl} class="btn-secondary-light">
                Also available for {isMac ? "Windows" : "Mac"}
            </a>
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

    .hero-image {
        display: flex;
        justify-content: center;
    }

    .macbook-frame {
        background: #1a1a1a;
        border-radius: 12px;
        padding: 8px;
        box-shadow: 0 40px 80px rgba(0, 0, 0, 0.5);
        max-width: 100%;
    }

    .macbook-frame img {
        width: 100%;
        border-radius: 8px;
        display: block;
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

    .video-placeholder {
        aspect-ratio: 16 / 9;
        background: rgba(255, 255, 255, 0.1);
        border-radius: 16px;
        display: flex;
        align-items: center;
        justify-content: center;
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.2);
    }

    .play-button {
        width: 80px;
        height: 80px;
        background: var(--gradient-accent);
        border: none;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        cursor: pointer;
        transition: transform 0.2s ease;
    }

    .play-button:hover {
        transform: scale(1.1);
    }

    .play-button svg {
        width: 32px;
        height: 32px;
        margin-left: 4px;
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
