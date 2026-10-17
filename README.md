<div align="center">

<img src="./assets/hero.svg" alt="Namraa Patel: backend and systems engineer, open source bug hunter" width="100%" />

<br/>

**I find difficult bugs in concurrency, memory and shared state, then get the fixes upstream.**

[![GitHub](https://img.shields.io/badge/GitHub-Namraa310806-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Namraa310806)
[![Portfolio](https://img.shields.io/badge/Portfolio-A78BFA?style=for-the-badge&logo=firefox&logoColor=white)](https://namraa310806.github.io/Portfolio/)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/namraa-patel/)
[![Codeforces](https://img.shields.io/badge/Codeforces-Expert_1731-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)](https://codeforces.com/profile/np254)
[![LeetCode](https://img.shields.io/badge/LeetCode-2207-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/patelnamraa08/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:patelnamraa88@gmail.com)

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
- **Proof of work:** 5 fixes merged into **Celery** and **Microsoft Agent Framework**, plus Open Food Facts
- **Systems:** C++20 matching engine, **13.7K → 3.81M ops/s (278×)** on cancel-heavy load, validated under TSan/ASan/UBSan
- **Competitions:** JPMorgan Chase Code for Good 2026 winner · Amazon ML Challenge 2025 top 2,500 of 30,000+ · Codeforces Expert (1731) · LeetCode 2207
- **Industry:** Django backend intern, cut average API response time by **25%** under production load
- **Contact:** [patelnamraa88@gmail.com](mailto:patelnamraa88@gmail.com)

</details>

<details>
<summary><b>🛠️ I'm an engineer. Show me the hard stuff.</b></summary>

<br/>

Jump to the [**bug autopsies**](#autopsies): two animated walkthroughs of a reference-cycle memory leak and a re-entrant lock deadlock, both in Celery. Then see the [**matching engine**](#systems), where ThreadSanitizer caught a real memory-ordering race in my own lock-free queue.

</details>

<details>
<summary><b>🔧 I'm a maintainer. Will this person waste my time?</b></summary>

<br/>

Every PR featured below comes with a root cause, a minimal reproduction where it applies, and regression tests. In one review I flagged an uncovered edge case in my own fix before anyone asked. My first open source PR took 9 commits to merge, and I treat review feedback as the point, not an obstacle.

</details>

## `> whoami`

```text
$ whoami

namraa@upstream

role       backend / systems engineer
specialty  concurrency · memory · shared state
hunting    race conditions · deadlocks · leaks
weapon     minimal reproductions + regression tests
habitat    Celery · Python · C++
status     open to Summer 2027 SDE / Backend / ML internships
```

<a id="proof"></a>

## `> the 20-second version`

<div align="center">

| **5** | **5** | **4** | **278×** | **9.61** |
|:---:|:---:|:---:|:---:|:---:|
| featured contributions | bug classes | merged in Celery | matching throughput | B.Tech CGPA |

**5 featured PRs: 4 in Celery, 1 in Microsoft Agent Framework · plus Open Food Facts**

<img src="./assets/upstream.svg" alt="Terminal listing five merged upstream fixes: four in Celery and one in Microsoft Agent Framework" width="100%" />

<sub>Selected highlights from a broader open-source contribution history. Everything on GitHub: [Celery](https://github.com/celery/celery/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Microsoft Agent Framework](https://github.com/microsoft/agent-framework/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged) · [Open Food Facts](https://github.com/openfoodfacts/openfoodfacts-explorer/pulls?q=is%3Apr+author%3ANamraa310806+is%3Amerged)</sub>

</div>

<a id="autopsies"></a>

## `> bug autopsies`

| Bug | System | Failure | Fix |
|:---|:---|:---|:---|
| 🧠 Memory leak | Celery | reference cycle on the hard-timeout path | traceback cleanup that actually runs |
| 🔒 Deadlock | Celery · Redis backend | re-entrant lock | guard on the redundant `UNSUBSCRIBE` |
| 🧬 State leak | Celery | mutable default aliased across retries | retry state no longer shared |
| 🔐 Time-of-use gap | Celery | expired certificate still verified | explicit expiry check |
| 🧱 Input mutation | Agent Framework | caller's nested dicts changed in place | non-mutating merge |

**Pick one to investigate.** Each animation loops: broken first, fixed second.

<div align="center">

### Autopsy #1 · the leak that could not be freed

<img src="./assets/autopsies/leak.svg" alt="Animated diagram: exception, traceback and frame form a reference cycle that leaks memory until the cycle is cut" width="100%" />

### Autopsy #2 · a thread that waits on itself

<img src="./assets/autopsies/deadlock.svg" alt="Animated diagram: a redundant UNSUBSCRIBE re-enters a non-reentrant lock and deadlocks until the call is guarded" width="100%" />

</div>

### Case files

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
| **Status** | Merged, **5.7.0** milestone |
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
| **Status** | Merged, **5.7.0** milestone |
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

Also merged: a keyboard-navigable crop handles accessibility fix in [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1225), after full review. 

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
<img src="./assets/achievements.svg" alt="Animated achievement cards: JPMorgan Chase Code for Good winner, Open Source Hackathon winner, Amazon ML Challenge top 2,500, Google The Big Code round 2, Hackout finalist, Oracle certifications, Codeforces Expert, Celery 5.7.0" width="100%" />
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
| 🔥 | **Codeforces Expert (1731)** · **LeetCode 2207** |

</details>

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