<svelte:head>
    <title>NullHub — Ecosystem Management Hub</title>
    <meta
        name="description"
        content="The simplest way to install, configure, and manage NullClaw, NullBoiler, NullTickets, and the rest of the autonomous AI ecosystem."
    />
</svelte:head>

<section class="hero">
    <div class="hero-inner">
        <p class="kicker">NULLHUB</p>
        <h1>The Simplest Way To Manage Your AI Infrastructure</h1>
        <p class="summary">
            One binary. One UI. Install, configure, monitor, and update
            <a href="https://nullclaw.io/nullclaw" target="_blank"
                ><strong>NullClaw</strong></a
            >,
            <a href="https://nullclaw.io/nullboiler" target="_blank"
                ><strong>NullBoiler</strong></a
            >,
            <a href="https://nullclaw.io/nulltickets" target="_blank"
                ><strong>NullTickets</strong></a
            >, and the rest of the ecosystem — from a single dashboard or the
            command line.
        </p>

        <div class="hero-actions">
            <a href="#quickstart" class="btn primary">Quick Start</a>
            <a href="#features" class="btn secondary">Features</a>
            <a href="#cli" class="btn secondary">CLI Reference</a>
            <a
                href="https://github.com/nullclaw/nullhub"
                target="_blank"
                class="btn secondary">GitHub</a
            >
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
                Manifest-driven guided setup with component-aware flows and
                local NullTickets → NullBoiler linking. Works from the browser
                or terminal.
            </p>
        </article>
        <article>
            <h3>Process Supervision</h3>
            <p>
                Start, stop, restart, crash recovery with backoff. All instances
                supervised by the hub daemon with auto-restart on failure.
            </p>
        </article>
        <article>
            <h3>Health Monitoring</h3>
            <p>
                Periodic HTTP health checks, dashboard status cards, and
                real-time state updates via SSE streaming.
            </p>
        </article>
        <article>
            <h3>Config Management</h3>
            <p>
                Structured editors for NullClaw, NullBoiler, and NullTickets
                configuration. Raw JSON fallback when needed.
            </p>
        </article>
        <article>
            <h3>Log Viewing</h3>
            <p>
                Tail and live SSE streaming per instance. View logs directly in
                the browser dashboard or follow from the CLI.
            </p>
        </article>
        <article>
            <h3>One-Click Updates</h3>
            <p>
                Download new versions, migrate config, rollback on failure.
                Update individual instances or everything at once.
            </p>
        </article>
        <article>
            <h3>Cross-Component Linking</h3>
            <p>
                Auto-connect NullTickets → NullBoiler, generate native tracker
                config, and inspect queue/orchestrator status from one UI.
            </p>
        </article>
        <article>
            <h3>Web UI + CLI</h3>
            <p>
                Browser dashboard for humans, CLI for automation and scripting.
                Both interfaces provide full feature parity.
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
                HTTP server, process supervisor, installer, manifest engine. Two
                modes: server (HTTP + supervisor threads) or CLI (direct calls,
                stdout, exit).
            </p>
        </article>
        <article>
            <h3>Svelte Frontend</h3>
            <p>
                SvelteKit with static adapter, <code>@embedFile</code>'d into
                the binary. Component UI modules (chat, monitor) loaded
                dynamically via Svelte 5 <code>mount()</code>.
            </p>
        </article>
        <article>
            <h3>Manifest-Driven</h3>
            <p>
                Each component publishes <code>nullhub-manifest.json</code> describing
                installation, configuration, launch, health checks, wizard steps,
                and UI modules. NullHub is a generic engine that interprets manifests.
            </p>
        </article>
        <article>
            <h3>Local Storage</h3>
            <p>
                All state lives under <code>~/.nullhub/</code> — config, instances,
                binaries, logs, and cached manifests. No external database required.
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
                Fastest, smallest, and fully autonomous AI assistant
                infrastructure written in Zig.
            </p>
            <ul>
                <li>Providers, channels, tools, memory, policy</li>
                <li>Terminal and gateway operation modes</li>
                <li>Execution runtime for worker agents</li>
            </ul>
            <div class="actions">
                <a href="https://nullclaw.io/nullclaw" target="_blank">Docs</a>
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
                Workflow orchestrator for multi-step run execution with
                approvals, retries, advanced step types, and worker dispatch.
            </p>
            <ul>
                <li>DAG execution with run/step/events APIs</li>
                <li>Worker registry + protocol-aware dispatch</li>
                <li>
                    Advanced steps: wait, router, loop, saga, debate, group_chat
                </li>
            </ul>
            <div class="actions">
                <a href="https://nullclaw.io/nullboiler" target="_blank">Docs</a
                >
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
            <h3>NullTickets (NullTracker)</h3>
            <p>
                Task tracking and execution-state backend with pipelines,
                leases, transitions, gates, dependencies, and artifacts.
            </p>
            <ul>
                <li>Role-based claim/lease loop for autonomous agents</li>
                <li>Pipeline transitions with required gates</li>
                <li>Operational queue analytics and OTLP ingest</li>
            </ul>
            <div class="actions">
                <a href="https://nullclaw.io/nulltickets" target="_blank"
                    >Docs</a
                >
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
        border: 1px solid var(--border);
        background: radial-gradient(
                circle at 85% 0%,
                color-mix(in srgb, var(--accent) 24%, transparent),
                transparent 40%
            ),
            var(--bg-surface);
        padding: 34px;
        border-radius: 12px;
    }

    .kicker {
        color: var(--accent-dim);
        font-size: 0.8rem;
        letter-spacing: 0.2em;
        margin-bottom: 14px;
    }

    h1 {
        margin: 0;
        font-size: clamp(1.8rem, 4vw, 3rem);
        line-height: 1.1;
        max-width: 20ch;
        text-wrap: balance;
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

    .feature-grid article {
        border: 1px solid var(--border);
        border-radius: 10px;
        padding: 16px;
        background: var(--bg-surface);
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

    .arch-grid article {
        border: 1px solid var(--border);
        border-radius: 10px;
        padding: 16px;
        background: var(--bg-surface);
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

    .arch-grid code {
        color: var(--accent);
        font-size: 0.85rem;
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
        border-radius: 12px;
        padding: 18px;
        background: var(--bg-surface);
        display: grid;
        gap: 10px;
    }

    .component.active {
        background: linear-gradient(
                160deg,
                color-mix(in srgb, var(--accent) 7%, transparent),
                transparent 45%
            ),
            var(--bg-surface);
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
        background: #000;
    }

    pre {
        margin: 0;
        padding: 18px;
        font-size: 0.88rem;
        line-height: 1.5;
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

    .stack-grid article {
        border: 1px solid var(--border);
        border-radius: 10px;
        padding: 16px;
        background: var(--bg-surface);
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

        .hero-metrics {
            grid-template-columns: repeat(2, minmax(0, 1fr));
        }

        .feature-grid,
        .arch-grid {
            grid-template-columns: 1fr;
        }
    }
</style>
