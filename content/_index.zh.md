---
title: 首页
---

<div class="hero-section" style="min-height:calc(100vh - 52px); display:flex; flex-direction:column; justify-content:center; position:relative;">
<div class="hero-container" style="display:flex; gap:28px; align-items:center; width:100%;">
  <div class="hero-text" style="flex:1; min-width:0;">
    <p>我目前是 UBC 计算机科学本科生，也是 <a href="https://nlp.cs.ubc.ca/">UBC NLP 研究组</a>的研究助理，导师是 <a href="https://peterwestai.notion.site/">Peter West</a>。我希望借助对人类思考、学习和发展过程的理解，改进模型的生成、推理和演化方式。近期研究兴趣包括：</p>
    <ul>
      <li><strong>创造力</strong>：理解智能系统如何产生真正新颖的想法，而非趋向共识。</li>
      <li><strong>记忆</strong>：探索记忆作为智能的基本组成部分的作用，而不仅将其视为信息存储。</li>
      <li><strong>非语言交流与推理</strong>：研究超越人类语言的推理与交流形式。</li>
    </ul>
  </div>
  <div class="hero-photo" style="flex:0 0 auto; width:180px; text-align:center;">
    <a>
      <picture>
        <source srcset="/img/Qingyun_Qian.webp" type="image/webp">
        <img src="/img/Qingyun_Qian.png" style="width:100%; display:block;" alt="Qingyun 的照片" />
      </picture>
    </a>
    <div style="margin-top:10px; font-size:0.72em; display:flex; justify-content:center; gap:6px; flex-wrap:nowrap; white-space:nowrap;">
      <a href="https://github.com/TriangleMesh">GitHub</a>
      <a href="https://x.com/qingyunqian?s=11">X/Twitter</a>
      <a href="mailto:trianglemeshq@gmail.com">Email</a>
      <a href="https://scholar.google.com/citations?user=g8avwkcAAAAJ&hl=en">Google Scholar</a>
    </div>
  </div>
</div>
<section class="news-section" aria-labelledby="news">
  <h3 id="news">近况</h3>
  <ol class="news-timeline" role="list">
    <li>
      <time class="news-date" datetime="2026-10-07">2026.10</time>
      <p class="news-description">入选 Lambda 的研究资助计划。感谢 Lambda！</p>
    </li>
    <li>
      <time class="news-date" datetime="2026-04-30">2026.04</time>
      <p class="news-description">我们的论文 <a href="https://arxiv.org/abs/2512.11883">Position: Universal Aesthetic Alignment Narrows Artistic Expression</a> 被 ICML 2026 接收为 Spotlight！我将于 7 月 6–11 日在首尔，欢迎来打招呼！</p>
    </li>
  </ol>
</section>
<a href="#experience" class="scroll-indicator" aria-label="向下查看经历">
  <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="6 9 12 15 18 9"></polyline></svg>
</a>
</div>

### 经历 {#experience}

我目前在 UBC 攻读计算机科学理学学士学位，此前获得了福建农林大学的法学学士学位。在转向计算机科学之前，我曾在荷兰皇家帝斯曼（现为 [dsm-firmenich](https://www.dsm-firmenich.com/en/home.html)）的中国法律团队担任法律助理。

在 UBC，我参与过多个领域的研究，包括与 [Khalad Hasan](https://cmps-people.ok.ubc.ca/mkhasan/) 和 [Shan Du](https://cmps.ok.ubc.ca/about/contact/shan-du/) 共同撰写 *Position: Universal Aesthetic Alignment Narrows Artistic Expression*（ICML 2026 Spotlight）；与 Khalad Hasan 和 [Mahmudur Fatmi](https://engineering.ok.ubc.ca/about/contact/mahmudur-fatmi/) 开展出行行为数据的收集、清理和分析；与 [Liane Gabora](https://gabora-psych.ok.ubc.ca/) 探索计算创造力；以及与 [Reza Khanbabaie](https://ca.linkedin.com/in/rkhanbabaie) 研究髓鞘与记忆。

---

<section class="selected-projects" aria-labelledby="selected-projects">
<h3 id="selected-projects">代表项目</h3>

<article class="selected-project">
<h4>Position: Universal Aesthetic Alignment Narrows Artistic Expression</h4>
<p class="project-details">ICML 2026 · Spotlight（前 5%）</p>
<p class="project-details">W. Guo、<strong>Q. Qian</strong>、K. Hasan、S. Du</p>

当用户出于艺术或批评目的要求生成“反审美”的内容时，过度对齐的生成模型仍然默认产出符合传统审美的结果；即使反审美图像完全符合用户提示，奖励模型也会对其施加惩罚。我们认为，这反映了一种系统性偏见，损害了用户自主权和审美多元性。

<p class="project-resources"><a href="https://arxiv.org/abs/2512.11883">论文</a><a href="https://github.com/weathon/icml2026_position">代码与数据</a><a href="https://weathon.github.io/icml2026_position/">项目主页</a><a href="https://icml.cc/virtual/2026/poster/67242">ICML 页面</a></p>
</article>

<article class="selected-project">
<h4>BCATUS：出行行为追踪</h4>
<p class="project-details">2024 年 10 月 – 2026 年 4 月</p>

使用 Swift 开发 iOS 应用（[BCATUS](https://apps.apple.com/ca/app/bc-atus/id6467030994)），被动追踪大温哥华地区和奥肯那根地区 500 多名用户的出行行为，收集了 12,600 多次行程，为政府交通政策提供依据。在数据处理方面，设计了行程目的推断算法（相对提升 11.2%）、室内错误 GPS 聚类检测与清理算法，以及行程碎片合并算法，使 95% 以上的行程处理实现自动化，并减少了 50% 的处理时间。

<p class="project-resources"><a href="https://bcatus.ok.ubc.ca/">项目主页</a></p>
</article>

<article class="selected-project">
<h4>AI 辅助组卷平台</h4>
<p class="project-details">2025 年 5–8 月</p>
<p class="project-details">React · Node.js · PostgreSQL · Docker · BERT</p>

基于微服务架构开发面向高校的自动组卷平台。我利用 BERT 嵌入的余弦相似度设计了语义题目推荐服务，并通过带有时间版本的缓存减少了 85–95% 的重复计算。同时，基于依赖链和相似度聚类设计了保持逻辑关系的选项打乱算法；针对 2–5 个选项的题目，将 O(n!) 的排列生成替换为 O(1) 的预计算查表，在最常见的情况下实现了最高 24 倍加速。

</article>
</section>
