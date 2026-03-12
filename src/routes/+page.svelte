<script lang="ts">
    import { onMount } from "svelte";

    type OperatingSystem = "windows" | "macos" | "linux" | "unknown";
    type Architecture = "x86_64" | "aarch64" | "riscv64" | "unknown";
    type DownloadOption = {
        id: string;
        os: Exclude<OperatingSystem, "unknown">;
        arch: Exclude<Architecture, "unknown">;
        label: string;
        shortLabel: string;
        assetName: string;
        href: string;
    };
    type NavigatorUAData = {
        platform?: string;
        getHighEntropyValues?: (
            hints: string[],
        ) => Promise<{
            architecture?: string;
            bitness?: string;
            platform?: string;
        }>;
    };

    const REPO_URL = "https://github.com/nullclaw/nullhub";
    const LATEST_RELEASE_URL = `${REPO_URL}/releases/latest`;
    const LATEST_DOWNLOAD_URL = `${LATEST_RELEASE_URL}/download`;
    const GHCR_PACKAGE_URL =
        "https://github.com/nullclaw/nullhub/pkgs/container/nullhub";
    const GHCR_IMAGE = "ghcr.io/nullclaw/nullhub:latest";
    const DOCKER_RUN_COMMAND =
        "docker run --rm -p 19800:19800 -v nullhub-data:/nullhub-data ghcr.io/nullclaw/nullhub:latest";
    const DOCKER_PULL_COMMAND = `docker pull ${GHCR_IMAGE}`;
    const downloadOptions: DownloadOption[] = [
        {
            id: "macos-aarch64",
            os: "macos",
            arch: "aarch64",
            label: "macOS Apple Silicon",
            shortLabel: "macOS ARM64",
            assetName: "nullhub-macos-aarch64.bin",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-macos-aarch64.bin`,
        },
        {
            id: "macos-x86_64",
            os: "macos",
            arch: "x86_64",
            label: "macOS Intel",
            shortLabel: "macOS x64",
            assetName: "nullhub-macos-x86_64.bin",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-macos-x86_64.bin`,
        },
        {
            id: "windows-x86_64",
            os: "windows",
            arch: "x86_64",
            label: "Windows x64",
            shortLabel: "Windows x64",
            assetName: "nullhub-windows-x86_64.exe",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-windows-x86_64.exe`,
        },
        {
            id: "windows-aarch64",
            os: "windows",
            arch: "aarch64",
            label: "Windows ARM64",
            shortLabel: "Windows ARM64",
            assetName: "nullhub-windows-aarch64.exe",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-windows-aarch64.exe`,
        },
        {
            id: "linux-x86_64",
            os: "linux",
            arch: "x86_64",
            label: "Linux x64",
            shortLabel: "Linux x64",
            assetName: "nullhub-linux-x86_64.bin",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-linux-x86_64.bin`,
        },
        {
            id: "linux-aarch64",
            os: "linux",
            arch: "aarch64",
            label: "Linux ARM64",
            shortLabel: "Linux ARM64",
            assetName: "nullhub-linux-aarch64.bin",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-linux-aarch64.bin`,
        },
        {
            id: "linux-riscv64",
            os: "linux",
            arch: "riscv64",
            label: "Linux RISC-V",
            shortLabel: "Linux RISC-V",
            assetName: "nullhub-linux-riscv64.bin",
            href: `${LATEST_DOWNLOAD_URL}/nullhub-linux-riscv64.bin`,
        },
    ];

    let detectedOs = $state<OperatingSystem>("unknown");
    let detectedArch = $state<Architecture>("unknown");

    let recommendedDownload = $derived.by(() => {
        if (detectedOs === "unknown" || detectedArch === "unknown") {
            return null;
        }

        return (
            downloadOptions.find(
                (option) =>
                    option.os === detectedOs && option.arch === detectedArch,
            ) ?? null
        );
    });

    let prioritizedDownloads = $derived.by(() => {
        if (recommendedDownload) {
            return [
                recommendedDownload,
                ...downloadOptions.filter(
                    (option) => option.id !== recommendedDownload.id,
                ),
            ];
        }

        if (detectedOs === "unknown") {
            return downloadOptions;
        }

        return [
            ...downloadOptions.filter((option) => option.os === detectedOs),
            ...downloadOptions.filter((option) => option.os !== detectedOs),
        ];
    });

    let detectedLabel = $derived(formatPlatformLabel(detectedOs, detectedArch));

    onMount(() => {
        void detectClientPlatform();
    });

    async function detectClientPlatform() {
        const uaData = (
            navigator as Navigator & { userAgentData?: NavigatorUAData }
        ).userAgentData;
        const highEntropyValues = uaData?.getHighEntropyValues
            ? await uaData.getHighEntropyValues([
                    "architecture",
                    "bitness",
                    "platform",
                ])
            : undefined;

        const os = inferOperatingSystem(
            highEntropyValues?.platform ??
                uaData?.platform ??
                navigator.userAgent ??
                navigator.platform,
        );
        const arch = inferArchitecture({
            os,
            userAgent: navigator.userAgent,
            platform: navigator.platform,
            architecture: highEntropyValues?.architecture,
            bitness: highEntropyValues?.bitness,
        });

        detectedOs = os;
        detectedArch = arch;
    }

    function inferOperatingSystem(source: string) {
        const normalized = source.toLowerCase();

        if (normalized.includes("win")) {
            return "windows" satisfies OperatingSystem;
        }

        if (normalized.includes("mac") || normalized.includes("darwin")) {
            return "macos" satisfies OperatingSystem;
        }

        if (normalized.includes("linux") || normalized.includes("x11")) {
            return "linux" satisfies OperatingSystem;
        }

        return "unknown" satisfies OperatingSystem;
    }

    function inferArchitecture({
        os,
        userAgent,
        platform,
        architecture,
        bitness,
    }: {
        os: OperatingSystem;
        userAgent: string;
        platform: string;
        architecture?: string;
        bitness?: string;
    }) {
        const normalizedArchitecture = architecture?.toLowerCase() ?? "";
        const normalizedBitness = bitness?.toLowerCase() ?? "";

        if (normalizedArchitecture.includes("riscv")) {
            return "riscv64" satisfies Architecture;
        }

        if (
            normalizedArchitecture.includes("arm") &&
            normalizedBitness !== "32"
        ) {
            return "aarch64" satisfies Architecture;
        }

        if (
            normalizedArchitecture.includes("x86") &&
            normalizedBitness === "64"
        ) {
            return "x86_64" satisfies Architecture;
        }

        const normalizedSource = `${userAgent} ${platform}`.toLowerCase();

        if (normalizedSource.includes("riscv64")) {
            return "riscv64" satisfies Architecture;
        }

        if (
            normalizedSource.includes("arm64") ||
            normalizedSource.includes("aarch64") ||
            normalizedSource.includes("armv8")
        ) {
            return "aarch64" satisfies Architecture;
        }

        if (
            os === "windows" &&
            /(win64|wow64|x64|amd64|x86_64)/.test(normalizedSource)
        ) {
            return "x86_64" satisfies Architecture;
        }

        if (
            os === "linux" &&
            /(x86_64|amd64)/.test(normalizedSource)
        ) {
            return "x86_64" satisfies Architecture;
        }

        if (
            os === "macos" &&
            normalizedSource.includes("apple silicon")
        ) {
            return "aarch64" satisfies Architecture;
        }

        return "unknown" satisfies Architecture;
    }

    function formatOperatingSystem(os: OperatingSystem) {
        if (os === "windows") return "Windows";
        if (os === "macos") return "macOS";
        if (os === "linux") return "Linux";
        return "Unknown OS";
    }

    function formatArchitecture(arch: Architecture) {
        if (arch === "x86_64") return "x64";
        if (arch === "aarch64") return "ARM64";
        if (arch === "riscv64") return "RISC-V";
        return "Unknown CPU";
    }

    function formatPlatformLabel(os: OperatingSystem, arch: Architecture) {
        if (os === "unknown") {
            return "Unsupported browser detection";
        }

        if (arch === "unknown") {
            return formatOperatingSystem(os);
        }

        return `${formatOperatingSystem(os)} ${formatArchitecture(arch)}`;
    }
