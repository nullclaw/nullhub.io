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
            <a href="https://nullclaw.io/nullclaw" target="_blank"
                ><strong>NullClaw</strong></a
            >,
            <a href="/nullboiler"><strong>NullBoiler</strong></a>,
            <a href="/nulltickets"><strong>NullTickets</strong></a>, and
            additional ecosystem components—all from a unified high-performance
            dashboard or CLI.
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
        transform: translateY(-4px);
        border-color: var(--accent);
        box-shadow:
            0 8px 24px rgba(0, 0, 0, 0.4),
            0 0 12px var(--border-glow),
            inset 0 1px 0 rgba(255, 255, 255, 0.05);
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
        transform: translateY(-4px);
        border-color: var(--accent);
        box-shadow:
            0 12px 32px rgba(0, 0, 0, 0.5),
            0 0 16px var(--border-glow),
            inset 0 1px 0 rgba(255, 255, 255, 0.1);
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
        background: rgba(0, 0, 0, 0.6);
        backdrop-filter: blur(12px);
        box-shadow:
            inset 0 0 20px rgba(0, 0, 0, 0.8),
            0 4px 12px rgba(0, 0, 0, 0.3);
    }

    pre {
        margin: 0;
        padding: 24px;
        font-size: 0.9rem;
        line-height: 1.6;
        color: var(--fg);
        text-shadow: 0 0 4px rgba(255, 255, 255, 0.2);
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

        .hero-metrics {
            grid-template-columns: repeat(2, minmax(0, 1fr));
        }

        .feature-grid,
        .arch-grid {
            grid-template-columns: 1fr;
        }
    }
</style>
