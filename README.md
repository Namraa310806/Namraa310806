<div align="center">

<img src="./hero.svg" alt="Namraa Patel - backend engineer and open source contributor" width="100%" />

<br/>

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/namraa-patel/)
[![Portfolio](https://img.shields.io/badge/Portfolio-A78BFA?style=for-the-badge&logo=firefox&logoColor=white)](https://namraa310806.github.io/Portfolio/)
[![LeetCode](https://img.shields.io/badge/LeetCode-2207-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)](https://leetcode.com/u/patelnamraa/)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:patelnamraa88@gmail.com)

<img src="https://komarev.com/ghpvc/?username=Namraa310806&label=Profile+Views&color=A78BFA&style=flat-square" alt="Profile views" />

</div>

<!-- UPDATE: Codeforces badge needs your handle -> https://codeforces.com/profile/<handle> -->

---

## `> what I actually do`

<div align="center">
<img src="./terminal.svg" alt="Terminal listing my upstream bug fixes" width="100%" />
</div>

I don't just add features to open source. I go looking for the bugs that hide in **concurrency, memory and shared state**, trace them to the exact line, and fix them with tests that make sure they never come back.

---

## `> the pipeline`

<div align="center">
<img src="./pipeline.svg" alt="Animated pipeline: detect, reproduce, fix and test, ship upstream" width="100%" />
</div>

---

## `> bug autopsies`

Real bugs, real root causes, real maintainers reviewing.

| PR | What was broken | Root cause | Outcome |
|:---|:---|:---|:---|
| [celery#10461](https://github.com/celery/celery/pull/10461) | `autoretry_for` shared state | A mutable default `retry_kwargs` dict was aliased across task retries | Minimal repro and fix; flagged an uncovered edge case in the `getattr` fallback during review |
| [celery#10493](https://github.com/celery/celery/pull/10493) | Memory leak on hard timeout | `traceback_clear(exc)` silently failed because it targeted a frame still on the call stack | Removed the dead call, broke the exception-traceback-frame cycle with `exc.__traceback__ = None`; regression and smoke tests; **5.7.0** |
| [celery#10497](https://github.com/celery/celery/pull/10497) | Deadlock in the Redis result backend | A redundant `UNSUBSCRIBE` re-entered redis-py's non-reentrant PubSub lock | Guarded the call; regression tests; **5.7.0** |
| [celery#10510](https://github.com/celery/celery/pull/10510) | Certificate verification gap | A cert could expire mid-run and still pass signature checks (time-of-use gap) | Explicit expiry check ahead of verification; regression tests; **5.7.0** |
| [agent-framework#7901](https://github.com/microsoft/agent-framework/pull/7901) | `SerializationMixin.from_dict()` mutating input | Merging dict-shaped dependencies updated the caller's nested dicts in place | Non-mutating merges and regression tests; **merged** |

Also: accessibility fix for keyboard-navigable crop handles in [Open Food Facts Explorer](https://github.com/openfoodfacts/openfoodfacts-explorer/pull/1225), merged after full review. <!-- UPDATE: add your current GSSoC 2026 rank / PR count here once you confirm which number is right -->

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

<img src="https://skillicons.dev/icons?i=python,java,cpp,js,ts,php,django,flask,fastapi,react,postgres,mysql,mongodb,redis,aws,docker,kubernetes,linux,git,tensorflow&perline=10" alt="Tech stack" />

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