</script>

<svelte:head>
    <title>NullHub — Ecosystem Management Hub</title>
    <meta
        name="description"
        content="The simplest way to install, configure, and manage NullClaw, NullBoiler, NullTickets, and the rest of the autonomous AI ecosystem."
    />
</svelte:head>

<section class="hero">
    <div class="hero-inner">
        <p class="kicker">ECOSYSTEM CONTROL PLANE</p>
        <h1>The Simplest Way To Manage Your Autonomous AI Agents</h1>
        <p class="summary">
            One binary. One seamless interface. Deploy, configure, supervise,
            and update
            <a href="https://nullclaw.io" target="_blank"
                ><strong>NullClaw</strong></a
            >,
            <a href="/nullboiler"><strong>NullBoiler</strong></a>,
            <a href="/nulltickets"><strong>NullTickets</strong></a>, and
            additional ecosystem components—all from a unified high-performance
            dashboard or CLI.
        </p>

        <div class="hero-actions">
            <a href="#downloads" class="btn primary">Download Latest</a>
            <a href="#docker" class="btn secondary">Run with Docker</a>
            <a href="#quickstart" class="btn secondary">Quick Start</a>
            <a href="#features" class="btn secondary">Features</a>
            <a href="#cli" class="btn secondary">CLI Reference</a>
            <a
                href="https://github.com/nullclaw/nullhub"
                target="_blank"
                class="btn secondary">GitHub</a
            >
        </div>

        <div class="download-console" id="downloads">
            <div class="download-header">
                <div class="download-copy">
                    <div class="download-title-row">
                        <div>
                            <p class="download-kicker">INSTALL NULLHUB</p>
                            <h2>
                                {#if recommendedDownload}
                                    Download for {recommendedDownload.label}
                                {:else}
                                    Install from binary or Docker
                                {/if}
                            </h2>
                        </div>
                        <span class="download-pill">
                            {#if recommendedDownload}
                                Auto: {detectedLabel}
                            {:else if detectedOs !== "unknown"}
                                Detected: {detectedLabel}
                            {:else}
                                Latest GitHub Release
                            {/if}
                        </span>
                    </div>
                    <p class="download-description">
                        {#if recommendedDownload}
                            Native binary for <strong>{detectedLabel}</strong>
                            plus a ready-to-run Docker image from GHCR.
                        {:else if detectedOs !== "unknown"}
                            Native binaries for
                            <strong>{formatOperatingSystem(detectedOs)}</strong>,
                            plus a ready-to-run Docker image from GHCR.
                        {:else}
                            Native binaries for macOS, Windows, Linux, plus a
                            ready-to-run Docker image from GHCR.
                        {/if}
                    </p>

                    <div class="install-actions">
                        {#if recommendedDownload}
                            <a
                                href={recommendedDownload.href}
                                class="btn primary download-primary"
                            >
                                Download {recommendedDownload.shortLabel}
                            </a>
                        {:else}
                            <a
                                href={LATEST_RELEASE_URL}
                                target="_blank"
                                rel="noreferrer"
                                class="btn primary download-primary"
                            >
                                Open Latest Release
                            </a>
                        {/if}
                        <a
                            href={GHCR_PACKAGE_URL}
                            target="_blank"
                            rel="noreferrer"
                            class="btn secondary"
                        >
                            GHCR Package
                        </a>
                    </div>
                </div>
            </div>

            <div class="package-command" id="docker">
                <span class="package-command-label">docker run</span>
                <code>{DOCKER_RUN_COMMAND}</code>
            </div>

            <details class="install-details">
                <summary>All binaries and Docker commands</summary>
                <div class="install-detail-grid">
                    {#each prioritizedDownloads as option}
                        <a
                            href={option.href}
                            class="install-link"
                            class:selected={recommendedDownload?.id === option.id}
                        >
                            <span>{option.label}</span>
                            <code>{option.assetName}</code>
                        </a>
                    {/each}
                </div>

                <div class="install-detail-actions">
                    <a
                        href={LATEST_RELEASE_URL}
                        target="_blank"
                        rel="noreferrer"
                        class="mini-link"
                    >
                        View all release assets
                    </a>
                    <a
                        href={GHCR_PACKAGE_URL}
                        target="_blank"
                        rel="noreferrer"
                        class="mini-link"
                    >
                        Open GHCR package
                    </a>
                </div>

                <div class="package-command">
                    <span class="package-command-label">docker pull</span>
                    <code>{DOCKER_PULL_COMMAND}</code>
                </div>

                <p class="download-note">
                    Docker image: <code>{GHCR_IMAGE}</code>. On macOS/Linux,
                    native <code>.bin</code> downloads require
                    <code>chmod +x</code> before first launch.
                </p>
            </details>
        </div>

        <div class="hero-metrics">
            <article>
                <h3>3</h3>
                <p>managed ecosystem components</p>
            </article>
            <article>
                <h3>16</h3>
                <p>CLI commands for automation</p>
            </article>
            <article>
                <h3>1</h3>
                <p>single binary — Zig + embedded Svelte UI</p>
            </article>
            <article>
                <h3>∞</h3>
                <p>multi-instance: run N copies side by side</p>
            </article>
        </div>
    </div>
</section>

<section class="features" id="features">
    <div class="section-title">Features</div>
    <div class="feature-grid">
        <article>
            <h3>Install Wizard</h3>
            <p>
                Manifest-driven setup wizard with intelligent component linking
                (e.g., NullTickets to NullBoiler). Fully accessible via browser
                or terminal.
            </p>
        </article>
        <article>
            <h3>Process Supervision</h3>
            <p>
                Robust lifecycle management: start, stop, restart, and automatic
                crash recovery with exponential backoff. The hub daemon
                supervises all ecosystem instances.
            </p>
        </article>
        <article>
            <h3>Health Monitoring</h3>
            <p>
                Continuous HTTP health probing, real-time dashboard status
                cards, and live state updates delivered via Server-Sent Events
                (SSE).
            </p>
        </article>
        <article>
            <h3>Config Management</h3>
            <p>
                Intuitive structured editors for NullClaw, NullBoiler, and
                NullTickets configurations, with a raw JSON fallback layer for
                advanced users.
            </p>
        </article>
        <article>
            <h3>Log Viewing</h3>
            <p>
                Real-time log tailing and streaming per instance. Analyze logs
                directly within the browser dashboard or follow them from your
                terminal.
            </p>
        </article>
        <article>
            <h3>One-Click Updates</h3>
            <p>
                Seamlessly download new component versions, automatically
                migrate configurations, and safely rollback on failure. Update
                individually or simultaneously.
            </p>
        </article>
        <article>
            <h3>Cross-Component Linking</h3>
            <p>
                Automatically bind dependent components (NullTickets →
                NullBoiler), configure trackers natively, and inspect
                orchestrator queues from a unified UI.
            </p>
        </article>
        <article>
            <h3>Web UI + CLI</h3>
            <p>
                A premium browser dashboard for human operators, paired with a
                powerful CLI for automation scripts—both offering complete
                feature parity.
            </p>
        </article>
    </div>
</section>

<section class="architecture" id="architecture">
    <div class="section-title">Architecture</div>
    <div class="arch-grid">
        <article>
            <h3>Zig Backend</h3>
            <p>
                High-performance HTTP server, robust process supervisor,
                installer, and manifest engine. Runs in long-lived server mode
                or transient CLI mode.
            </p>
        </article>
        <article>
            <h3>Svelte Frontend</h3>
            <p>
                Statically generated SvelteKit application embedded directly
                into the binary overhead-free. Modular UI components load
                dynamically using Svelte 5.
            </p>
        </article>
        <article>
            <h3>Manifest-Driven</h3>
            <p>
                Ecosystem components publish declarative manifests specifying
                their setup, configuration, and health parameters. NullHub acts
                as the universal interpreter.
            </p>
        </article>
        <article>
            <h3>Local Storage</h3>
            <p>
                Zero external database required. All state—configurations,
                runtime instances, binaries, logs, and manifest caches—persists
                neatly in a local directory.
            </p>
        </article>
    </div>
</section>

<section class="components" id="components">
    <div class="section-title">Managed Components</div>
    <div class="component-grid">
        <article class="component active">
            <div class="top">
                <span class="tag">MANAGED</span>
                <span class="name">Core Runtime</span>
            </div>
            <h3>NullClaw</h3>
            <p>
                The fastest, most lightweight, and fully autonomous AI agent
                execution runtime written from the ground up in Zig.
            </p>
            <ul>
                <li>Providers, channels, tools, memory, policy</li>
                <li>Terminal and gateway operation modes</li>
                <li>Execution runtime for worker agents</li>
            </ul>
            <div class="actions">
                <a
                    href="https://nullclaw.io/nullclaw/docs/getting-started"
                    target="_blank">Docs</a
                >
                <a href="https://github.com/nullclaw/nullclaw" target="_blank"
                    >GitHub</a
                >
            </div>
        </article>

        <article class="component active">
            <div class="top">
                <span class="tag">MANAGED</span>
                <span class="name">Orchestration</span>
            </div>
            <h3>NullBoiler</h3>
            <p>
                Advanced workflow orchestrator handling multi-step agent
                executions with manual approvals, automatic retries, and
                intelligent worker dispatch.
            </p>
            <ul>
                <li>DAG execution with run/step/events APIs</li>
                <li>Worker registry + protocol-aware dispatch</li>
                <li>
                    Advanced steps: wait, router, loop, saga, debate, group_chat
                </li>
            </ul>
            <div class="actions">
                <a href="/nullboiler/docs/quick-start" target="_blank">Docs</a>
                <a href="https://github.com/nullclaw/nullboiler" target="_blank"
                    >GitHub</a
                >
            </div>
        </article>

        <article class="component active">
            <div class="top">
                <span class="tag">MANAGED</span>
                <span class="name">Task Plane</span>
            </div>
            <h3>NullTickets</h3>
            <p>
                Comprehensive execution-state backend managing task pipelines,
                leases, requisite gates, dependencies, and generated artifacts.
            </p>
            <ul>
                <li>Role-based claim/lease loop for autonomous agents</li>
                <li>Pipeline transitions with required gates</li>
                <li>Operational queue analytics and OTLP ingest</li>
            </ul>
            <div class="actions">
                <a href="/nulltickets/docs/quick-start" target="_blank">Docs</a>
                <a
                    href="https://github.com/nullclaw/nulltickets"
                    target="_blank">GitHub</a
                >
            </div>
        </article>
    </div>
</section>

<section class="cli-section" id="cli">
    <div class="section-title">CLI Reference</div>
    <div class="panel">
        <pre><code
                >nullhub                          # Start server + open browser
nullhub serve [--port N]         # Start server without browser

nullhub install &lt;component&gt;      # Terminal wizard
nullhub uninstall &lt;c&gt;/&lt;n&gt;        # Remove instance

nullhub start &lt;c&gt;/&lt;n&gt;            # Start instance
nullhub stop &lt;c&gt;/&lt;n&gt;             # Stop instance
nullhub restart &lt;c&gt;/&lt;n&gt;          # Restart instance
nullhub start-all / stop-all     # Bulk start/stop

nullhub status                   # Table of all instances
nullhub status &lt;c&gt;/&lt;n&gt;           # Single instance detail
nullhub logs &lt;c&gt;/&lt;n&gt; [-f]        # Tail logs (-f for follow)

nullhub check-updates            # Check for new versions
nullhub update &lt;c&gt;/&lt;n&gt;           # Update single instance
nullhub update-all               # Update everything

nullhub config &lt;c&gt;/&lt;n&gt; [--edit]  # View/edit config
nullhub service install          # Register as OS service (systemd/launchd)
nullhub version                  # Print version</code
            ></pre>
    </div>
    <p class="cli-note">
        Instance addressing uses <code
            >{"{"}component{"}"}/{"{"}instance-name{"}"}</code
        > everywhere.
    </p>
</section>

<section class="quickstart" id="quickstart">
    <div class="section-title">Quick Start</div>
    <div class="panel">
        <pre><code
                ># Clone and build NullHub
git clone https://github.com/nullclaw/nullhub.git
cd nullhub
zig build

# Launch — opens browser to http://nullhub.localhost:19800
./zig-out/bin/nullhub

# Or install a component directly from the CLI
./zig-out/bin/nullhub install nullclaw
./zig-out/bin/nullhub install nullboiler
./zig-out/bin/nullhub install nulltickets

# Start everything
./zig-out/bin/nullhub start-all

# Check status
./zig-out/bin/nullhub status</code
            ></pre>
    </div>
</section>

<section class="layout-section">
    <div class="section-title">Project Layout</div>
    <div class="panel">
        <pre><code
                >src/
  main.zig              # Entry: CLI dispatch or server start
  cli.zig               # CLI command parser & handlers
  server.zig            # HTTP server (API + static UI)
  auth.zig              # Optional bearer token auth
  api/                  # REST endpoints (components, instances, wizard, ...)
  core/                 # Manifest parser, state, platform, paths
  installer/            # Download, build, UI module fetching
  supervisor/           # Process spawn, health checks, manager
  wizard/               # Manifest wizard engine, config writer
ui/src/
  routes/               # SvelteKit pages (dashboard, install, instances, settings)
  lib/components/       # Reusable Svelte components
  lib/api/              # Typed API client
  lib/stores/           # Reactive state (instances, hub config)
tests/
  test_e2e.sh           # End-to-end test script</code
            ></pre>
    </div>
</section>

<section class="tech-stack">
    <div class="section-title">Tech Stack</div>
    <div class="stack-grid">
        <article>
            <h3>Zig 0.15.2</h3>
            <p>
                Backend runtime, HTTP server, process supervisor, and native
                build system.
            </p>
        </article>
        <article>
            <h3>Svelte 5 + SvelteKit</h3>
            <p>
                Frontend with static adapter, embedded into the binary at
                compile time.
            </p>
        </article>
        <article>
            <h3>JSON over HTTP/1.1</h3>
            <p>
                REST API for all control operations. SSE for live streaming logs
                and status.
            </p>
        </article>
    </div>
</section>

<style>
    .hero,
    .features,
    .architecture,
    .components,
    .cli-section,
    .quickstart,
    .layout-section,
    .tech-stack {
        width: min(1180px, 100% - 48px);
        margin: 0 auto;
    }

    .hero {
        padding: 72px 0 52px;
    }

    .hero-inner {
        position: relative;
        border: 1px solid var(--border);
        background: radial-gradient(
                circle at 85% 0%,
                color-mix(in srgb, var(--accent) 15%, transparent),
                transparent 45%
            ),
            var(--bg-surface);
        padding: 40px;
        border-radius: 16px;
        backdrop-filter: blur(16px);
        -webkit-backdrop-filter: blur(16px);
        box-shadow:
            0 8px 32px rgba(0, 0, 0, 0.4),
            inset 0 1px 0 rgba(255, 255, 255, 0.05);
        overflow: hidden;
    }

    .hero-inner::before {
        content: "";
        position: absolute;
        inset: 0;
        background: linear-gradient(
            135deg,
            transparent,
            rgba(255, 255, 255, 0.02) 40%,
            transparent
        );
        pointer-events: none;
    }

    .kicker {
        color: var(--accent-dim);
        font-size: 0.8rem;
        letter-spacing: 0.2em;
        margin-bottom: 14px;
    }

    h1 {
        margin: 0;
        font-size: clamp(2rem, 4.5vw, 3.5rem);
        line-height: 1.1;
        max-width: 20ch;
        text-wrap: balance;
        background: linear-gradient(180deg, var(--fg) 0%, var(--fg-dim) 100%);
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
        text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
    }

    .summary {
        color: var(--fg-dim);
        max-width: 76ch;
        margin: 16px 0 0;
        line-height: 1.7;
    }

    .summary strong {
        color: var(--fg);
    }

    .hero-actions {
        display: flex;
        gap: 12px;
        flex-wrap: wrap;
        margin: 24px 0 26px;
    }

    .btn {
        text-decoration: none;
        padding: 10px 16px;
        border-radius: 8px;
        border: 1px solid var(--border);
        font-weight: 700;
        font-size: 0.85rem;
        letter-spacing: 0.05em;
        text-transform: uppercase;
    }

    .btn.primary {
        background: color-mix(in srgb, var(--accent) 18%, transparent);
        color: var(--fg);
    }

    .btn.secondary {
        color: var(--accent);
    }

    .btn:hover {
        border-color: var(--accent);
        box-shadow: 0 0 12px var(--border-glow);
    }

    .download-console {
        margin: 0 0 28px;
        padding: 16px;
        border: 1px solid var(--border);
        border-radius: 14px;
        background:
            linear-gradient(
                140deg,
                color-mix(in srgb, var(--accent) 10%, transparent),
                transparent 55%
            ),
            color-mix(in srgb, var(--bg) 55%, transparent);
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.04);
        display: grid;
        gap: 14px;
    }

    .download-header {
        display: block;
    }

    .download-copy {
        display: grid;
        gap: 12px;
    }

    .download-kicker {
        color: var(--accent-dim);
        font-size: 0.76rem;
        letter-spacing: 0.16em;
        margin: 0 0 8px;
    }

    .download-title-row {
        display: flex;
        gap: 12px;
        justify-content: space-between;
        align-items: center;
        flex-wrap: wrap;
    }

    .download-title-row h2 {
        margin: 0;
        font-size: clamp(1.05rem, 2vw, 1.45rem);
        line-height: 1.2;
    }

    .download-description {
        margin: 0;
        max-width: 70ch;
        color: var(--fg-dim);
        font-size: 0.9rem;
    }

    .download-description strong {
        color: var(--fg);
    }

    .download-pill {
        border: 1px solid var(--border);
        border-radius: 999px;
        padding: 8px 12px;
        font-size: 0.78rem;
        white-space: nowrap;
        color: var(--accent);
        background: color-mix(in srgb, var(--bg) 50%, transparent);
    }

    .install-actions {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
    }

    .download-primary {
        color: var(--fg);
    }

    .package-command code,
    .install-link code,
    .download-note code {
        color: var(--accent);
    }

    .package-command {
        display: flex;
        align-items: center;
        gap: 10px;
        padding: 10px 12px;
        border: 1px dashed var(--border);
        border-radius: 10px;
        background: color-mix(in srgb, var(--bg) 78%, transparent);
        overflow-x: auto;
    }

    .package-command-label {
        color: var(--fg);
        font-size: 0.78rem;
        text-transform: uppercase;
        letter-spacing: 0.08em;
        white-space: nowrap;
    }

    .package-command code {
        white-space: nowrap;
        font-size: 0.85rem;
    }

    .install-details {
        border-top: 1px solid var(--border);
        padding-top: 12px;
    }

    .install-details summary {
        cursor: pointer;
        color: var(--accent);
        list-style: none;
        font-size: 0.88rem;
        user-select: none;
    }

    .install-details summary::-webkit-details-marker {
        display: none;
    }

    .install-details summary::before {
        content: "+";
        display: inline-block;
        margin-right: 8px;
    }

    .install-details[open] summary::before {
        content: "-";
    }

    .install-detail-grid {
        margin-top: 12px;
        display: grid;
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 8px;
    }

    .install-link {
        display: grid;
        gap: 4px;
        padding: 10px 12px;
        border: 1px solid var(--border);
        border-radius: 10px;
        text-decoration: none;
        background: color-mix(in srgb, var(--bg) 64%, transparent);
        transition:
            border-color 0.25s ease,
            transform 0.25s ease,
            box-shadow 0.25s ease;
    }

    .install-link:hover,
    .install-link.selected {
        border-color: var(--accent);
        box-shadow: 0 0 16px color-mix(in srgb, var(--accent) 20%, transparent);
        transform: translateY(-1px);
    }

    .install-link span {
        color: var(--fg);
        font-size: 0.9rem;
        line-height: 1.3;
    }

    .install-link code {
        font-size: 0.72rem;
        word-break: break-all;
    }

    .install-detail-actions {
        display: flex;
        gap: 12px;
        flex-wrap: wrap;
        margin-top: 12px;
    }

    .mini-link {
        font-size: 0.85rem;
        text-decoration: none;
        border-bottom: 1px dashed var(--accent-dim);
    }

    .mini-link:hover {
        border-bottom-style: solid;
    }

    .download-note {
        margin-top: 12px;
        color: var(--fg-dim);
        font-size: 0.82rem;
        line-height: 1.6;
    }

    .hero-metrics {
        display: grid;
        gap: 12px;
        grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    }

    .hero-metrics article {
        border: 1px dashed var(--border);
        border-radius: 10px;
        padding: 14px;
        background: color-mix(in srgb, var(--bg) 70%, transparent);
    }

    .hero-metrics h3 {
        margin: 0;
        font-size: 1.5rem;
        color: var(--accent);
    }

    .hero-metrics p {
        margin: 8px 0 0;
        color: var(--fg-dim);
        font-size: 0.85rem;
        line-height: 1.4;
    }

    .section-title {
        font-size: 0.9rem;
        text-transform: uppercase;
        letter-spacing: 0.14em;
        color: var(--accent-dim);
        margin-bottom: 16px;
    }

    /* Features */
    .features {
        margin-top: 10px;
    }

    .feature-grid {
        display: grid;
        grid-template-columns: repeat(4, minmax(0, 1fr));
        gap: 12px;
    }

    .feature-grid article,
    .arch-grid article,
    .stack-grid article {
        border: 1px solid var(--border);
        border-radius: 12px;
        padding: 20px;
        background: var(--bg-surface);
        backdrop-filter: blur(12px);
        transition:
            transform 0.3s cubic-bezier(0.16, 1, 0.3, 1),
            box-shadow 0.3s ease,
            border-color 0.3s ease;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.02);
    }

    .feature-grid article:hover,
    .arch-grid article:hover,
    .stack-grid article:hover {
        border-color: var(--accent);
    }

    .feature-grid h3 {
        margin: 0 0 10px;
        font-size: 1rem;
    }

    .feature-grid p {
        margin: 0;
        color: var(--fg-dim);
        line-height: 1.6;
        font-size: 0.9rem;
    }

    /* Architecture */
    .architecture {
        margin-top: 28px;
    }

    .arch-grid {
        display: grid;
        grid-template-columns: repeat(4, minmax(0, 1fr));
        gap: 12px;
    }

    .arch-grid h3 {
        margin: 0 0 10px;
        font-size: 1rem;
    }

    .arch-grid p {
        margin: 0;
        color: var(--fg-dim);
        line-height: 1.6;
        font-size: 0.9rem;
    }

    /* Components */
    .components {
        margin-top: 28px;
    }

    .component-grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 12px;
    }

    .component {
        border: 1px solid var(--border);
        border-radius: 14px;
        padding: 24px;
        background: var(--bg-surface);
        display: grid;
        gap: 12px;
        backdrop-filter: blur(12px);
        transition:
            transform 0.3s cubic-bezier(0.16, 1, 0.3, 1),
            box-shadow 0.3s ease,
            border-color 0.3s ease;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.05);
    }

    .component.active {
        background: linear-gradient(
                160deg,
                color-mix(in srgb, var(--accent) 5%, transparent),
                transparent 50%
            ),
            var(--bg-surface);
    }

    .component:hover {
        border-color: var(--accent);
    }

    .top {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 8px;
    }

    .tag {
        font-size: 0.7rem;
        padding: 4px 8px;
        border: 1px solid var(--border);
        border-radius: 999px;
        letter-spacing: 0.08em;
    }

    .name {
        color: var(--fg-dim);
        font-size: 0.8rem;
        text-transform: uppercase;
    }

    .component h3 {
        margin: 0;
        font-size: 1.35rem;
    }

    .component p {
        margin: 0;
        color: var(--fg-dim);
        line-height: 1.6;
    }

    .component ul {
        margin: 0;
        padding-left: 18px;
        color: var(--fg-dim);
        display: grid;
        gap: 6px;
    }

    .actions {
        display: flex;
        flex-wrap: wrap;
        gap: 10px;
    }

    .actions a {
        color: var(--accent);
        text-decoration: none;
        border-bottom: 1px dashed var(--accent-dim);
        font-size: 0.9rem;
    }

    .actions a:hover {
        border-bottom-style: solid;
    }

    /* CLI & Quick Start */
    .cli-section,
    .quickstart,
    .layout-section {
        margin-top: 28px;
    }

    .panel {
        border: 1px solid var(--border);
        border-radius: 12px;
        overflow: auto;
        background: var(--panel-bg);
        backdrop-filter: blur(12px);
        box-shadow: var(--panel-shadow);
    }

    pre {
        margin: 0;
        padding: 24px;
        font-size: 0.9rem;
        line-height: 1.6;
        color: var(--fg);
        text-shadow: var(--panel-pre-text-shadow);
    }

    .cli-note {
        margin-top: 10px;
        color: var(--fg-dim);
        font-size: 0.85rem;
    }

    .cli-note code {
        color: var(--accent);
    }

    .quickstart {
        margin-top: 28px;
    }

    /* Tech Stack */
    .tech-stack {
        margin: 28px auto 56px;
    }

    .stack-grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 12px;
    }

    .stack-grid h3 {
        margin: 0 0 10px;
        font-size: 1rem;
    }

    .stack-grid p {
        margin: 0;
        color: var(--fg-dim);
        line-height: 1.6;
        font-size: 0.9rem;
    }

    @media (max-width: 1100px) {
        .feature-grid,
        .arch-grid {
            grid-template-columns: repeat(2, minmax(0, 1fr));
        }

        .component-grid,
        .stack-grid {
            grid-template-columns: 1fr;
        }
    }

    @media (max-width: 700px) {
        .hero,
        .features,
        .architecture,
        .components,
        .cli-section,
        .quickstart,
        .layout-section,
        .tech-stack {
            width: min(1180px, 100% - 26px);
        }

        .hero {
            padding: 30px 0 40px;
        }

        .hero-inner {
            padding: 24px 18px;
        }

        h1 {
            font-size: 1.6rem;
        }

        .hero-actions {
            justify-content: stretch;
        }

        .hero-actions .btn {
            width: 100%;
            text-align: center;
        }

        .download-title-row,
        .install-actions {
            width: 100%;
        }

        .download-pill {
            width: 100%;
            white-space: normal;
        }

        .install-actions .btn {
            width: 100%;
            text-align: center;
        }

        .install-detail-grid {
            grid-template-columns: 1fr;
        }

        .package-command {
            align-items: flex-start;
            flex-direction: column;
        }

        .hero-metrics {
            grid-template-columns: repeat(2, minmax(0, 1fr));
        }

        .feature-grid,
        .arch-grid {
            grid-template-columns: 1fr;
        }
    }
</style>
