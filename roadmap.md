# DevOps Hotshot Roadmap — 6-8 Month Plan

**Pace:** 2-3 hrs/day | **Style:** Concept → hands-on immediately, every time | **Certs:** Fluid checkpoints, not deadlines

## How to use this doc
- Each phase has: concepts, a resource for *understanding*, a hands-on task for *proving* you understood it, and a checkpoint.
- Shell scripting and Python have their own dedicated curriculum (see the section right after this one) laid out in levels, not weeks — and every phase below now has explicit **[Shell Lx]** / **[Python Lx]** tags marking exactly where each level gets applied to real work. You'll hit Shell L1 in Phase 1 Week 1-2, Python L1 in Phase 1 Week 3-4, and both continue through Phase 6.
- Certs (AWS SAA, CKA) are marked as **"cert checkpoint"** — take them when your practice-exam scores say you're ready, not on a fixed week. If you fall behind, skip the exam and keep the hands-on skill; you lose nothing.
- Resource picks below are the ones that came up consistently as current best-in-class as of mid-2026 — mostly Udemy/free because they're the most battle-tested for these specific exams, with Pluralsight/LinkedIn Learning as your subscription-covered alternative where it's genuinely comparable. Use what's already paid for (Pluralsight/LinkedIn) as your first pass for concept videos, and pull in the named Udemy courses specifically for exam-pattern practice tests, since that's where they clearly outperform.

---

## Shell Scripting & Python — the dedicated track

You were right to push on this — "it's woven in" wasn't specific enough. Here's the actual curriculum, laid out in levels rather than weeks, because you'll move through these at different speeds depending on what else you're doing that phase. Each level lists what to learn, where to learn it, and which phase's hands-on work you apply it to.

### Shell Scripting

**Level 1 — Fundamentals (do this in Phase 1, Weeks 1-2, before networking):**
- Variables, quoting rules (`"$var"` vs `$var`, single vs double quotes), exit codes and `$?`, conditionals (`if`/`elif`/`case`), loops (`for`/`while`/`until`), functions, arrays, reading input.
- Resources: Zero To Mastery's "Bash Scripting" course (paid, well-reviewed, project-based) or the free alternative — freeCodeCamp's Bash scripting section within their Linux course, and Telusko's "Linux Essentials with Bash Shell Scripting for DevOps & Cloud" on Udemy if you want an AWS-EC2-flavored version specifically.
- Apply immediately to: the Phase 1 cron/process-monitoring script and the host-ping script — don't watch a full course start-to-finish first, learn a construct then use it that same session.

**Level 2 — Text processing & pipes (Phase 1, Weeks 3-4, alongside networking):**
- `grep`, `sed`, `awk` (this is the one people under-invest in — awk alone can replace a lot of Python for log parsing), `cut`, `sort`, `uniq`, `xargs`, pipe chaining, `find` with `-exec`.
- Resource: same Bash courses above cover this, but also just practice on real log files — `journalctl` output, `/var/log/*`, or nginx access logs are perfect raw material.
- Apply to: parsing your ping-script output and the process-monitoring logs into a clean report.

**Level 3 — Robust scripting (Phase 2, alongside AWS console work):**
- `set -euo pipefail` and why every serious script should start with it, `trap` for cleanup on exit/error, argument parsing with `getopts`, proper logging (timestamps, log levels), meaningful exit codes for calling scripts to check.
- Resource: no single course nails this well — pull from "Google's Shell Style Guide" (free, official, the industry reference for what "good" bash looks like) and apply its rules retroactively to your Level 1-2 scripts.
- Apply to: harden your cost-hygiene script from Phase 2 into something that could survive being run unattended in a cron job — proper error handling, logging, and a `--dry-run` flag.

**Level 4 — Debugging & style (ongoing, revisit in Phase 3+):**
- `bash -x` for tracing execution, `shellcheck` (install it and run it against every script you've written so far — it will humble you productively), idempotency (can this script run twice safely?).
- Resource: `shellcheck.net` itself is both the tool and the best teacher — it explains every warning it gives.

### Python

**Level 1 — Core fundamentals (start Phase 1, Weeks 3-4, right as bash starts feeling natural):**
- Syntax, data structures (lists/dicts/sets/tuples and when to use each), functions, file I/O, exceptions/try-except, virtual environments (`venv`) and `pip` — get in the habit of a venv per project immediately, don't skip this.
- Resources: Pluralsight's "Introduction to Python for DevOps/Scripting" (use your subscription — it's specifically framed around ops tasks, not general CS, which matters for how fast this feels useful) or Coursera's "Python Scripting for DevOps Specialization" if you prefer a structured multi-course path with a completion certificate.
- Apply to: nothing yet — this level is pure fundamentals, resist the urge to skip to boto3 before this is solid.

