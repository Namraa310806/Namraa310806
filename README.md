<div align="center">

<img src="./hero.svg" alt="Namraa Patel - backend engineer and open source contributor" width="100%" />

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/namraa-patel/)
[![Portfolio](https://img.shields.io/badge/Portfolio-A78BFA?style=for-the-badge&logo=firefox&logoColor=white)](https://namraa310806.github.io/Portfolio/)
[![LeetCode](https://img.shields.io/badge/LeetCode-2207-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/patelnamraa08/)
[![Codeforces](https://img.shields.io/badge/Codeforces-Expert_1731-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)](https://codeforces.com/profile/np254)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:patelnamraa88@gmail.com)

<img src="https://komarev.com/ghpvc/?username=Namraa310806&label=Profile+Views&color=A78BFA&style=flat-square" alt="Profile views" />

<br/><br/>

<a href="#autopsies"><b>Bug autopsies</b></a> · <a href="#projects"><b>Projects</b></a> · <a href="#experience"><b>Experience</b></a> · <a href="#skills"><b>Skills</b></a> · <a href="#wins"><b>Wins</b></a> · <a href="#contact"><b>Contact</b></a>

</div>


<div align="center">
<img src="./winner.svg" alt="Winner of JPMorgan Chase Code for Good 2026" width="100%" />
</div>

## `> choose your path`

<details open>
<summary><b>🧑‍💼 I'm a recruiter. Give me 20 seconds.</b></summary>

<br/>

- **Looking for:** Summer 2027 SDE / Backend / ML internships
- **Education:** B.Tech CSBS at PDEU, **CGPA 9.61 / 10** (graduating May 2028) · Diploma in CS, CGPA 9.78
- **Proof of work:** 5 fixes merged into **Celery** and **Microsoft Agent Framework**, plus Open Food Facts
- **Systems:** C++20 matching engine, **13.7K → 3.81M ops/s (278×)** on cancel-heavy load, validated under TSan/ASan/UBSan
- **Competitions:** JPMorgan Chase Code for Good 2026 winner · Amazon ML Challenge 2025 top 2,500 of 30,000+ · Codeforces Expert (1731) · LeetCode 2207
- **Industry:** Django backend intern, cut average API response time by **25%** under production load
- **Contact:** [patelnamraa88@gmail.com](mailto:patelnamraa88@gmail.com)

</details>

<details>
<summary><b>🛠️ I'm an engineer. Show me the hard stuff.</b></summary>

<br/>

Jump to the [**bug autopsies**](#autopsies): two animated walkthroughs of a reference-cycle memory leak and a re-entrant lock deadlock, both in Celery. Then see the [**matching engine**](#projects), where ThreadSanitizer caught a real memory-ordering race in my own lock-free queue.

</details>

<details>
<summary><b>🔧 I'm a maintainer. Will this person waste my time?</b></summary>

<br/>

Every PR featured below comes with a root cause, a minimal reproduction where it applies, and regression tests. In one review I flagged an uncovered edge case in my own fix before anyone asked. My first open source PR took 9 commits to merge, and I treat review feedback as the point, not an obstacle.

</details>

## `> what I actually do`

<div align="center">
<img src="./terminal.svg" alt="Terminal listing my upstream bug fixes" width="100%" />
</div>

I don't just add features to open source. I go looking for the bugs that hide in **concurrency, memory and shared state**, trace them to the exact line, and fix them with tests that make sure they never come back.

<div align="center">

| 🎯 **My best 5** merged PRs are featured below | 🧬 **5** different bug classes among them | 🏷️ **3** landed in Celery **5.7.0** |
|:---:|:---:|:---:|

<sub>This is a hand-picked selection, not the full list. See everything on GitHub: [Celery](https://github.com/celery/celery/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Microsoft Agent Framework](https://github.com/microsoft/agent-framework/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Open Food Facts](https://github.com/openfoodfacts/openfoodfacts-explorer/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)</sub>

<!-- UPDATE: replace the line above with your real total, e.g. "XX+ merged PRs across Celery, Microsoft Agent Framework and Open Food Facts" -->

</div>

## `> the long game`

<div align="center">
<img src="./resilience.svg" alt="9 commits to merge my first PR, then five upstream fixes" width="100%" />
</div>

My first open-source PR took **9 commits** to merge. I didn't quit, and I didn't take the review comments personally. Every round taught me something, and that's why I now keep fixing concurrency, memory and shared-state bugs in Celery and Microsoft Agent Framework: the kind that take patience just to reproduce. **Same person. Same persistence. Harder bugs.**

## `> the pipeline`

<div align="center">
<img src="./pipeline.svg" alt="Animated pipeline: detect, reproduce, fix and test, ship upstream" width="100%" />
</div>

<a id="autopsies"></a>

## `> 🔬 bug autopsies`

Five of my favourite fixes, picked for the hardest root causes. Watch the bug happen, then watch the fix. Each animation loops: **broken first, fixed second**.

<div align="center">

### Autopsy #1 · the leak that could not be freed

<img src="./autopsy-leak.svg" alt="Animated diagram: exception, traceback and frame form a reference cycle that leaks memory until the cycle is cut" width="100%" />

### Autopsy #2 · a thread that waits on itself

<img src="./autopsy-deadlock.svg" alt="Animated diagram: a redundant UNSUBSCRIBE re-enters a non-reentrant lock and deadlocks until the call is guarded" width="100%" />

</div>

### Case files · my best 5, not the full list

<details>
<summary><b>🧬 celery#10461</b> · shared-state mutation in <code>autoretry_for</code></summary>

<br/>

| | |
|:---|:---|
| **Bug class** | Shared mutable state |
| **Symptom** | State leaking between task retries |
| **Root cause** | A mutable default `retry_kwargs` dict was aliased across retries |
| **Fix** | Minimal reproduction first, then the fix |
| **Review** | Flagged an uncovered edge case in the `getattr` fallback branch |
| **Link** | [celery/celery#10461](https://github.com/celery/celery/pull/10461) |

</details>

<details>
<summary><b>🧬 celery#10493</b> · memory leak on the hard-timeout path</summary>

<br/>

| | |
|:---|:---|
| **Bug class** | Reference cycle / memory leak |
| **Root cause** | `traceback_clear(exc)` was silently failing because it targeted a frame still on the call stack |
| **Fix** | Removed the dead call and set `exc.__traceback__ = None` to break the exception, traceback, frame cycle |
| **Proof** | Regression test plus a hard-timeout smoke test |
| **Status** | Merged, **5.7.0** milestone |
| **Link** | [celery/celery#10493](https://github.com/celery/celery/pull/10493) |

</details>

<details>
<summary><b>🧬 celery#10497</b> · re-entrant lock deadlock in the Redis result backend</summary>

<br/>

| | |
|:---|:---|
| **Bug class** | Deadlock / re-entrancy |
| **Root cause** | A redundant `UNSUBSCRIBE` could re-enter redis-py's non-reentrant PubSub lock |
| **Fix** | Guarded the redundant call |
| **Proof** | Regression tests |
| **Status** | Merged, **5.7.0** |
| **Link** | [celery/celery#10497](https://github.com/celery/celery/pull/10497) |

</details>

<details>
<summary><b>🧬 celery#10510</b> · certificate expires mid-run, still passes</summary>

<br/>

| | |
|:---|:---|
| **Bug class** | Time-of-use gap (security) |
| **Root cause** | A certificate could expire mid-run and still pass signature checks |
| **Fix** | Explicit expiry check ahead of verification |
| **Proof** | Regression tests |
| **Status** | Merged, **5.7.0** |
| **Link** | [celery/celery#10510](https://github.com/celery/celery/pull/10510) |

</details>

<details>
<summary><b>🧬 agent-framework#7901</b> · <code>from_dict()</code> mutates the caller's input</summary>

<br/>

| | |
|:---|:---|
| **Bug class** | Unintended input mutation |
| **Root cause** | `SerializationMixin.from_dict()` updated nested dictionaries in the caller's input in place when merging dictionary-shaped dependencies |
| **Fix** | Replaced in-place updates with non-mutating merges |
| **Proof** | Regression tests |
| **Status** | Merged |
| **Link** | [microsoft/agent-framework#7901](https://github.com/microsoft/agent-framework/pull/7901) |

</details>

Also merged, among many others: a keyboard-navigable crop handles accessibility fix in [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1225), after full review. <!-- UPDATE: add your confirmed GSSoC 2026 rank / PR count here -->

<a id="projects"></a>

## `> things I've built`

Five projects. Three of them have live architecture diagrams, so you can watch the data move.

<div align="center">

| 🏎️ **278×** cancel throughput (Matching Engine) | ⚡ **−60%** synchronous load (TeamSense) | 🚀 **−25%** API response time (Django internship) | ☁️ **0** servers to manage (Personal Cloud Assistant) |
|:---:|:---:|:---:|:---:|

</div>

### 📈 [Limit Order Book & Matching Engine](https://github.com/Namraa310806/Matching-Engine) · C++20, lock-free, profiled

<div align="center">
<img src="./arch-matching-engine.svg" alt="Animated matching engine architecture: order flow into a lock-free SPSC queue, a single-writer matching core, a sorted-vector order book with intrusive lists, and a freelist object pool" width="100%" />
</div>

A price-time-priority engine with limit and market orders, partial fills, cancels and multiple instruments. Built to be **measured**, not just fast:

- **Data structure:** swapped a map/deque book for **sorted-vector + intrusive lists** (O(1) cancel), taking cancel-heavy load from **13.7K to 3.81M ops/s (278×)**.
- **Allocation:** a chunked **freelist object pool** (99.99% hit rate) added **+22–49%** throughput and up to **5× lower variance**. Profiling-driven tuning added another **+15%**, and experiments that regressed were reverted.
- **Concurrency:** a **lock-free SPSC ingestion queue** (acquire/release ordering, single-writer matching thread). ThreadSanitizer found a **real memory-ordering race**, which I fixed. I also caught a silent-drop flaw in my own benchmark and re-validated with `submitted = processed` checks. **490 test runs** under ASan/UBSan/TSan.

`C++20` `CMake` `GoogleTest` `Google Benchmark` `TSan` `ASan`

---

### 🧠 [TeamSense](https://github.com/Namraa310806/TeamSense) · async HR analytics pipeline

<div align="center">
<img src="./arch-teamsense.svg" alt="Animated TeamSense architecture: React dashboard, DRF API, Redis queue, Celery workers, PostgreSQL, all in Docker" width="100%" />
</div>

HR teams work across fragmented tools. TeamSense ingests employee data and surfaces it on one dashboard. Compute-heavy feedback processing runs in a **Celery + Redis** pipeline instead of the request cycle, which cut synchronous processing load by **60%**. Multi-entity **PostgreSQL** schema for employees, meetings and feedback, containerized with **Docker**.

`Django` `DRF` `Celery` `Redis` `PostgreSQL` `Docker` `React`

---

### ☁️ [Personal Cloud Assistant](https://github.com/Namraa310806/AWSPeronsalCloudAssistant) · serverless on AWS · [live demo](https://main.d1xrjjt0e3swym.amplifyapp.com)

<div align="center">
<img src="./arch-cloud.svg" alt="Animated Personal Cloud Assistant architecture: React, Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch" width="100%" />
</div>

Notes and file management with no servers to maintain. Stateless **Lambda + API Gateway** backend, **DynamoDB** for notes, **S3** with presigned URLs for files, **Cognito** JWT auth with **least-privilege IAM**, custom **CloudWatch** metrics (operation counts, request duration, errors) behind an admin-only dashboard, and a **React 19** frontend deployed through Amplify CI/CD.

`Lambda` `API Gateway` `DynamoDB` `S3` `Cognito` `CloudWatch` `React`

---

<table>
<tr>
<td width="50%" valign="top">

### 🎓 [ProfiLens](https://github.com/Namraa310806/ProfiLens)
**Career-prep platform**

Roadmap, exam, certificate, resume, in one flow. Normalized, indexed **PostgreSQL** schema for user progress and gamification data, which reduced read latency on high-frequency endpoints. **RBAC** with session-scoped permissions, plus a background ReportLab pipeline for certificate and resume generation.

`Django` `DRF` `PostgreSQL` `ReportLab`

</td>
<td width="50%" valign="top">

### 🎨 [Color Classifier](https://github.com/Namraa310806/ColorClassifierML)
**ML web app** · [live demo](https://namraapatel-colorclassifier.streamlit.app/)

Random Forest on a custom-labeled color dataset, with image patching and OpenCV histogram features, served as a live Streamlit app with distribution charts. Full pipeline from raw image to classified, visualized output.

`Python` `scikit-learn` `OpenCV` `NumPy` `Streamlit`

</td>
</tr>
</table>

<a id="experience"></a>

## `> experience & education`

<table>
<tr>
<td width="50%" valign="top">

### 💼 Django Backend Intern
**Techaronic IT Solutions** · Ahmedabad · Jun – Aug 2024

- Cut average API response time by **25%** under production load with database indexing and ORM query tuning
- Built authentication and authorization workflows with request validation and access control
- Deployed and debugged services on **AWS EC2** under live traffic

</td>
<td width="50%" valign="top">

### 🎓 Education
**PDEU, Gandhinagar**<br/>B.Tech, Computer Science & Business Systems<br/>**CGPA 9.61 / 10** · expected May 2028

**Government Polytechnic Ahmedabad**<br/>Diploma Engineering, Computer Science<br/>**CGPA 9.78 / 10** · Aug 2022 – Apr 2025

</td>
</tr>
</table>

<a id="skills"></a>

## `> arsenal`

Every logo below is a real logo with its name written under it, and all of them live inside one file in this repo.

<div align="center">
<img src="./stack.svg" alt="Tech stack: Python, Java, C++, JavaScript, TypeScript, PHP, Django, Flask, FastAPI, Celery, PostgreSQL, MySQL, MongoDB, Redis, AWS, Docker, Kubernetes, Linux, Git, GitHub, React, HTML5, CSS3, Bootstrap, TensorFlow, scikit-learn, OpenCV, NumPy, pandas, Hugging Face, Streamlit, Oracle OCI" width="100%" />
</div>

<details>
<summary><b>📋 Prefer it as text?</b></summary>

<br/>

| | |
|:---|:---|
| **Core CS** | Data Structures & Algorithms, Concurrency & Correctness, OOP, DBMS, Operating Systems, Computer Networks, System Design |
| **Languages** | Python, Java, C++, JavaScript, TypeScript, PHP |
| **Backend & databases** | Django, REST APIs, Flask, FastAPI, PostgreSQL, MySQL, MongoDB, Redis |
| **Cloud & systems** | AWS (EC2, Lambda, S3, DynamoDB, IAM), Docker, Kubernetes, Linux, Git |
| **AI & ML** | RAG, Transformer models, TensorFlow, Hugging Face, model evaluation |

</details>

<a id="wins"></a>

## `> scoreboard`

<div align="center">
<img src="./achievements.svg" alt="Animated achievement cards: JPMorgan Chase Code for Good winner, Open Source Hackathon winner, Amazon ML Challenge top 2,500, Google The Big Code round 2, Hackout finalist, Oracle certifications, GSSoC 2026, Celery 5.7.0" width="100%" />
</div>

<details>
<summary><b>📋 Prefer it as a table?</b></summary>

<br/>

| | Achievement |
|:---:|:---|
| 🥇 | **JPMorgan Chase Code for Good 2026**, winner, production-ready solution for a nonprofit partner |
| 🏆 | **Open Source Hackathon**, winner, production-ready features under time pressure |
| 🤖 | **Amazon ML Challenge 2025**, top 2,500 of 30,000+ |
| 🔎 | **Google The Big Code 2026**, qualified for Round 2 |
| 🏁 | **Hackout 2025**, finalist |
| 🎓 | **Oracle Certified**, OCI Generative AI Professional + Data Science Professional |
| 🌍 | **GSSoC 2026**, nationally ranked open source contributor |
| 🔥 | **Codeforces Expert (1731)** · **LeetCode 2207** |
| 📚 | **PDEU B.Tech CSBS**, CGPA 9.61 / 10, graduating May 2028 · **Diploma CS**, CGPA 9.78 / 10 |

</details>

## `> status`

<div align="center">

| 🟢 **Contributing to** | 🎯 **Looking for** | 🎓 **Studying** |
|:---:|:---:|:---:|
| Celery · Microsoft Agent Framework · Open Food Facts | Summer 2027 SDE / Backend / ML internships | B.Tech CSBS at PDEU, CGPA 9.61, graduating May 2028 |

</div>

## `> activity`

<div align="center">

<img height="170" src="https://github-readme-stats.vercel.app/api?username=Namraa310806&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true&hide_border=true&bg_color=0D1117&title_color=A78BFA&icon_color=A78BFA" alt="GitHub stats" />
<img height="170" src="https://github-readme-streak-stats.herokuapp.com/?user=Namraa310806&theme=tokyonight&hide_border=true&background=0D1117&ring=A78BFA&fire=F472B6&currStreakLabel=A78BFA" alt="GitHub streak" />

<br/><br/>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Namraa310806/Namraa310806/output/github-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Namraa310806/Namraa310806/output/github-snake.svg" />
  <img alt="Contribution snake eating my commits" src="https://raw.githubusercontent.com/Namraa310806/Namraa310806/output/github-snake-dark.svg" />
</picture>

</div>

<a id="contact"></a>

## `> contact`

<div align="center">

<a href="mailto:patelnamraa88@gmail.com">
  <img src="./cta.svg" alt="Got a bug that only shows up in production? Let's talk. patelnamraa88@gmail.com" width="100%" />
</a>

</div>

<br/>

<details>
<summary><b>🧨 please don't open this</b></summary>

<br/>

```python
Traceback (most recent call last):
  File "curiosity.py", line 1, in <module>
    raise CuriosityError("you opened the one thing marked 'do not open'")
CuriosityError: same energy that makes me read other people's stack traces for fun.

>>> hire(namraa, role="SDE / Backend / ML Intern", start="Summer 2027")
Offer sent to patelnamraa88@gmail.com
```

</details>

<br/>

<div align="center">
<img src="./footer.svg" alt="Every wrong answer builds understanding. Every merged PR builds confidence." width="100%" />
</div>
