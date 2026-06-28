---
title: Home
---


<div style="min-height:calc(100vh - 52px); display:flex; align-items:center;">
<div style="display:flex; gap:28px; align-items:center; width:100%;">
  <div style="flex:1; min-width:0;">
    <p>I'm currently an undergraduate student at UBC and research assistant in the <a href="https://nlp.cs.ubc.ca/">UBC NLP Group</a>, advised by <a href="https://peterwestai.notion.site/">Peter West</a>. I'm interested in using our insights towards how human think, learn and develop to improve how models generate, reason, and evolve. My recent research interests include:</p>
    <ul>
      <li><strong>Creativity</strong>. Understanding how intelligent systems can generate genuinely novel ideas rather than converging toward consensus.</li>
      <li><strong>Memory</strong>. Exploring memory as a fundamental component of intelligence, not merely as information storage.</li>
      <li><strong>Non-language communication/reasoning</strong>. Investigating forms of reasoning and conversation that extend beyond human language.</li>
    </ul>
  </div>
  <div style="flex:0 0 auto; width:180px; text-align:center;">
    <a>
      <img src="/img/Qingyun_Qian.png" style="width:100%; display:block;" alt="Qingyun portrait" />
    </a>
    <div style="margin-top:10px; font-size:0.8em; display:flex; justify-content:center; gap:8px; flex-wrap:wrap;">
      <a href="https://github.com/TriangleMesh">GitHub</a>
      <a href="mailto:trianglemeshq@gmail.com">Email</a>
      <a href="https://scholar.google.com/citations?user=g8avwkcAAAAJ&hl=en">Google Scholar</a>
    </div>
  </div>
</div>
</div>

---

### Experience

I am currently pursuing a B.Sc. in Computer Science at UBC, and hold an Bachelor of Laws (LL.B.) from Fujian Agriculture and Forestry University. Prior to transitioning into computer science, I worked as a legal assistant in the China legal team at Royal DSM (now [dsm-firmenich](https://www.dsm-firmenich.com/en/home.html)). 

At UBC, I have worked across multiple research areas. My work includes co-authoring *Position: Universal Aesthetic Alignment Narrows Artistic Expression* (ICML 2026 Spotlight) with [Khalad Hasan](https://cmps-people.ok.ubc.ca/mkhasan/) and [Shan Du](https://cmps.ok.ubc.ca/about/contact/shan-du/); conducting travel behavior data collection, cleaning, and analysis with Khalad Hasan and [Mahmudur Fatmi](https://engineering.ok.ubc.ca/about/contact/mahmudur-fatmi/); exploring computational creativity with [Liane Gabora](https://gabora-psych.ok.ubc.ca/); and studying myelin and memory with [Reza Khanbabaie](https://ca.linkedin.com/in/rkhanbabaie).


---

### Selected Projects

**[Position: Universal Aesthetic Alignment Narrows Artistic Expression](https://arxiv.org/abs/2512.11883)**
*ICML 2026 Spotlight (top 5%)* · W. Guo, **Q. Qian**, K. Hasan, S. Du · [Code & Data](https://github.com/weathon/icml2026_position) · [Project Page](https://weathon.github.io/icml2026_position/) · [ICML Page](https://icml.cc/virtual/2026/poster/67242)

When users request "anti-aesthetic" outputs for artistic or critical purposes, over-aligned generative models default to conventionally beautiful results anyway — and reward models penalize anti-aesthetic images even when they perfectly match the user's prompt. We argue this reflects a systematic bias that compromises user autonomy and aesthetic pluralism.

**[BCATUS: Travel Behavior Tracking](https://bcatus.ok.ubc.ca/)** 
*Oct. 2024 – Apr. 2026*

Built an iOS app ([BCATUS](https://apps.apple.com/ca/app/bc-atus/id6467030994)) in Swift that passively tracked the travel behavior of 500+ users across Metro Vancouver and the Okanagan, collecting 12,600+ trips to inform government transportation policy. On the data pipeline side, we designed algorithms to impute missing trip-purpose labels (11.2% relative improvement), detect and remove erroneous indoor GPS loops, and merge fragmented transit segments — automating 95%+ of trip processing and cutting analysis time by 50%.

**AI-Powered Exam Generation Platform** · *React · Node.js · PostgreSQL · Docker · BERT*
*May – Aug. 2025*

A microservice-based platform for automated university exam generation. I designed a semantic question recommendation service via cosine similarity over BERT embeddings, with time-versioned caching that cuts redundant computation by 85–95%. I also designed a logic-preserving shuffle algorithm using dependency chains and similarity clustering, and replaced O(n!) permutation generation with O(1) precomputed lookup for 2–5 option questions — achieving up to 24× speedup for the most common cases.


