---
created: 2026-06-03
type: roadmap
tags: career, devops, cloud, learning
status: active
target: 2026-12-03
---

# 🚀 DevOps / Cloud Engineer — Fresher-to-Hired Roadmap

> [!info] What this is
> A fact-checked (24 sources, 25 verified claims) 3–6 month plan to go from "strong dev with some cloud exposure" to job-ready DevOps/Cloud engineer. Free-first budget. Targeting remote-global + abroad. Serves [[Goals 2026]] (Craft/skills). PDF/DOCX copies live at `~/Desktop/DevOps-Roadmap/`.

> [!important] The strategic reality (read first)
> **"DevOps fresher hired directly into a sponsored job abroad" is rare** — sponsored listings skew senior. The "regions with official DevOps shortages → easy sponsorship" claim was **refuted** in verification. So: aim at **remote-global + entry titles** now, and treat relocation as the step *after* the first role. The dev background (most DevOps freshers can't actually code) is the edge — lead with it.

**Realistic ladder, best → hardest:** remote-global (contractor/EOR) → domestic India junior cloud/SRE then relocate w/ 1–2 yrs → Europe Blue Card → Gulf (employer-tied) → US/CA/AU (long game).

---

## ✅ Milestones (auto-rolls up to [[Tasks dashboard]])

- [ ] **M1 — Foundations**: comfortable in Linux terminal + networking basics; AWS Free Tier account live 📅 2026-07-03 #devops-roadmap
- [ ] **Sit AWS Cloud Practitioner (CLF-C02)** — cert #1 🔼 📅 2026-08-03 #devops-roadmap
- [ ] **M2 — Project 1 live**: CI/CD pipeline (GitHub Actions) on a real app, pinned on GitHub 📅 2026-08-03 #devops-roadmap
- [ ] **M3 — Project 2 live**: AWS infra provisioned entirely in Terraform (no console clicks) 📅 2026-09-03 #devops-roadmap
- [ ] **M4 — Project 3 live (centerpiece)**: Kubernetes-deployed app + Prometheus/Grafana 📅 2026-10-03 #devops-roadmap
- [ ] **Sit AWS Solutions Architect Associate (SAA-C03)** — cert #2, the ROI cert ⏫ 📅 2026-11-03 #devops-roadmap
- [ ] **M5 — Portfolio polished**: 4 repos w/ diagrams + dashboard screenshots; DevOps resume variant + LinkedIn 📅 2026-11-03 #devops-roadmap
- [ ] **M6 — Applying daily** to remote-global + entry titles; 1 OSS contribution (serves [[OSS contributions]]) 📅 2026-12-03 #devops-roadmap

---

## 1. Skill stack & learning order

| Layer | Learn | Why |
|---|---|---|
| Foundations | **Linux** + **networking** (DNS, HTTP, TCP/IP, ports, OSI) | "Containers are just Linux processes." Bedrock. |
| Scripting | **Bash** + **Python** | Top-two automation langs. Python = head start. |
| Version control | **Git** (branching, PRs) | Formalize what you use. |
| Cloud | **AWS** (start here) | ~29–31% share, most postings. Azure only for Azure-heavy EU/Gulf. |
| Containers | **Docker** | Build/run/compose. Go deep. |
| Orchestration | **Kubernetes** | Biggest junior-vs-hireable differentiator. |
| IaC | **Terraform** (know OpenTofu exists) | De-facto standard. Skip Chef/Puppet. |
| CI/CD | **GitHub Actions** (default) | #1 now (JetBrains 2025: 33% vs Jenkins 28%). Know *of* Jenkins. |
| Observability | **Prometheus + Grafana** | Standard free monitoring. |

**Order (tuned for cloud-engineer goal):** Linux + networking → Bash (lean on Python) → Git → **AWS + Docker** → Kubernetes → Terraform → GitHub Actions → Prometheus/Grafana

> [!warning] Don't put cloud last
> Some roadmaps (milanm) put cloud dead last — wrong for a cloud-engineer goal. Pull AWS forward to early-mid so certs + projects build on it.

See also: [[Tech stack map]].

---

## 2. Certifications — the 1–2 that matter

| Cert | ~Cost | Verdict |
|---|---|---|
| **AWS Cloud Practitioner (CLF-C02)** | ~$100 | **Buy #1.** Cheap on-ramp, passes ATS screens. Won't get you hired alone. |
| **AWS Solutions Architect Associate (SAA-C03)** | ~$150 | **Buy #2 — the real ROI cert.** The credible-fresher minimum. |
| KCNA | ~$250 | Optional 3rd, only for k8s/platform roles. |
| CKA | ~$395 | Not yet — hands-on, ~40–50% fail. After first job. |
| Terraform Associate (003) | ~$70.50 | Cheap + respected; nice 3rd. |
| Azure AZ-900 / AZ-104 | ~$99 / $165 | Only for Azure-heavy markets. Don't split focus. |

**Play: AWS CCP → AWS SAA.** *Caveat: some cert-ordering advice traces to a vendor (KodeKloud) that sells these courses; the CCP→SAA path itself is independently corroborated.*

---

## 3. Best FREE resources

**Spine:** [roadmap.sh/devops](https://roadmap.sh/devops) · [milanm/DevOps-Roadmap](https://github.com/milanm/DevOps-Roadmap)

**One channel:** [TechWorld with Nana](https://www.youtube.com/c/TechWorldwithNana) — Docker, K8s, Terraform, CI/CD, Ansible, monitoring. *Her ~2020 K8s video has a few dated `kubectl` commands — pair with labs.*

**Hands-on labs (this is what makes you hireable):**
- [KodeKloud free tier](https://kodekloud.com/free-courses) — beginner crash courses + browser Playgrounds
- [Killercoda](https://killercoda.com/) — Linux, K8s, CKA/CKAD/CKS, Git, Terraform, GH Actions
- [AWS Free Tier](https://aws.amazon.com/free/) — real sandbox for projects

**Per-skill:**
- Linux: freeCodeCamp Linux Command Handbook + KodeKloud Linux crash course
- Networking: Professor Messer (Network+) + Cloudflare Learning Center
- Git: [Pro Git book](https://git-scm.com/book) + [Learn Git Branching](https://learngitbranching.js.org/)
- Python: [Automate the Boring Stuff](https://automatetheboringstuff.com/)
- AWS cert: freeCodeCamp's free ~14-hr Cloud Practitioner course (Andrew Brown / ExamPro, CLF-C02)
- Terraform: [HashiCorp Learn](https://developer.hashicorp.com/terraform/tutorials) + freeCodeCamp Terraform
- Ansible: KodeKloud "Ansible for Beginners"

---

## 4. Portfolio — 4 projects that get callbacks

Build in order; each reuses the last:

1. [ ] **CI/CD pipeline** — containerize a real app; GH Actions builds → tests → pushes image → deploys on push #devops-roadmap
2. [ ] **IaC (Terraform)** — provision that app's AWS infra in code only, no console clicks #devops-roadmap
3. [ ] **Kubernetes + monitoring** — deploy to k8s (kind/minikube → real cluster) + Prometheus/Grafana. **Strongest single signal.** #devops-roadmap
4. [ ] **Stretch** — homelab OR a documented "cloud migration" write-up #devops-roadmap

> [!tip] GitHub presentation = half the value
> Pin the repos. README = architecture doc: a diagram ([mingrammer/diagrams](https://github.com/mingrammer/diagrams)), one-line what/why/stack at top, pipeline YAML, **Grafana dashboard screenshots**. Recruiters skim — a diagram + screenshot beats 500 lines of YAML. Wire a *real* app (you have several) through the whole pipeline and lead with that story.

---

## 5. Job-search strategy

**Target these titles** (not "DevOps Engineer"): Cloud Support Associate/Engineer · Junior/Associate Cloud Engineer · Platform Support / SRE (Intern/Associate) · Junior Infrastructure/Ops Engineer

**Boards:** RemoteOK · We Work Remotely · Wellfound · Himalayas · NoDesk · jaabz.com (visa-tagged — *verify each on the company's own careers page; it infers sponsorship & skews senior*)

**The "no experience" gap:** portfolio projects ARE your experience — present them as such. Verified success pattern: dev → public projects → certs → targeted remote applications. One OSS contribution converts "no experience" → "shipped to a real project."

---

## 6. Timeline (3–6 months, part-time)

| Month | Focus | Output |
|---|---|---|
| 1 | Linux + networking; Bash; start AWS CCP course | Terminal-comfortable; Free Tier live |
| 2 | Pass **CCP**; Docker deep-dive | Cert #1 + **Project 1** (CI/CD) |
| 3 | Terraform/IaC; start SAA study | **Project 2** (Terraform infra) |
| 4 | Kubernetes (the big one) | **Project 3** (k8s + monitoring) |
| 5 | Pass **SAA**; polish | 4 repos + DevOps resume |
| 6 | Apply hard + 1 OSS contribution | Interviewing |

> Full-time compresses to ~3 months; part-time 5–6 is realistic.

---

## 🔎 Honest caveats (from fact-check)
- Skill/order claims = roadmap/Coursera consensus, *not* measured hiring data.
- Cert-ROI leans partly on a vendor blog; CCP→SAA path is solid.
- "Official shortages → easy sponsorship" was **refuted**.
- Visa/board data from a self-built aggregator that infers sponsorship & skews senior.
- Re-verify exam codes (CLF-C02, SAA-C03) + free-tier terms quarterly.

## Related
- [[Goals 2026]] · [[Tech stack map]] · [[OSS contributions]] · [[Reading list]] · [[Tasks dashboard]]