**Level 2 — Structured data & shelling out (Phase 2, alongside AWS console work):**
- Parsing JSON (`json` module — you'll live in this for AWS API responses), YAML (`pyyaml` — essential the moment you touch Terraform/Kubernetes/GitHub Actions configs programmatically), CSV, calling shell commands from Python (`subprocess.run`, and why it's preferred over the older `os.system`).
- Apply to: nothing new yet, but this unlocks Level 3.

**Level 3 — AWS automation with boto3 (Phase 2, Week 7-8, right alongside Terraform):**
- `boto3` basics: sessions, clients vs resources, pagination (AWS APIs paginate — a script that "misses" resources because it didn't paginate is a classic bug), error handling for AWS-specific exceptions.
- Resource: the Udemy course "Python for DevOps: Mastering Real-World Automation" is well-aligned to this exact stage — it assumes you know basic Python and CI/CD concepts (which you do) and goes straight into automation patterns. Pair it with boto3's own official docs (boto3.amazonaws.com) as your day-to-day reference, since that's what you'll actually use on the job.
- Apply to: the untagged-EC2-resources script (already planned in Phase 2) — build it properly here rather than as a one-off, with pagination and error handling from Level 3 techniques.

**Level 4 — Kubernetes automation (Phase 3-4, alongside kind/EKS work):**
- The official `kubernetes` Python client library — listing/watching resources, filtering by status, basic CRUD on custom objects.
- Resource: the client library's GitHub examples directory is honestly the best teaching material here — there isn't a single standout course for this narrow a topic, so lean on official docs and adapt examples to your own cluster.
- Apply to: the CrashLoopBackOff-detector script already planned in Phase 3.

**Level 5 — Testing & packaging (Phase 5-6, once you have several working scripts):**
- Basic `pytest` (write tests for at least one of your automation scripts — this is a small but real interview signal: "I write tests for my ops tooling"), `argparse` or `click` to turn a script into a proper CLI tool with `--help` output, structuring a script as an installable package if it's grown beyond one file.
- Resource: pytest's own official docs "Get Started" page is genuinely sufficient for this level — you don't need a full course for basic unit tests on scripts this size.
- Apply to: pick your best script from Phases 2-4 (probably the boto3 tagging script or the K8s debugger) and give it a proper CLI interface plus 3-4 tests.

**A rule of thumb across both:** if a task is single-machine, sequential, and mostly gluing shell commands together — bash. If it needs to call an API, parse structured data (JSON/YAML), handle pagination, or do anything with real logic/error handling — Python. Knowing when to reach for which is itself a skill worth having opinions about in an interview.

---

## PHASE 0 (Week 0): Setup
- Set an AWS Billing Alarm at $20 and $50 immediately (you have a non-free-tier account — do this before touching anything else).
- Install: `kind`, `docker`, `terraform`, `awscli` v2, `kubectl`, VS Code with YAML/Terraform extensions.
- Create a GitHub org/repo structure now: `devops-journey` monorepo with folders `linux-labs/`, `terraform-projects/`, `k8s-projects/`, `scripts/`, `ci-cd/`. Commit as you go — this becomes your portfolio.

---

## PHASE 1 (Weeks 1-4): Linux Internals + Networking + Bash

### Week 1-2: Linux processes, systemd, internals
**Concepts:** processes vs threads, process states, signals (SIGTERM/SIGKILL/SIGHUP), `/proc` filesystem, file descriptors, systemd units & targets, memory (buff/cache vs used/available), load average vs CPU%, zombie/orphan processes.

**Resources:**
- Pluralsight: "Linux System Administration" or "Understanding the Linux Kernel" path (use your subscription — check current catalog for the closest active title, catalog names shift).
- YouTube (free, excellent): freeCodeCamp's Linux full course, and NetworkChuck for practical/entertaining walkthroughs of processes and systemd.
- Book-lite: `man systemd`, and the classic "The Linux Command Line" (free PDF online) for a reference you'll come back to.

**Hands-on:**
- Spin up an EC2 instance (or local VM). Deliberately create a runaway process, kill it different ways (`kill`, `kill -9`, `pkill`), observe with `ps`, `top`, `htop`.
- **[Shell L1 — see dedicated track]** Write your first bash scripts here: a script that logs top 5 memory-consuming processes to a file every minute via cron. This is where you actually do Level 1 (variables, conditionals, loops, functions) — don't finish a bash course first, learn a construct then use it same-session.
- Explore `/proc/<pid>/` for a running process — map fields to what `ps aux` shows you.
- Create and manage a custom systemd service (wrap a simple script as a service with auto-restart).

**Checkpoint:** You can explain what happens end-to-end when you type a command in bash and hit enter (fork/exec, PATH resolution, exit codes) — this is a genuinely common interview question.

### Week 3-4: Networking fundamentals
**Concepts:** OSI/TCP-IP model (just enough, not academic depth), subnetting/CIDR math, DNS resolution chain, TCP 3-way handshake, ports & sockets, L4 vs L7 load balancing, reverse proxy vs forward proxy, TLS handshake basics.

**Resources:**
- LinkedIn Learning: "Networking Foundations" or "CompTIA Network+" intro modules (use your subscription).
- YouTube: NetworkChuck's subnetting videos are widely considered the clearest free explanation; PowerCert Animated Videos for OSI/TCP-IP visuals.
- Practice tool: subnetting practice site (search "subnetting practice game" — several free ones let you drill until it's automatic).

**Hands-on:**
- Use `ss`, `netstat`, `dig`, `nslookup`, `traceroute`, `curl -v` on real requests — watch a TLS handshake happen with `curl -v https://example.com`.
- **[Shell L2 — see dedicated track]** Write a script that pings a list of hosts and reports up/down status with timestamps, logs to a file. Pipe the output through `grep`/`awk`/`sort` to produce a clean summary report — this is where Level 2 text-processing tools actually get used, not just watched in a video.
- Manually subnet a `/16` into eight `/19`s on paper, then verify by configuring it as an actual AWS VPC CIDR block later in Phase 2.
- **[Python L1 starts here]** In parallel with the networking work, start Python fundamentals (syntax, data structures, file I/O, `venv` habit). Nothing to apply it to yet — that's intentional, Level 1 is pure foundation before Phase 2's boto3 work needs it.

**Checkpoint:** Given a `/24`, you can produce 4 usable subnets with correct ranges without a calculator, in under 2 minutes.

---

## PHASE 2 (Weeks 5-8): AWS Core Services + Start SAA Track

### Week 5-6: AWS core services (console-first)
**Concepts:** IAM (users/roles/policies/STS — the #1 source of confusion, spend real time here), EC2, VPC (subnets/route tables/security groups/NACLs/IGW/NAT gateway), S3 (storage classes, lifecycle policies, bucket policies), ELB (ALB vs NLB), Route53, RDS basics, CloudWatch (metrics/logs/alarms).

**Resources — this is your SAA prep track, start now:**
- **Primary video course:** Stéphane Maarek's "Ultimate AWS Certified Solutions Architect Associate SAA-C03" on Udemy — consistently the top recommendation, ~27 hrs, frequently on sale for ~$12-15. Worth buying even with Pluralsight available, because it's built exam-pattern-first.
- **Alternative if you want more depth over speed:** Adrian Cantrill's SAA-C03 course (learn.cantrill.io) — longer (~60-70 hrs with labs) but builds real architectural intuition, not just exam recall. Consider this if Maarek feels too fast-paced for your gaps.
- **Free supplement:** AWS Skill Builder (skillbuilder.aws) — official AWS learning paths plus a free SAA exam prep course with practice questions. Also pull the official SAA-C03 Exam Guide PDF from AWS's certification page and use it as your syllabus checklist.
- Use Pluralsight's AWS SAA-C03 learning path as a secondary pass on any topic that didn't stick from Maarek.

**Hands-on (console, deliberately, before Terraform):**
- Build a VPC with 2 public + 2 private subnets across 2 AZs, IGW, NAT gateway, route tables. This directly uses your Week 3-4 subnetting practice.
- Launch an EC2 instance in a private subnet, reachable only via a bastion host or SSM Session Manager (no exposed SSH port — good practice + interview talking point).
- Put an ALB in front, target group health checks, and an RDS instance in the private subnet.
- Set up a CloudWatch alarm on CPU utilization that notifies via SNS.
- **Cost discipline:** since your account isn't free-tier, use `t3.micro`, tear everything down at the end of each session (`terraform destroy` will matter more in Week 7-8, but for now, manually delete resources you spin up).

**[Python L2 + L3 — see dedicated track]** Before writing this script, do a quick pass on Level 2 (parsing JSON — AWS API responses are JSON, `subprocess.run` if you need to shell out at all) then move straight into Level 3 (`boto3` sessions/clients, pagination, AWS-specific error handling). Then write the actual script: a Python script using `boto3` that lists all running EC2 instances across regions and flags any without a stop/termination tag — a genuinely useful cost-hygiene script you can put on your resume as "built automation to catch untagged AWS resources." Get pagination right here — it's the classic bug in scripts like this.

### Week 7-8: Terraform fundamentals + rebuild everything
**Concepts:** providers, resources/data sources, state (why remote state + locking matters — this is a common interview question), variables/outputs/locals, modules basics, `plan`/`apply`/`destroy` workflow.

**Resources:**
- HashiCorp's own "Terraform - Getting Started" tutorials (learn.hashicorp.com) — free, official, well-maintained.
- YouTube: TechWorld with Nana's Terraform crash course is widely recommended as the clearest free intro.
- Pluralsight: "Terraform - Getting Started" path (use subscription) as a secondary pass.

**Hands-on:**
- Set up remote state: S3 bucket + DynamoDB table for locking, from scratch, before writing any other resources.
- Rewrite your entire Week 5-6 VPC/EC2/ALB/RDS stack in Terraform, using variables and at least one reusable module (e.g., a VPC module you could reuse later for a different project).
- Intentionally break something (e.g., manually delete a resource in console that Terraform manages) and practice reconciling state — this teaches you more than any tutorial.
- **[Shell L3 — see dedicated track]** Go back to your Phase 1 scripts and the Week 5-6 boto3 script's calling wrapper (if any) and harden them: `set -euo pipefail`, `trap` for cleanup, `getopts` for arguments, real logging, a `--dry-run` flag. This is also a good week to run `shellcheck` (Level 4) against everything you've written so far, since you now have several scripts to point it at.

**Cert checkpoint:** Start Tutorials Dojo (Jon Bonso) practice exams now, even before you feel ready — the format familiarity matters, and using it as a diagnostic to reveal weak domains is more useful than saving it for "when I'm ready." When you're consistently scoring 80%+, schedule the SAA-C03 exam ($150, via Pearson VUE/PSI). Given your pace, this likely lands end of Phase 2 or into Phase 3 — that's fine, don't force it.

---

## PHASE 3 (Weeks 9-12): Docker Deep Dive + Kubernetes Core

### Week 9: Docker beyond the basics
**Concepts:** multi-stage builds, layer caching and image size optimization, bind mounts vs named volumes, Docker networking modes (bridge/host/none), `docker-compose` for multi-container local dev, debugging running containers.

**Resources:**
- YouTube: TechWorld with Nana's Docker course, or freeCodeCamp's Docker course — both free and thorough.
- Pluralsight: "Docker Deep Dive" path.

**Hands-on:**
- Take a Dockerfile you'd consider "simple" (your current level) and optimize it: multi-stage build to shrink image size, non-root user, proper layer ordering for cache efficiency. Measure before/after image size — good resume metric.
- Build a docker-compose stack: app + database + reverse proxy (nginx), with health checks and restart policies.

### Week 10-12: Kubernetes core concepts
**Concepts:** pods, deployments, replicasets, services (ClusterIP/NodePort/LoadBalancer), ingress, configmaps/secrets, namespaces, resource requests/limits, liveness/readiness/startup probes, labels & selectors.

**Resources — this doubles as your CKA on-ramp:**
- **Primary:** Mumshad Mannambeth / KodeKloud's "Certified Kubernetes Administrator (CKA) with Practice Tests" on Udemy — the most-recommended CKA course by a wide margin, browser-based labs included, no local cluster setup friction.
- Free supplement: Kubernetes official docs tutorials (kubernetes.io/docs/tutorials) — genuinely well-written and the exam allows you to reference official docs, so get comfortable navigating them now.
- Kelsey Hightower's "Kubernetes the Hard Way" (free on GitHub) — don't do this yet, save it for Phase 4, but bookmark it now.

**Hands-on:**
- Start with `kind` locally (free, fast iteration) — deploy a multi-container app with a Deployment, Service, ConfigMap, and Secret.
- Deliberately misconfigure something (wrong selector labels, missing resource limits, a failing readiness probe) and practice diagnosing with `kubectl describe`, `kubectl logs`, `kubectl get events` — this debugging muscle is what actually gets tested in CKA and in real jobs.
- **[Python L4 — see dedicated track]** Write a Python script using the official `kubernetes` client library to list all pods across namespaces that are in `CrashLoopBackOff`. This is your first real use of the client library — lean on its GitHub examples rather than looking for a course, since there isn't a great one for this narrow a topic.

**Checkpoint:** You can deploy an app to a local cluster from scratch, break its networking on purpose, and fix it, without looking anything up beyond official docs.

---

## PHASE 4 (Weeks 13-16): EKS + Kubernetes Advanced (+ CKA decision point)

### Week 13-14: Kubernetes advanced concepts
**Concepts:** Helm (templating, charts, releases), StatefulSets, PersistentVolumes/PersistentVolumeClaims/StorageClasses, Horizontal Pod Autoscaler, RBAC (roles/rolebindings/serviceaccounts), NetworkPolicies.

**Resources:** Continue KodeKloud CKA course into these modules — it covers all of this directly. Supplement Helm specifically with the official Helm "Quickstart Guide" (helm.sh/docs).

**Hands-on:**
- Convert your Phase 3 app deployment into a Helm chart with templated values for environment-specific config (dev vs prod).
- Add a StatefulSet + PVC for a stateful component (e.g., a database) — understand why this is different from a Deployment.
- Configure HPA on a deployment and load-test it (even a simple `hey` or `ab` load test) to watch it scale.

### Week 15-16: Move to real EKS, provisioned via Terraform
**Concepts:** EKS cluster architecture, node groups vs Fargate, IAM roles for service accounts (IRSA), ALB Ingress Controller, cluster autoscaler basics.

**Resources:** AWS's own EKS workshop (eksworkshop.com) — free, hands-on, official, and specifically designed for this exact learning goal. Pair with your existing Terraform knowledge — search "terraform-aws-modules/eks" on the Terraform Registry for the standard community module as your reference implementation (read it, don't just copy-paste it — understand what each block does).

**Hands-on:**
- Provision an EKS cluster via Terraform (this is the single highest-leverage combo project in this whole plan — Terraform + EKS together is a strong interview story).
- Deploy your Helm chart from Week 13 onto real EKS.
- Set up Ingress with a real domain (Route53) and TLS (ACM or cert-manager).
- **Cost discipline:** EKS control plane costs money per hour even idle — destroy the cluster at the end of each work session (`terraform destroy`), don't leave it running overnight.
- **[Python L4 continued]** Point your Phase 3 `kubernetes`-client CrashLoopBackOff script at the real EKS cluster instead of `kind` — a script that only works against a local toy cluster isn't actually proven yet. Extend it to also check for pods stuck in `Pending` (often a resource-request/scheduling issue you'll now recognize from Phase 3 concepts).

**CKA decision point:** By now you'll know whether Kubernetes concepts feel solid or shaky.
- If solid → start killer.sh simulator sessions (included with CKA exam registration, and the closest thing to the real exam format) and schedule the CKA exam (~$445) in the next 4-6 weeks.
- If shaky → keep doing labs without the exam pressure, revisit CKA after the job search starts, or skip it — the EKS project on your resume carries real weight even without the cert.

---

## PHASE 5 (Weeks 17-20): GitHub Actions + Full CI/CD Integration

**Concepts:** workflow syntax (triggers, jobs, steps), matrix builds, reusable workflows/composite actions, secrets management, **OIDC federation to AWS** (avoid long-lived AWS access keys entirely — this is a strong modern-practices signal in interviews), caching dependencies for faster builds.

Since you already understand CI/CD deeply from Jenkins, this phase should move faster than the time allotted — use spare time here to go back and reinforce any weaker phase.

**Resources:**
- GitHub's own Actions documentation and "Learn GitHub Actions" course — genuinely good, keep it as primary reference since syntax details change.
- YouTube: TechWorld with Nana's GitHub Actions course for a structured walkthrough.

**Hands-on — your flagship project:**
- Build a full pipeline: code push → run tests → build Docker image → push to ECR → deploy to your EKS cluster via Helm.
- Use OIDC so GitHub Actions assumes an AWS IAM role rather than storing static credentials.
- Add a manual approval gate before production deploy (common real-world pattern, easy to demo).
- This project — Terraform-provisioned EKS + Helm + GitHub Actions CI/CD with OIDC — is the one you lead with in every interview.
- **[Python L5 — see dedicated track]** Since your CI/CD understanding is already strong, use the spare time this phase frees up to go back to your best script (probably the boto3 tagging script or the CrashLoopBackOff detector) and give it Level 5 treatment: a proper `argparse`/`click` CLI interface with `--help` output, plus 3-4 `pytest` tests. Wire the tests into a GitHub Actions workflow so they run on every push — this ties Phase 5's CI/CD skill directly to Phase 2-4's scripts instead of leaving them as one-off files.

---

## PHASE 6 (Weeks 21-24): Observability + Security Basics + Polish

**Concepts:** Prometheus (metrics scraping) + Grafana (dashboards), basic alerting rules, CloudWatch Container Insights as the lighter AWS-native alternative, image scanning (Trivy), least-privilege IAM review, secrets management (avoid hardcoded secrets — AWS Secrets Manager or External Secrets Operator for K8s).

You flagged this as a real gap — the target here isn't mastery, it's "I can set up meaningful visibility into a running system and explain what I'd alert on," which is what gets tested in interviews.

**Resources:**
- Prometheus + Grafana official "Getting Started" guides.
- YouTube: TechWorld with Nana's Prometheus/Grafana crash course.

**Hands-on:**
- Deploy Prometheus + Grafana to your EKS cluster (via Helm — the `kube-prometheus-stack` chart is the standard).
- Build one dashboard showing pod CPU/memory and request latency for your flagship app.
- Set up one meaningful alert (e.g., pod restart count threshold) routed somewhere visible (even just Slack webhook).
- Run Trivy against your Docker images and fix at least one real finding.
- **[Python thread, applied not new]** Write the Slack-webhook alert handler itself in Python (using `requests`) rather than a raw curl call in an alertmanager config — small, but it's a natural place to demonstrate the `requests`/API-calling skill from Level 2-3 without introducing a new concept this late in the plan.

**If CKA is happening, this is a reasonable window to sit it,** since Kubernetes concepts are now reinforced by the observability work too.

---

## PHASE 7 (Weeks 25-32ish): Portfolio Polish + Job Hunt

- Rewrite your resume around the projects with real metrics: image size reduction %, deploy time before/after CI/CD, cost savings from the tagging script, etc.
- Clean up your GitHub repos — strong READMEs with architecture diagrams (even simple ones) matter as much as certs for "hotshot" positioning.
- Start applying by Phase 6/7 overlap, not after everything feels perfect — real interview loops will surface gaps faster than continued solo study, and you'll have 6-8 weeks of runway left to patch whatever comes up.

---

## Running project list (your portfolio, in build order)
1. VPC + EC2 + ALB + RDS — console, then rebuilt in Terraform with remote state
2. Cost-hygiene Python/boto3 script (untagged resource finder)
3. Optimized multi-stage Dockerfile + docker-compose stack
4. Local Kubernetes app deployment (kind) with deliberate break/fix documentation
5. Helm chart conversion + HPA + StatefulSet demo
6. **EKS cluster via Terraform + Helm deployment** (flagship piece #1)
7. **Full GitHub Actions CI/CD pipeline with OIDC to AWS, deploying to EKS** (flagship piece #2)
8. Prometheus/Grafana observability stack on the EKS cluster

## Cert summary (fluid, not deadlines)
| Cert | Primary resource | Practice exams | Realistic window |
|---|---|---|---|
| AWS SAA-C03 | Stéphane Maarek (Udemy) or Adrian Cantrill for more depth | Tutorials Dojo / Jon Bonso | End of Phase 2 into Phase 3 |
| CKA | KodeKloud / Mumshad Mannambeth (Udemy) | killer.sh (included w/ exam registration) | Decision point end of Phase 4, exam in Phase 6 if pursuing |
