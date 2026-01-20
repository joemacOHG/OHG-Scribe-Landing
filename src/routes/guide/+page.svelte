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

    const downloadUrl = $derived(isMac ? macDownloadUrl : winDownloadUrl);
</script>

<div class="guide-page">
    <!-- Hero -->
    <section class="guide-hero">
        <div class="container">
            <a href="/" class="back-link">← Back to Home</a>
            <h1>Getting Started with OHG Scribe</h1>
            <p class="hero-subtitle">
                Everything you need to know to start transcribing like a pro
            </p>
        </div>
    </section>

    <!-- Table of Contents -->
    <nav class="toc">
        <div class="container">
            <h2>Quick Links</h2>
            <div class="toc-grid">
                <a href="#api-setup" class="toc-item">
                    <span class="toc-icon">🔑</span>
                    <span>API Key Setup</span>
                </a>
                <a href="#openai-key" class="toc-item">
                    <span class="toc-icon">🤖</span>
                    <span>OpenAI Key</span>
                </a>
                <a href="#speakers" class="toc-item">
                    <span class="toc-icon">👥</span>
                    <span>Speaker Identification</span>
                </a>
                <a href="#vocabulary" class="toc-item">
                    <span class="toc-icon">📚</span>
                    <span>Medical Vocabulary</span>
                </a>
                <a href="#extraction" class="toc-item">
                    <span class="toc-icon">✨</span>
                    <span>Smart Extraction</span>
                </a>
                <a href="#analysis" class="toc-item">
                    <span class="toc-icon">📊</span>
                    <span>Analysis Options</span>
                </a>
            </div>
        </div>
    </nav>

    <!-- API Setup Section -->
    <section id="api-setup" class="guide-section">
        <div class="container">
            <div class="section-header">
                <span class="section-number">01</span>
                <h2>AssemblyAI API Key Setup</h2>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        OHG Scribe uses AssemblyAI for high-accuracy
                        transcription. You'll need an API key to get started.
                    </p>

                    <div class="steps-list">
                        <div class="step-item">
                            <span class="step-num">1</span>
                            <div class="step-content">
                                <h4>Request your API key</h4>
                                <p>
                                    Contact us to receive your team's AssemblyAI
                                    API key.
                                </p>
                                <a
                                    href="mailto:assemblyai@openhealthgroup.com?subject=OHG%20Scribe%20API%20Key%20Request"
                                    class="btn btn-secondary"
                                >
                                    Request API Key →
                                </a>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">2</span>
                            <div class="step-content">
                                <h4>Open Settings</h4>
                                <p>
                                    Click the gear icon in the top-right corner
                                    of OHG Scribe.
                                </p>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">3</span>
                            <div class="step-content">
                                <h4>Enter your key</h4>
                                <p>
                                    Paste your API key into the "AssemblyAI API
                                    Key" field and click Save.
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-image">
                    <div class="placeholder-image">
                        <span>📷</span>
                        <p>Settings screenshot coming soon</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- OpenAI Key Section -->
    <section id="openai-key" class="guide-section alt-bg">
        <div class="container">
            <div class="section-header">
                <span class="section-number">02</span>
                <h2>OpenAI API Key (Optional)</h2>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        An OpenAI key enables advanced features like AI speaker
                        name inference, document vocabulary extraction, and
                        summary generation.
                    </p>

                    <div class="steps-list">
                        <div class="step-item">
                            <span class="step-num">1</span>
                            <div class="step-content">
                                <h4>Create an OpenAI account</h4>
                                <p>
                                    Visit OpenAI's platform and sign up for an
                                    account.
                                </p>
                                <a
                                    href="https://platform.openai.com/signup"
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    class="btn btn-secondary"
                                >
                                    Create OpenAI Account →
                                </a>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">2</span>
                            <div class="step-content">
                                <h4>Generate an API key</h4>
                                <p>
                                    Go to the API Keys section and create a new
                                    secret key.
                                </p>
                                <a
                                    href="https://platform.openai.com/api-keys"
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    class="btn btn-secondary"
                                >
                                    Go to API Keys →
                                </a>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">3</span>
                            <div class="step-content">
                                <h4>Add credits (if needed)</h4>
                                <p>
                                    OpenAI requires pre-paid credits. $5-10 is
                                    plenty for extensive use.
                                </p>
                                <a
                                    href="https://platform.openai.com/settings/organization/billing/overview"
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    class="btn btn-secondary"
                                >
                                    Add Billing →
                                </a>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">4</span>
                            <div class="step-content">
                                <h4>Enter in OHG Scribe</h4>
                                <p>
                                    Paste your key in Settings → "OpenAI API
                                    Key" field.
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="info-callout">
                        <span class="callout-icon">💡</span>
                        <div>
                            <strong>Good to know:</strong> OpenAI charges per usage.
                            Typical costs are $0.01-0.05 per transcription for AI
                            features. Your key stays on your device.
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Speakers Section -->
    <section id="speakers" class="guide-section">
        <div class="container">
            <div class="section-header">
                <span class="section-number">03</span>
                <h2>Speaker Identification</h2>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        OHG Scribe can automatically identify and label
                        different speakers in your recordings — perfect for
                        interviews, panels, and advisory boards.
                    </p>

                    <h3>Presets</h3>
                    <p>Quick-start with common labeling schemes:</p>

                    <div class="preset-grid">
                        <div class="preset-item">
                            <strong>Interview</strong>
                            <span>Interviewer / Interviewee</span>
                        </div>
                        <div class="preset-item">
                            <strong>Panel</strong>
                            <span>Moderator / Panelist A, B, C...</span>
                        </div>
                        <div class="preset-item">
                            <strong>Advisory Board</strong>
                            <span>Facilitator / Advisor 1, 2, 3...</span>
                        </div>
                        <div class="preset-item">
                            <strong>Custom</strong>
                            <span>Define your own labels</span>
                        </div>
                    </div>

                    <h3>AI Speaker Names</h3>
                    <p>
                        With an OpenAI key, OHG Scribe can infer real speaker
                        names from the conversation context — no pre-training
                        required. If someone says "Thank you, Dr. Smith," the AI
                        will pick up on that.
                    </p>
                </div>
                <div class="card-image">
                    <div class="placeholder-image">
                        <span>👥</span>
                        <p>Speaker options screenshot coming soon</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Vocabulary Section -->
    <section id="vocabulary" class="guide-section alt-bg">
        <div class="container">
            <div class="section-header">
                <span class="section-number">04</span>
                <h2>Medical Vocabulary</h2>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        Boost transcription accuracy for specialized terminology
                        by enabling medical vocabularies. OHG Scribe includes
                        curated word lists for major specialties.
                    </p>

                    <h3>Built-in Specialties</h3>
                    <div class="vocab-grid">
                        <span class="vocab-tag">Cardiology</span>
                        <span class="vocab-tag">Neurology</span>
                        <span class="vocab-tag">Oncology</span>
                        <span class="vocab-tag">Immunology</span>
                        <span class="vocab-tag">Endocrinology</span>
                        <span class="vocab-tag">Pulmonology</span>
                        <span class="vocab-tag">+ more</span>
                    </div>

                    <h3>Custom Vocabularies</h3>
                    <p>
                        Create your own word lists with product names, acronyms,
                        or trial-specific terminology. Terms are saved locally
                        for reuse.
                    </p>
                </div>
                <div class="card-image">
                    <div class="placeholder-image">
                        <span>📚</span>
                        <p>Vocabulary panel screenshot coming soon</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Smart Extraction Section -->
    <section id="extraction" class="guide-section featured">
        <div class="container">
            <div class="section-header">
                <span class="section-number">05</span>
                <h2>Smart Vocabulary Extraction</h2>
                <span class="feature-badge">✨ Highlight Feature</span>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        Don't manually type terminology — let AI extract it from
                        your documents. Upload an agenda, protocol, or product
                        sheet, and OHG Scribe will identify relevant terms
                        automatically.
                    </p>

                    <h3>How It Works</h3>
                    <div class="steps-list">
                        <div class="step-item">
                            <span class="step-num">1</span>
                            <div class="step-content">
                                <h4>Upload a document</h4>
                                <p>
                                    Supports Word (.docx), PDF, Plain Text
                                    (.txt), and Markdown (.md)
                                </p>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">2</span>
                            <div class="step-content">
                                <h4>AI analyzes content</h4>
                                <p>
                                    Identifies drug names, acronyms, medical
                                    terms, and proper nouns
                                </p>
                            </div>
                        </div>
                        <div class="step-item">
                            <span class="step-num">3</span>
                            <div class="step-content">
                                <h4>Review and add</h4>
                                <p>
                                    Choose which terms to add to your
                                    transcription vocabulary
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="info-callout success">
                        <span class="callout-icon">💡</span>
                        <div>
                            <strong>Pro tip:</strong> Upload your advisory board
                            agenda before the meeting starts. The AI will prepare
                            a vocabulary list with all the drugs, companies, and
                            clinical terms mentioned — improving accuracy for the
                            actual recording.
                        </div>
                    </div>
                </div>
                <div class="card-image">
                    <div class="placeholder-image featured">
                        <span>✨</span>
                        <p>Document extraction screenshot coming soon</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Analysis Section -->
    <section id="analysis" class="guide-section alt-bg">
        <div class="container">
            <div class="section-header">
                <span class="section-number">06</span>
                <h2>Analysis Options</h2>
            </div>
            <div class="content-card">
                <div class="card-content">
                    <p>
                        Beyond transcription, OHG Scribe can analyze your
                        content with AI-powered insights.
                    </p>

                    <div class="analysis-grid">
                        <div class="analysis-item">
                            <div class="analysis-icon">📝</div>
                            <h4>Summary</h4>
                            <p>
                                Get a concise overview of the discussion points
                                and key takeaways.
                            </p>
                        </div>
                        <div class="analysis-item">
                            <div class="analysis-icon">😊</div>
                            <h4>Sentiment Analysis</h4>
                            <p>
                                Understand the emotional tone of each speaker's
                                contributions.
                            </p>
                        </div>
                        <div class="analysis-item">
                            <div class="analysis-icon">🏷️</div>
                            <h4>Topic Detection</h4>
                            <p>
                                Automatically categorize and tag discussion
                                topics.
                            </p>
                        </div>
                    </div>

                    <p class="analysis-note">
                        <em
                            >Analysis options are configured before starting
                            transcription and appear in the exported Word
                            document.</em
                        >
                    </p>
                </div>
            </div>
        </div>
    </section>

    <!-- Download CTA -->
    <section class="guide-cta">
        <div class="container">
            <h2>Ready to Start?</h2>
            <p>Download OHG Scribe and start transcribing in minutes.</p>
            <a href={downloadUrl} class="btn btn-primary">
                {isMac ? "Download for Mac" : "Download for Windows"}
            </a>
            <p class="cta-note">
                Questions? <a href="mailto:joestevens@openhealthgroup.com"
                    >Contact us</a
                >
            </p>
        </div>
    </section>
