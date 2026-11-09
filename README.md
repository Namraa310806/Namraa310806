<div align="center">

<img src="./assets/hero.svg" alt="Namraa Patel: backend and systems engineer, open source bug hunter" width="100%" />

<br/>

**I find difficult bugs in concurrency, memory and shared state, then get the fixes merged into Celery, Django, Microsoft Agent Framework and Open Food Facts.**

[![Django](https://img.shields.io/badge/Django-Contributor-092E20?style=for-the-badge&logo=django&logoColor=white)](https://github.com/django/django/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)
[![Celery](https://img.shields.io/badge/Celery-Contributor-37814A?style=for-the-badge&logo=celery&logoColor=white)](https://github.com/celery/celery/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)
[![Microsoft Agent Framework](https://img.shields.io/badge/Microsoft_Agent_Framework-Contributor-0078D4?style=for-the-badge&logo=github&logoColor=white)](https://github.com/microsoft/agent-framework/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)
[![Open Food Facts](https://img.shields.io/badge/Open_Food_Facts-Contributor-FF8714?style=for-the-badge&logoColor=white)](https://github.com/openfoodfacts/openfoodfacts-explorer/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)

[![GitHub](https://img.shields.io/badge/GitHub-Namraa310806-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Namraa310806)
[![Portfolio](https://img.shields.io/badge/Portfolio-A78BFA?style=for-the-badge&logo=firefox&logoColor=white)](https://namraa310806.github.io/Portfolio/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/namraa-patel/)
[![Codeforces](https://img.shields.io/badge/Codeforces-Expert_1704-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)](https://codeforces.com/profile/hacker08)
[![LeetCode](https://img.shields.io/badge/LeetCode-2207-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/patelnamraa08/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:patelnamraa88@gmail.com)

<img src="./assets/orgs.svg" alt="Open source contributor to Celery, Django, Microsoft Agent Framework and Open Food Facts" width="100%" />

<a href="#proof"><b>Proof</b></a> · <a href="#autopsies"><b>Bug autopsies</b></a> · <a href="#systems"><b>Systems</b></a> · <a href="#experience"><b>Experience</b></a> · <a href="#contact"><b>Contact</b></a>

<br/>

<img src="./assets/winner.svg" alt="Winner of JPMorgan Chase Code for Good 2026" width="100%" />

</div>

## `> choose your path`

<details open>
<summary><b>🧑‍💼 I'm a recruiter. Give me 20 seconds.</b></summary>

<br/>

- **Looking for:** Summer 2027 SDE / Backend / ML internships
- **Education:** B.Tech CSBS at PDEU, **CGPA 9.61 / 10** (graduating May 2028) · Diploma in CS, CGPA 9.78
- **Proof of work:** Merged fixes in **Celery**, **Django**, **Microsoft Agent Framework** and **Open Food Facts**, shipped in Celery 5.7.0 and Agent Framework 1.19.0 / 1.22.0
- **Systems:** C++20 matching engine, **13.7K → 3.81M ops/s (278×)** on cancel-heavy load, validated under TSan/ASan/UBSan
- **Competitions:** JPMorgan Chase Code for Good 2026 winner · Amazon ML Challenge 2025 top 2,500 of 30,000+ · Codeforces Expert (1704) · LeetCode 2207
- **Industry:** Django backend intern, cut average API response time by **25%** under production load
- **Contact:** [patelnamraa88@gmail.com](mailto:patelnamraa88@gmail.com)

</details>

<details>
<summary><b>🛠️ I'm an engineer. Show me the hard stuff.</b></summary>

<br/>

Jump to the [**bug autopsies**](#autopsies): a Redis chord race in Celery, a Django ORM overwrite bug, workflow state leaking between runs in Agent Framework, plus two animated walkthroughs (a reference-cycle leak and a re-entrant lock deadlock). Then see the [**matching engine**](#systems), where ThreadSanitizer caught a real memory-ordering race in my own lock-free queue.

</details>

<details>
<summary><b>🔧 I'm a maintainer. Will this person waste my time?</b></summary>

<br/>

Every bug fix featured below comes with a root cause and regression tests. In one review I flagged an uncovered edge case in my own fix before anyone asked. In Django, maintainer feedback shrank my patch to a smaller change confined to `DeferredAttribute`, and the fix is better for it. My first open source PR took 9 commits to merge, and I treat review feedback as the point, not an obstacle.

</details>

## `> whoami`

```text
$ whoami

namraa@upstream

role       backend / systems engineer
specialty  concurrency · memory · shared state
hunting    race conditions · deadlocks · leaks
weapon     minimal reproductions + regression tests
habitat    Celery · Django · Agent Framework · C++
status     open to Summer 2027 SDE / Backend / ML internships
```

<a id="proof"></a>

## `> the 20-second version`

<div align="center">

| **4** | **278×** | **9.61** | **25%** |
|:---:|:---:|:---:|:---:|
| open-source orgs | matching throughput | B.Tech CGPA | API latency cut |

<img src="./assets/upstream.svg" alt="Terminal listing eight featured merged upstream pull requests across Celery, Django, Microsoft Agent Framework and Open Food Facts" width="100%" />

<sub>Featured highlights. Everything on GitHub: [Celery](https://github.com/celery/celery/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Django](https://github.com/django/django/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Microsoft Agent Framework](https://github.com/microsoft/agent-framework/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Open Food Facts](https://github.com/openfoodfacts/openfoodfacts-explorer/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)</sub>

</div>

<a id="autopsies"></a>

## `> bug autopsies`

| Bug | System | What broke | The fix | PR |
|:---|:---|:---|:---|:---|
| 🔀 Race condition | Celery · Redis chord | concurrent completions could publish the callback twice | atomic Lua-scripted claim, unique tokens, ownership-checked cleanup | [#10998](https://github.com/celery/celery/pull/10998) |
| 🧬 Overwritten state | Django · ORM | `FETCH_PEERS` deferred fetch overwrote loaded or modified values | fix confined to `DeferredAttribute`, with tests | [#22177](https://github.com/django/django/pull/22177) |
| 🌊 State leak | Agent Framework · workflows | a failed or cancelled superstep's staged writes leaked into later runs | pending state discarded on failure (**1.22.0**) | [#8819](https://github.com/microsoft/agent-framework/pull/8819) |
| 🧠 Memory leak | Celery · hard timeout | `traceback_clear(exc)` silently failed on a live frame | `exc.__traceback__ = None` breaks the cycle (**5.7.0**) | [#10493](https://github.com/celery/celery/pull/10493) |
| 🔒 Deadlock | Celery · Redis backend | redundant `UNSUBSCRIBE` re-entered a non-reentrant lock | guarded the call (**5.7.0**) | [#10497](https://github.com/celery/celery/pull/10497) |
| 💬 Lost context | Agent Framework · compaction | synthetic summaries silently discarded on the next run | reconciliation in `before_run` (**1.19.0**) | [#7944](https://github.com/microsoft/agent-framework/pull/7944) |
| 🔐 Time-of-use gap | Celery · security | certificate expiring mid-run still passed verification | explicit expiry check first (**5.7.0**) | [#10510](https://github.com/celery/celery/pull/10510) |
| 🍎 Feature | Open Food Facts Explorer | search had no brand suggestions | brand autocomplete via taxonomy fallback | [#1766](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1766) |

**Pick one to investigate.** Each animation loops: broken first, fixed second.

<div align="center">

### Autopsy #1 · the leak that could not be freed

<img src="./assets/autopsies/leak.svg" alt="Animated diagram: exception, traceback and frame form a reference cycle that leaks memory until the cycle is cut" width="100%" />

### Autopsy #2 · a thread that waits on itself

<img src="./assets/autopsies/deadlock.svg" alt="Animated diagram: a redundant UNSUBSCRIBE re-enters a non-reentrant lock and deadlocks until the call is guarded" width="100%" />

</div>

<details>
<summary><b>➕ More merged work</b> · Celery, Agent Framework, Open Food Facts</summary>

<br/>

| Project | PR | Change |
|:---|:---|:---|
| Celery | [#10884](https://github.com/celery/celery/pull/10884) | stamped header revocation |
| Celery | [#10822](https://github.com/celery/celery/pull/10822) | redact Azure credentials in diagnostic output |
| Celery | [#10836](https://github.com/celery/celery/pull/10836) | avoid `get_state` for plain chain failures |
| Celery | [#10876](https://github.com/celery/celery/pull/10876) | task execution after ETA conversion failure |
| Celery | [#10866](https://github.com/celery/celery/pull/10866) | database backend cleanup timezone |
| Celery | [#10701](https://github.com/celery/celery/pull/10701) | missing ArangoDB documents on delete |
| Celery | [#10513](https://github.com/celery/celery/pull/10513) | invalid worker environment values |
| Celery | [#10461](https://github.com/celery/celery/pull/10461) | shared `retry_kwargs` in `autoretry_for` |
| Celery | [#10520](https://github.com/celery/celery/pull/10520) | pre-commit formatting fix |
| Agent Framework | [#9796](https://github.com/microsoft/agent-framework/pull/9796) | Bedrock parallel tool-result handling |
| Agent Framework | [#9469](https://github.com/microsoft/agent-framework/pull/9469) | stateless shell tools inheriting host stdin |
| Agent Framework | [#7901](https://github.com/microsoft/agent-framework/pull/7901) | `from_dict()` mutating the caller's input |
| Open Food Facts | [#1851](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1851) | preserve missing country data in the countries map |
| Open Food Facts | [#1225](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1225) | keyboard-navigable crop handles (a11y) |

</details>

<a id="systems"></a>

## `> 01 / matching engine`

### [Limit Order Book & Matching Engine](https://github.com/Namraa310806/Matching-Engine) · 278× faster cancel-heavy workload

```text
13.7K ops/s  ──────────────────────────────►  3.81M ops/s
C++20 · lock-free SPSC · single-writer architecture
```

<div align="center">
<img src="./assets/architectures/matching-engine.svg" alt="Animated matching engine architecture: order flow into a lock-free SPSC queue, a single-writer matching core, a sorted-vector order book with intrusive lists, and a freelist object pool" width="100%" />
</div>

A price-time-priority engine with limit and market orders, partial fills, cancels and multiple instruments. Built to be **measured**, not just fast:

- **Data structure:** swapped a map/deque book for **sorted-vector + intrusive lists** (O(1) cancel), taking cancel-heavy load from **13.7K to 3.81M ops/s (278×)**.
- **Allocation:** a chunked **freelist object pool** (99.99% hit rate) added **+22–49%** throughput and up to **5× lower variance**. Profiling-driven tuning added another **+15%**, and experiments that regressed were reverted.
- **Concurrency:** a **lock-free SPSC ingestion queue** (acquire/release ordering, single-writer matching thread). ThreadSanitizer found a **real memory-ordering race**, which I fixed. I also caught a silent-drop flaw in my own benchmark and re-validated with `submitted = processed` checks. **490 test runs** under ASan/UBSan/TSan.

`C++20` `CMake` `GoogleTest` `Google Benchmark` `TSan` `ASan`

## `> more systems`

### 🧠 [TeamSense](https://github.com/Namraa310806/TeamSense) · async HR analytics pipeline · **−60%** synchronous load

<div align="center">
<img src="./assets/architectures/teamsense.svg" alt="Animated TeamSense architecture: React dashboard, DRF API, Redis queue, Celery workers, PostgreSQL, all in Docker" width="100%" />
</div>

HR teams work across fragmented tools. TeamSense ingests employee data and surfaces it on one dashboard. Compute-heavy feedback processing runs in a **Celery + Redis** pipeline instead of the request cycle, which cut synchronous processing load by **60%**. Multi-entity **PostgreSQL** schema for employees, meetings and feedback, containerized with **Docker**.

`Django` `DRF` `Celery` `Redis` `PostgreSQL` `Docker` `React`

### ☁️ [Personal Cloud Assistant](https://github.com/Namraa310806/AWSPeronsalCloudAssistant) · serverless on AWS · [live demo](https://main.d1xrjjt0e3swym.amplifyapp.com)

<div align="center">
<img src="./assets/architectures/cloud-assistant.svg" alt="Animated Personal Cloud Assistant architecture: React, Cognito, API Gateway, Lambda, DynamoDB, S3, CloudWatch" width="100%" />
</div>

Notes and file management with no servers to maintain. Stateless **Lambda + API Gateway** backend, **DynamoDB** for notes, **S3** with presigned URLs for files, **Cognito** JWT auth with **least-privilege IAM**, custom **CloudWatch** metrics (operation counts, request duration, errors) behind an admin-only dashboard, and a **React 19** frontend deployed through Amplify CI/CD.

`Lambda` `API Gateway` `DynamoDB` `S3` `Cognito` `CloudWatch` `React`

### 🎓 [ProfiLens](https://github.com/Namraa310806/ProfiLens) · career-prep platform

Roadmap, exam, certificate, resume, in one flow. Normalized, indexed **PostgreSQL** schema for user progress and gamification data, which reduced read latency on high-frequency endpoints. **RBAC** with session-scoped permissions, plus a background ReportLab pipeline for certificate and resume generation.

`Django` `DRF` `PostgreSQL` `ReportLab`

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

## `> arsenal`

| | |
|:---|:---|
| **Languages** | `Python` `C++` `Java` `JavaScript` `TypeScript` `PHP` |
| **Backend** | `Django` `DRF` `FastAPI` `Flask` `Celery` |
| **Data** | `PostgreSQL` `Redis` `MongoDB` `MySQL` |
| **Systems / Cloud** | `AWS` `Docker` `Kubernetes` `Linux` `Git` |
| **ML** | `scikit-learn` `TensorFlow` `OpenCV` `Hugging Face` |
| **Core CS** | DSA, Concurrency & Correctness, OOP, DBMS, OS, Networks, System Design |

## `> scoreboard`

<div align="center">
<img src="./assets/achievements.svg" alt="Animated achievement cards: JPMorgan Chase Code for Good winner, Open Source Hackathon winner, Amazon ML Challenge top 2,500, Google The Big Code round 2, Hackout finalist, Oracle certifications, Codeforces Expert, open source" width="100%" />
</div>

<a id="contact"></a>

## `> contact`

<div align="center">

<a href="mailto:patelnamraa88@gmail.com">
  <img src="./assets/cta.svg" alt="Got a bug that only shows up in production? Let's talk. patelnamraa88@gmail.com" width="100%" />
</a>

</div>

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
