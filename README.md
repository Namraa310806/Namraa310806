<div align="center">

<img src="./hero.svg" alt="Namraa Patel - backend engineer and open source contributor" width="100%" />

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/namraa-patel/)
[![Portfolio](https://img.shields.io/badge/Portfolio-A78BFA?style=for-the-badge&logo=firefox&logoColor=white)](https://namraa310806.github.io/Portfolio/)
[![LeetCode](https://img.shields.io/badge/LeetCode-2207-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/patelnamraa/)
[![Codeforces](https://img.shields.io/badge/Codeforces-Expert_1731-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)](https://codeforces.com/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:patelnamraa88@gmail.com)

<img src="https://komarev.com/ghpvc/?username=Namraa310806&label=Profile+Views&color=A78BFA&style=flat-square" alt="Profile views" />

</div>

<!-- UPDATE: replace https://codeforces.com/ above with https://codeforces.com/profile/<your-handle> -->

---

## `> choose your path`

<details>
<summary><b>🧑‍💼 I'm a recruiter. Give me 20 seconds.</b></summary>

<br/>

- **Looking for:** Summer 2027 SDE / Backend / ML internships
- **Proof of work:** fixes merged into **Celery** and **Microsoft Agent Framework**, plus Open Food Facts
- **Competitions:** JPMorgan Chase Code for Good 2026 winner · Amazon ML Challenge 2025 top 2,500 of 30,000+ · Codeforces Expert (1731) · LeetCode 2207
- **Industry:** Django backend intern, cut average API response time by **25%** under production load
- **Contact:** [patelnamraa88@gmail.com](mailto:patelnamraa88@gmail.com)

</details>

<details>
<summary><b>🛠️ I'm an engineer. Show me the hard stuff.</b></summary>

<br/>

Scroll to the **bug autopsies** section. Two of them are animated: a reference-cycle memory leak and a re-entrant lock deadlock, both in Celery.

</details>

<details>
<summary><b>🔧 I'm a maintainer. Will this person waste my time?</b></summary>

<br/>

Every PR below comes with a root cause, a minimal reproduction where it applies, and regression tests. In one review I flagged an uncovered edge case in my own fix before anyone asked. My first open source PR took 9 commits to merge, and I treat review feedback as the point, not an obstacle.

</details>

---

## `> what I actually do`

<div align="center">
<img src="./terminal.svg" alt="Terminal listing my upstream bug fixes" width="100%" />
</div>

I don't just add features to open source. I go looking for the bugs that hide in **concurrency, memory and shared state**, trace them to the exact line, and fix them with tests that make sure they never come back.

<div align="center">

| 🐛 **5** upstream fixes | 🧬 **5** different bug classes | 🏷️ **3** in Celery **5.7.0** | 🧪 **0** fixes without tests |
|:---:|:---:|:---:|:---:|

</div>

---

## `> the pipeline`

<div align="center">
<img src="./pipeline.svg" alt="Animated pipeline: detect, reproduce, fix and test, ship upstream" width="100%" />
</div>

---

## `> 🔬 bug autopsies`

Watch the bug happen, then watch the fix. Each animation loops: **broken first, fixed second**.

<div align="center">

### Autopsy #1 · the leak that could not be freed

<img src="./autopsy-leak.svg" alt="Animated diagram: exception, traceback and frame form a reference cycle that leaks memory until the cycle is cut" width="100%" />

### Autopsy #2 · a thread that waits on itself

<img src="./autopsy-deadlock.svg" alt="Animated diagram: a redundant UNSUBSCRIBE re-enters a non-reentrant lock and deadlocks until the call is guarded" width="100%" />

</div>

### The full case files

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

Also merged: a keyboard-navigable crop handles accessibility fix in [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1225), after full review. <!-- UPDATE: add your confirmed GSSoC 2026 rank / PR count here -->

---

## `> things I've built`

<table>
<tr>
<td width="50%" valign="top">

### [TeamSense](https://github.com/Namraa310806/TeamSense)
**Async HR analytics pipeline**

Moved compute-heavy feedback processing off the request cycle with Celery + Redis, cutting synchronous processing load by **60%**. Multi-entity PostgreSQL schema, Dockerized for reproducible dev and prod, React dashboard.

`Django` `DRF` `Celery` `Redis` `PostgreSQL` `Docker` `React`

</td>
<td width="50%" valign="top">

### [Personal Cloud Assistant](https://github.com/Namraa310806/AWSPeronsalCloudAssistant)
**Fully serverless on AWS, zero EC2** · [live demo](https://main.d1xrjjt0e3swym.amplifyapp.com)

Lambda + API Gateway backend, DynamoDB and S3 with presigned URLs, Cognito JWT auth with least-privilege IAM, CloudWatch custom metrics behind an admin dashboard, React 19 on Amplify CI/CD.

`Lambda` `API Gateway` `DynamoDB` `S3` `Cognito` `React`

</td>
</tr>
<tr>
<td width="50%" valign="top">

### [ProfiLens](https://github.com/Namraa310806/ProfiLens)
**Career-prep platform**

Roadmap, exam, certificate, resume in one flow. Normalized and indexed PostgreSQL schema for gamification data, RBAC with session-scoped permissions, background PDF generation for certificates and resumes.

`Django` `DRF` `PostgreSQL` `ReportLab`

</td>
<td width="50%" valign="top">

### [Color Classifier](https://github.com/Namraa310806/ColorClassifierML)
**ML web app** · [live demo](https://namraapatel-colorclassifier.streamlit.app/)

Random Forest on a custom-labeled dataset with OpenCV histogram features, deployed as a live Streamlit app with distribution charts.

`Python` `scikit-learn` `OpenCV` `Streamlit`

</td>
</tr>
</table>

---

## `> arsenal`

<div align="center">

**Languages**

<img src="https://skillicons.dev/icons?i=python,java,cpp,js,ts,php&perline=6" alt="Languages" />

**Backend and databases**

<img src="https://skillicons.dev/icons?i=django,flask,fastapi,postgres,mysql,mongodb,redis&perline=7" alt="Backend and databases" />

![Celery](https://img.shields.io/badge/Celery-37814A?style=flat-square&logo=celery&logoColor=white)
![DRF](https://img.shields.io/badge/Django_REST_Framework-A30000?style=flat-square&logo=django&logoColor=white)
![REST APIs](https://img.shields.io/badge/REST_APIs-0f0c29?style=flat-square)

**Cloud, systems and DevOps**

<img src="https://skillicons.dev/icons?i=aws,docker,kubernetes,linux,git,github&perline=6" alt="Cloud and DevOps" />

![EC2](https://img.shields.io/badge/EC2-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![Lambda](https://img.shields.io/badge/Lambda-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![S3](https://img.shields.io/badge/S3-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![DynamoDB](https://img.shields.io/badge/DynamoDB-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![IAM](https://img.shields.io/badge/IAM-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![Cognito](https://img.shields.io/badge/Cognito-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![API Gateway](https://img.shields.io/badge/API_Gateway-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![CloudWatch](https://img.shields.io/badge/CloudWatch-FF9900?style=flat-square&logo=amazonaws&logoColor=white)

**Frontend**

<img src="https://skillicons.dev/icons?i=react,html,css,bootstrap&perline=4" alt="Frontend" />

**AI and ML**

<img src="https://skillicons.dev/icons?i=tensorflow,sklearn,opencv,numpy,pandas&perline=5" alt="ML libraries" />

![Hugging Face](https://img.shields.io/badge/Hugging_Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)
![RAG](https://img.shields.io/badge/RAG-302b63?style=flat-square)
![Transformers](https://img.shields.io/badge/Transformer_Models-302b63?style=flat-square)
![Model Evaluation](https://img.shields.io/badge/Model_Evaluation-302b63?style=flat-square)
![OCI GenAI](https://img.shields.io/badge/OCI_Generative_AI_Professional-F80000?style=flat-square&logo=oracle&logoColor=white)

**Core CS**

![DSA](https://img.shields.io/badge/Data_Structures_%26_Algorithms-24243e?style=flat-square)
![Concurrency](https://img.shields.io/badge/Concurrency_%26_Correctness-24243e?style=flat-square)
![OOP](https://img.shields.io/badge/OOP-24243e?style=flat-square)
![DBMS](https://img.shields.io/badge/DBMS-24243e?style=flat-square)
![OS](https://img.shields.io/badge/Operating_Systems-24243e?style=flat-square)
![CN](https://img.shields.io/badge/Computer_Networks-24243e?style=flat-square)
![System Design](https://img.shields.io/badge/System_Design-24243e?style=flat-square)

</div>

---

## `> scoreboard`

<div align="center">

| | Achievement |
|:---:|:---|
| 🥇 | **JPMorgan Chase Code for Good 2026**, winner, production-ready solution for a nonprofit partner |
| 🌍 | **GSSoC 2026**, nationally ranked open source contributor |
| 🏆 | **Open Source Hackathon**, winner, production-ready features under time pressure |
| 🤖 | **Amazon ML Challenge 2025**, top 2,500 of 30,000+ |
| 🔎 | **Google The Big Code 2026**, qualified for Round 2 |
| 🏁 | **Hackout 2025**, finalist |
| 🔥 | **Codeforces Expert (1731)** · **LeetCode 2207** |
| 🎓 | **Oracle Certified**, OCI Generative AI Professional + Data Science Professional |
| 📚 | **PDEU B.Tech CSBS**, CGPA 9.46 / 10, graduating 2028 |

</div>

---

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

---

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

*"My first open-source PR took 9 commits to get merged. Every one of them taught me something."*

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:24243e,50:302b63,100:0f0c29&height=110&section=footer" alt="" width="100%" />

</div>
