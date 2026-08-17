# JMeter Performance Testing Pipeline

Automated performance testing using **Apache JMeter**, executed via **GitHub Actions**, with HTML reports published to **GitHub Pages** and available as downloadable artifacts.

---

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── jmeter-pipeline.yml   # GitHub Actions workflow
├── jmeter/
│   ├── test-plan.jmx             # Your JMeter test plan (REPLACE THIS)
│   └── user.properties           # JMeter configuration overrides
└── README.md
```

---

## Getting Started

### Step 1: Add Your JMX File

Replace the sample `jmeter/test-plan.jmx` with your own JMeter test plan:

```bash
# Simply copy your JMX file to this path:
jmeter/test-plan.jmx
```

> If your JMX uses parameterized variables (threads, ramp_up, duration),
> make sure they reference `${__P(threads,10)}`, `${__P(ramp_up,30)}`, etc.
> to allow runtime overrides from the workflow.

### Step 2: Enable GitHub Pages

1. Go to your GitHub repository → **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. Save the settings

### Step 3: Push & Trigger the Pipeline

```bash
git add .
git commit -m "feat: add JMeter performance testing pipeline"
git push origin main
```

The pipeline will automatically trigger on every push to `main`.

---

## Pipeline Triggers

| Trigger | Description |
|---------|-------------|
| `push` to `main`/`master` | Runs automatically on every push |
| `pull_request` | Runs tests (no Pages deployment) |
| `workflow_dispatch` | Manual trigger with custom parameters |

---

## Manual Run with Custom Parameters

Go to **Actions** → **JMeter Performance Tests** → **Run workflow**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `jmeter_version` | `5.6.3` | JMeter version to use |
| `threads` | `10` | Number of concurrent virtual users |
| `ramp_up` | `30` | Ramp-up period in seconds |
| `duration` | `0` | Test duration in seconds (0 = use JMX value) |

---

## Viewing Results

### GitHub Pages (Live Report)
After a successful run, the HTML report is published at:
```
https://<your-username>.github.io/<your-repo>/
```

### Downloadable Artifacts
Each run uploads two artifacts (retained for 30 days):
- **`jmeter-html-report-<run_number>`** — Full HTML performance report (open `index.html`)
- **`jmeter-jtl-results-<run_number>`** — Raw `.jtl` results file

Download from: **Actions** → select a run → **Artifacts** section at the bottom.

---

## Configuration

Edit `jmeter/user.properties` to customize JMeter settings:
- Result file save configuration
- HTTP timeout settings
- Logging verbosity
- Default thread/ramp-up/duration values

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Pipeline fails with "JMX not found" | Ensure your JMX is at `jmeter/test-plan.jmx` |
| HTML report generation fails | Check that the `.jtl` results file has content |
| GitHub Pages not updating | Verify Pages is set to **GitHub Actions** as source |
| JMeter version unavailable | Check [archive.apache.org](https://archive.apache.org/dist/jmeter/binaries/) for valid versions |