</div>

<style>
    .guide-page {
        min-height: 100vh;
    }

    /* Hero */
    .guide-hero {
        background: var(--gradient-hero);
        color: white;
        padding: 60px 0 80px;
    }

    .back-link {
        display: inline-block;
        margin-bottom: 24px;
        opacity: 0.8;
        font-size: 14px;
        transition: opacity 0.2s;
    }

    .back-link:hover {
        opacity: 1;
    }

    .guide-hero h1 {
        font-size: 48px;
        margin-bottom: 16px;
    }

    .hero-subtitle {
        font-size: 20px;
        opacity: 0.9;
        max-width: 600px;
    }

    /* Table of Contents */
    .toc {
        background: white;
        padding: 40px 0;
        border-bottom: 1px solid #e2e8f0;
        position: sticky;
        top: 0;
        z-index: 100;
    }

    .toc h2 {
        font-size: 14px;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        color: var(--text-light);
        margin-bottom: 16px;
    }

    .toc-grid {
        display: flex;
        flex-wrap: wrap;
        gap: 12px;
    }

    .toc-item {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 8px 16px;
        background: var(--bg-light);
        border-radius: 8px;
        font-size: 14px;
        font-weight: 500;
        transition: all 0.2s;
    }

    .toc-item:hover {
        background: var(--gradient-accent);
        color: white;
    }

    .toc-icon {
        font-size: 16px;
    }

    /* Section Styles */
    .guide-section {
        padding: 80px 0;
    }

    .guide-section.alt-bg {
        background: var(--bg-light);
    }

    .guide-section.featured {
        background: linear-gradient(135deg, #fdf4ff 0%, #fce7f3 100%);
    }

    .section-header {
        display: flex;
        align-items: center;
        gap: 16px;
        margin-bottom: 32px;
    }

    .section-number {
        font-size: 14px;
        font-weight: 700;
        color: var(--accent-pink);
        background: rgba(236, 72, 153, 0.1);
        padding: 4px 12px;
        border-radius: 20px;
    }

    .section-header h2 {
        font-size: 32px;
        color: var(--text-dark);
    }

    .feature-badge {
        font-size: 12px;
        font-weight: 600;
        color: white;
        background: var(--gradient-accent);
        padding: 4px 12px;
        border-radius: 20px;
    }

    /* Content Cards */
    .content-card {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 48px;
        align-items: start;
    }

    .card-content h3 {
        font-size: 18px;
        margin: 24px 0 12px;
        color: var(--text-dark);
    }

    .card-content p {
        color: var(--text-light);
        line-height: 1.7;
    }

    .card-image {
        position: sticky;
        top: 140px;
    }

    .placeholder-image {
        aspect-ratio: 4 / 3;
        background: linear-gradient(135deg, #e2e8f0 0%, #cbd5e1 100%);
        border-radius: 12px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        color: var(--text-light);
    }

    .placeholder-image.featured {
        background: linear-gradient(135deg, #f0abfc 0%, #f472b6 100%);
        color: white;
    }

    .placeholder-image span {
        font-size: 48px;
        margin-bottom: 12px;
    }

    .placeholder-image p {
        font-size: 14px;
    }

    /* Steps List */
    .steps-list {
        margin: 24px 0;
    }

    .step-item {
        display: flex;
        gap: 16px;
        padding: 20px 0;
        border-bottom: 1px solid #e2e8f0;
    }

    .step-item:last-child {
        border-bottom: none;
    }

    .step-num {
        width: 32px;
        height: 32px;
        background: var(--gradient-accent);
        color: white;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: 700;
        font-size: 14px;
        flex-shrink: 0;
    }

    .step-content h4 {
        font-size: 16px;
        margin-bottom: 4px;
        color: var(--text-dark);
    }

    .step-content p {
        font-size: 14px;
        margin-bottom: 12px;
    }

    /* Buttons */
    .btn {
        display: inline-flex;
        align-items: center;
        padding: 12px 24px;
        border-radius: 8px;
        font-size: 14px;
        font-weight: 600;
        transition: all 0.2s;
        text-decoration: none;
    }

    .btn-primary {
        background: var(--gradient-accent);
        color: white;
        box-shadow: 0 4px 16px rgba(236, 72, 153, 0.3);
    }

    .btn-primary:hover {
        transform: translateY(-2px);
        box-shadow: 0 6px 20px rgba(236, 72, 153, 0.4);
    }

    .btn-secondary {
        background: white;
        color: var(--accent-pink);
        border: 1px solid var(--accent-pink);
    }

    .btn-secondary:hover {
        background: var(--accent-pink);
        color: white;
    }

    /* Info Callout */
    .info-callout {
        display: flex;
        gap: 12px;
        padding: 16px;
        background: rgba(59, 130, 246, 0.1);
        border-radius: 8px;
        margin-top: 24px;
        font-size: 14px;
    }

    .info-callout.success {
        background: rgba(34, 197, 94, 0.1);
    }

    .callout-icon {
        font-size: 20px;
    }

    /* Preset Grid */
    .preset-grid {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 12px;
        margin: 16px 0;
    }

    .preset-item {
        padding: 16px;
        background: white;
        border-radius: 8px;
        border: 1px solid #e2e8f0;
    }

    .preset-item strong {
        display: block;
        color: var(--text-dark);
        margin-bottom: 4px;
    }

    .preset-item span {
        font-size: 13px;
        color: var(--text-light);
    }

    /* Vocabulary Tags */
    .vocab-grid {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        margin: 12px 0 24px;
    }

    .vocab-tag {
        padding: 6px 14px;
        background: white;
        border-radius: 20px;
        font-size: 13px;
        font-weight: 500;
        color: var(--accent-magenta);
        border: 1px solid var(--accent-magenta);
    }

    /* Analysis Grid */
    .analysis-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 24px;
        margin: 24px 0;
    }

    .analysis-item {
        padding: 24px;
        background: white;
        border-radius: 12px;
        text-align: center;
    }

    .analysis-icon {
        font-size: 32px;
        margin-bottom: 12px;
    }

    .analysis-item h4 {
        font-size: 16px;
        margin-bottom: 8px;
        color: var(--text-dark);
    }

    .analysis-item p {
        font-size: 13px;
    }

    .analysis-note {
        font-size: 14px;
        text-align: center;
        margin-top: 16px;
    }

    /* CTA Section */
    .guide-cta {
        background: var(--gradient-hero);
        color: white;
        text-align: center;
        padding: 80px 0;
    }

    .guide-cta h2 {
        font-size: 36px;
        margin-bottom: 12px;
    }

    .guide-cta p {
        opacity: 0.9;
        margin-bottom: 24px;
    }

    .cta-note {
        margin-top: 16px;
        font-size: 14px;
        opacity: 0.8;
    }

    .cta-note a {
        text-decoration: underline;
    }

    /* Responsive */
    @media (max-width: 900px) {
        .content-card {
            grid-template-columns: 1fr;
        }

        .card-image {
            position: static;
            order: -1;
        }

        .guide-hero h1 {
            font-size: 36px;
        }

        .section-header h2 {
            font-size: 24px;
        }

        .preset-grid,
        .analysis-grid {
            grid-template-columns: 1fr;
        }

        .toc-grid {
            flex-direction: column;
        }

        .toc-item {
            justify-content: flex-start;
        }
    }
</style>
