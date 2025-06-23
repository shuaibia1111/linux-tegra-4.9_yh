<hr>
<p>title: Linux Audio
subtitle: 开发指南
author: Allwinner
changelog:</p>
<ul>
<li>ver: 1.0
date: 2022.05.22
author: AWA1692
desc: |
  初始版本</li>
<li>ver: 1.1
date: 2023.11.27
author: AWA1458
desc: |
  调整优化文档结构</li>
<li>ver: 1.2
date: 2024.03.08
author: AWA1692
desc: |
  减少platform层接口重复介绍</li>
<li>ver: 1.3
date: 2024.05.4
author: AWA2136
desc: |
  修改menuconfig配置说明</li>
<li>ver: 1.4
date: 2024.07.23
author: AWA2077
desc: |
  新增sun300iw1相关配置</li>
<li>ver: 1.5
date: 2024.10.29
author: AWA2136
desc: |
  新增sun251iw1相关配置,补充HDMI EDP AV驱动说明</li>
<li>ver: 1.6
date: 2025.02.07
author: AWA1458
desc: |
  新增 sun60iw2相关配置</li>
<li>ver: 1.7
date: 2025.03.10
author: AWA2077
desc: |
  新增 sun50iw15相关配置</li>
<li>ver: 1.8
date: 2025.03.12
author: AWA2077
desc: |
  新增 sun65iw1相关配置</li>
<li>ver: 1.9
date: 2025.04.12
author: AWA2278
desc: |
  新增 sun8iw22相关配置</li>
<li>ver: 2.0
date: 2025.05.10
author: AWA2136
desc: |
  附录针对复杂配置项增加调试指南</li>
</ul>
<hr>
<h1 id="-">前言</h1>
<h2 id="-">文档简介</h2>
<p>本文档基于sunxi平台基础音频框架介绍，能够让使用者在sunxi平台开发使用音频驱动，内容分四大部分。</p>
<p>1、<strong>模块介绍</strong> 章节主要从 <strong>&quot;配置-&gt;KO加载-&gt;声卡控件-&gt;使用方法&quot;</strong> 等部分，介绍sunxi平台各音频接口的使用；</p>
<p>2、<strong>驱动架构介绍</strong> 章节主要从 <strong>&quot;软件框图-&gt;源码结构-&gt;关键数据结构-&gt;接口说明&quot;</strong> 等部分，介绍音频驱动源码的关键内容，方便二次开发；</p>
<p>3、 <strong>测试工具介绍</strong> 章节主要从 <strong>&quot;工具-&gt;asound.conf文件&quot;</strong> 等部分，介绍模块常用测试工具的使用和配置；</p>
<p>4、<strong>FAQ</strong> 章节主要提供了模块的调试方法和常见问题。</p>
<p>5、<strong>附录</strong> 章节主要介绍了各平台的AudioCodec声卡使用等。</p>
<h2 id="-">目标读者</h2>
<p>音频系统相关人员。</p>
<h2 id="-">适用平台</h2>
<p>Table: 适用平台列表</p>
<table>
<thead>
<tr>
<th>产品名称</th>
<th>内核版本</th>
<th>驱动文件</th>
</tr>
</thead>
<tbody>
<tr>
<td>T113</td>
<td>linux-5.4</td>
<td>sound/soc/sunxi_v2/*</td>
</tr>
<tr>
<td>T528</td>
<td>linux-5.4</td>
<td>sound/soc/sunxi_v2/*</td>
</tr>
<tr>
<td>V853</td>
<td>linux-4.9</td>
<td>sound/soc/sunxi_v2/*</td>
</tr>
<tr>
<td>T3</td>
<td>linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A40I</td>
<td>linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>MR153</td>
<td>llinux-5.15-origin</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T153</td>
<td>linux-5.10-rt</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T5</td>
<td>linux-5.4、linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T507</td>
<td>linux-5.4、linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H618</td>
<td>linux-5.4、linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H313</td>
<td>linux-5.4、linux-5.10</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A100</td>
<td>linux-5.10、linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A133</td>
<td>linux-5.10、linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T509</td>
<td>linux-5.10、linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>R818</td>
<td>linux-5.10、linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A523</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A527</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T527</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>MR527</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>AI985</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H728</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>TI533</td>
<td>linux-5.15-origin</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T536</td>
<td>linux-5.15-origin</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>MR536</td>
<td>linux-5.15-origin</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>MR533</td>
<td>linux-5.15-origin</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>V821</td>
<td>linux-5.4-andes</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>F135</td>
<td>linux-6.6-xuantie</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>F136</td>
<td>linux-6.6-xuantie</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H135</td>
<td>linux-6.6-xuantie</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H136</td>
<td>linux-6.6-xuantie</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H137</td>
<td>linux-6.6-xuantie</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>T736</td>
<td>linux-6.6</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A733</td>
<td>linux-6.6</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>TV323</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>H726</td>
<td>linux-5.15</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A537</td>
<td>linux-6.6</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
<tr>
<td>A333</td>
<td>linux-6.6</td>
<td>bsp/drivers/sound/platform/*</td>
</tr>
</tbody>
</table>
<h2 id="-">相关术语</h2>
<p>Table: 硬件术语</p>
<table>
<thead>
<tr>
<th>相关术语</th>
<th>解释说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>AudioCodec</td>
<td>芯片内置音频接口。</td>
</tr>
<tr>
<td>I2S/PCM</td>
<td>外置数字音频接口，常用于外接codec模块。</td>
</tr>
<tr>
<td>DAM</td>
<td>数字音频混音器。</td>
</tr>
<tr>
<td>OWA</td>
<td>外置数组音频接口，常用于同轴电缆或光纤输出。</td>
</tr>
<tr>
<td>DMIC</td>
<td>外置数字MIC接口。</td>
</tr>
<tr>
<td>同源播放</td>
<td>不同音频模块同时播放同一份音频数据。</td>
</tr>
<tr>
<td>同步采样</td>
<td>不同音频模块同时录音（可消除线程调度时差影响）。</td>
</tr>
</tbody>
</table>
<p>Table: 软件术语</p>
<table>
<thead>
<tr>
<th>相关术语</th>
<th>解释说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>ALSA</td>
<td>Advanced Linux Sound Architecture。</td>
</tr>
<tr>
<td>ASoC</td>
<td>ALSA System on Chip。</td>
</tr>
<tr>
<td>DAPM</td>
<td>动态音频电源管理。</td>
</tr>
<tr>
<td>samplebit</td>
<td>样本精度，记录音频数据最基本的单位，常见的有16位。</td>
</tr>
<tr>
<td>channel</td>
<td>通道数，该参数为1表示单声道，2表示立体声，大于2表示多声道。</td>
</tr>
<tr>
<td>rate</td>
<td>采样率，每秒钟采样次数，该次数是针对帧而言。</td>
</tr>
<tr>
<td>frame</td>
<td>帧，记录了一个声音单元，其长度为样本精度与通道数的乘积。</td>
</tr>
<tr>
<td>period size</td>
<td>每次硬件中断处理音频数据的帧数。</td>
</tr>
<tr>
<td>period count</td>
<td>处理完一个 buffer 数据所需的硬件中断次数。</td>
</tr>
<tr>
<td>buffer size</td>
<td>数据缓冲区大小 (period size * period count)</td>
</tr>
<tr>
<td>DRC</td>
<td>音频输出动态范围控制。</td>
</tr>
<tr>
<td>HPF</td>
<td>高通滤波。</td>
</tr>
<tr>
<td>XRUN</td>
<td>音频流异常状态，分为 underrun 和 overrun 两种状态。</td>
</tr>
<tr>
<td>交错模式</td>
<td>一种音频数据记录模式，数据以连续帧形式存放。</td>
</tr>
<tr>
<td>非交错模式</td>
<td>一种音频数据记录模式，数据是以连续通道形式存放。</td>
</tr>
<tr>
<td>tinyalsa</td>
<td>在 Linux 内核中与 ALSA 接口对接的库，可用于基本播录。</td>
</tr>
<tr>
<td>alsalib</td>
<td>在 Linux 内核中与 ALSA 接口对接的库，可用于播录。</td>
</tr>
</tbody>
</table>
<h1 id="-">模块介绍</h1>
<p>在 sunxi 中，从 Linux 软件上通常存在 5 类音频接口，如下。</p>
<ul>
<li>AudioCodec</li>
<li>I2S/PCM</li>
<li>I2S/PCM with DAM</li>
<li>DMIC</li>
<li>OWA</li>
</ul>
<p>音频接口驱动针对上述音频接口，会分别创建播放设备pcmXp和录音设备pcmXc（X：声卡序号）。</p>
<p>Table: AW SOC音频接口分布</p>
<table border="1" cellspacing="0" cellpadding="5">
<!-- T113 -->
<tr>
<th rowspan="7">T113</th>
<th>DAC x2, 8k-192kHz</th>
<th>x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x3, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x3</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>FMINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- V853 -->
<tr>
<th rowspan="5">V853</th>
<th>DAC x1, 8k-192kHz</th>
<th>x2</th>
<th>1-8ch</th>
<th>\</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>LINEOUTP/N</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- T3 -->
<tr>
<th rowspan="7">T3</th>
<th>DAC x2, 8k-192kHz</th>
<th>x3</th>
<th>\</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MIC x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>FMINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- T153 -->
<tr>
<th rowspan="2">T153</th>
<th>DAC x1, 8k-192kHz</th>
<th>x3</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>LINEOUTP/N x1</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>

<!-- T5 -->
<tr>
<th rowspan="4">T5</th>
<th>DAC x2, 8k-192kHz</th>
<th>I2S/PCM x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>LINEOUTL/R</th>
<th>DAM x2</th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th>APB x3</th>
<th></th>
<th></th>
</tr>
<tr>
<th>FMINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- T509 -->
<tr>
<th rowspan="5">T509</th>
<th>DAC x2, 8k-192kHz</th>
<th>x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTLP/N</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- A523 -->
<tr>
<th rowspan="5">A523</th>
<th>DAC x2, 8k-192kHz</th>
<th>x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x3, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R x1</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x3</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- T1533 -->
<tr>
<th rowspan="2">T1533</th>
<th>DAC x1, 8k-192kHz</th>
<th>x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>LINEOUTP/N x1</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>

<!-- V821 -->
<tr>
<th rowspan="4">V821</th>
<th>DAC x1, 8k-192kHz</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>ADC x1, 8k-48kHz</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTP/N x1</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x1</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- F135 -->
<tr>
<th rowspan="7">F135</th>
<th>DAC x2, 8k-192kHz</th>
<th>x3</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>LINEOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>FMINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- A733 -->
<tr>
<th rowspan="2">A733</th>
<th>\</th>
<th>x5</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th></th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>

<!-- TV323 -->
<tr>
<th rowspan="5">TV323</th>
<th>DAC x2, 8k-192kHz</th>
<th>x1</th>
<th></th>
<th>x2</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEINL/R</th>
<th></th>
<th></th>
<th></th>
</tr>

<!-- A537 -->
<tr>
<th rowspan="5">A537</th>
<th>DAC x2, 8k-192kHz</th>
<th>x4</th>
<th>1-8ch</th>
<th>x1</th>
</tr>
<tr>
<th>ADC x2, 8k-48kHz</th>
<th></th>
<th>8k-48kHz</th>
<th></th>
</tr>
<tr>
<th>HPOUTL/R x1</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>LINEOUTP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
<tr>
<th>MICP/N x2</th>
<th></th>
<th></th>
<th></th>
</tr>
</table>
<p>:::note</p>
<p>AW SOC通常会内置上述多种接口，接口具体特性请参考发布文档中 <strong>《XXX_User_Manual_Vx.x.pdf》</strong> 对应模块的描述。</p>
<p>:::</p>
<h2 id="audiocodec">AudioCodec</h2>
<h3 id="device-tree-">Device Tree配置</h3>
<h4 id="-">配置路径</h4>
<p>设备树中定义的是该类芯片对应于IC规格的所有配置，设备树文件路径如下：</p>
<ul>
<li>linux-4.9 ~ linux-5.4：</li>
</ul>
<blockquote>
<p>32位平台：kernel/{KERNEl_VER}/arch/arm/boot/dts/{CHIP}.dtsi</p>
<p>64位平台：kernel/{KERNEl_VER}/arch/arm64/boot/dts/sunxi/{CHIP}.dtsi</p>
</blockquote>
<ul>
<li>linux-5.10（含linux-5.10）之后：</li>
</ul>
<blockquote>
<p>32/64位平台：bsp/configs/{KERNEl_VER}/{CHIP}.dtsi</p>
</blockquote>
<p>:::note</p>
<ol>
<li>{KERNEl_VER}为内核版本，如linux-5.15;</li>
<li>{CHIP}.dtsi为具体芯片型号，如sun50iw10p1.dtsi。</li>
</ol>
<p>:::</p>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">codec:</span>codec@{module_base_reg} {
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-codec"</span>;
    reg             = <span class="hljs-params">&lt;<span class="hljs-number">0x0</span> {module_base_reg} <span class="hljs-number">0x0</span> <span class="hljs-number">0x32C</span>&gt;</span>;
    resets          = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> RST_BUS_AUDIO_CODEC&gt;</span>;
    clocks          = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_BUS_AUDIO_{module}&gt;</span>,
                      <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_PLL_xxx1&gt;</span>,    <span class="hljs-comment">/* 24.576M * n*/</span>
                      <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_PLL_xxx2&gt;</span>,    <span class="hljs-comment">/* 22.5792M * n */</span>
                      <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_AUDIO_{module}&gt;</span>,
    clock-names     = <span class="hljs-string">"clk_bus_audio_{module}"</span>,
                      <span class="hljs-string">"clk_pll_xxx1"</span>,
                      <span class="hljs-string">"clk_pll_xxx2"</span>,
                      <span class="hljs-string">"clk_audio_{module}"</span>；
    interrupts      = <span class="hljs-params">&lt;GIC_SPI {irq_num} IRQ_TYPE_LEVEL_HIGH&gt;</span>;    <span class="hljs-comment">/* jack irq */</span>
    status          = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-symbol">
codec_plat:</span><span class="hljs-class">codec_plat </span>{
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-plat-aaudio"</span>;
    dac-txdata      = <span class="hljs-params">&lt;{dac_txdata_reg}&gt;</span>;
    adc-rxdata      = <span class="hljs-params">&lt;{adc_rxdata_reg}&gt;</span>;
    dmas            = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;dma</span> {DRQ_PORT}&gt;</span>, <span class="hljs-params">&lt;<span class="hljs-variable">&amp;dma</span> {DRQ_PORT}&gt;</span>;
    dma-names       = <span class="hljs-string">"tx"</span>, <span class="hljs-string">"rx"</span>;
    playback-cma    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    capture-cma     = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    tx-fifo-size    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    rx-fifo-size    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    status          = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-symbol">
codec_mach:</span><span class="hljs-class">codec_mach </span>{
    compatible                      = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span>;
    soundcard-mach,name             = <span class="hljs-string">"audiocodec"</span>;
    soundcard-mach,pin-switches     = <span class="hljs-string">"MIC1"</span>, <span class="hljs-string">"MIC2"</span>,
                                      <span class="hljs-string">"LINEOUT"</span>, <span class="hljs-string">"HPOUT"</span>, <span class="hljs-string">"SPK"</span>;
    soundcard-mach,routing          = <span class="hljs-string">"MIC1_PIN"</span>, <span class="hljs-string">"MIC1"</span>,
                                      <span class="hljs-string">"MIC2_PIN"</span>, <span class="hljs-string">"MIC2"</span>,
                                      <span class="hljs-string">"LINEOUT"</span>, <span class="hljs-string">"LINEOUTL_PIN"</span>,
                                      <span class="hljs-string">"HPOUT"</span>, <span class="hljs-string">"HPOUTL_PIN"</span>,
                                      <span class="hljs-string">"HPOUT"</span>, <span class="hljs-string">"HPOUTR_PIN"</span>,
                                      <span class="hljs-string">"SPK"</span>, <span class="hljs-string">"HPOUTL_PIN"</span>,
                                      <span class="hljs-string">"SPK"</span>, <span class="hljs-string">"HPOUTR_PIN"</span>,
                                      <span class="hljs-string">"SPK"</span>, <span class="hljs-string">"LINEOUTL_PIN"</span>;
    soundcard-mach,jack-support     = <span class="hljs-params">&lt;<span class="hljs-number">1</span>&gt;</span>;
    status                          = <span class="hljs-string">"disabled"</span>;
    soundcard-mach,<span class="hljs-class">cpu </span>{
        sound-dai     = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;codec_plat</span>&gt;</span>;
    };
    soundcard-mach,<span class="hljs-class">codec </span>{
        sound-dai     = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;codec</span>&gt;</span>;
        soundcard-mach,pll-fs   = <span class="hljs-params">&lt;{n}&gt;</span>;
    };
};
</code></pre>
<p>::: note</p>
<p>本部分仅为示例，具体内容根据实际情况填写。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>AudioCodec 模块由3个设备树节点构建。</p>
<p>1、ASoC层codec: codec</p>
<p>Table: AudioCodec codec 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>reg</td>
<td>设置audiocodec寄存器起始地址和地址长度。</td>
</tr>
<tr>
<td>resets</td>
<td>设置audiocodec所需的复位时钟。</td>
</tr>
<tr>
<td>clocks</td>
<td>设置audiocodec所需的时钟源和模块时钟。</td>
</tr>
<tr>
<td>clock-names</td>
<td>对clocks属性内容进行名称定义，用于辅助clocks属性获取。</td>
</tr>
<tr>
<td>interrupts</td>
<td>AudioCodec中断号</td>
</tr>
</tbody>
</table>
<p>2、ASoC层platform: codec_plat</p>
<p>Table: AudioCodec codec_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>playback-cma</td>
<td>设置播放流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>capture-cma</td>
<td>设置录音流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>tx-fifo-size</td>
<td>设置播放流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>rx-fifo-size</td>
<td>设置录音流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>dac-txdata</td>
<td>设置播放流DMA搬运地址(audiocodec模块tx_fifo寄存器地址)。</td>
</tr>
<tr>
<td>adc-rxdata</td>
<td>设置录音流DMA搬运地址(audiocodec模块rx_fifo寄存器地址)。</td>
</tr>
<tr>
<td>dmas</td>
<td>设置模块所绑定的dma通道号。</td>
</tr>
<tr>
<td>dma-names</td>
<td>对dmas属性内容进行名称定义，用于辅助dmas属性获取。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: codec_mach</p>
<p>Table: AudioCodec codec_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>pin-switches</td>
<td>用于定义模块接口开关，</td>
</tr>
<tr>
<td></td>
<td>需参考驱动代码dapm进行设定。</td>
</tr>
<tr>
<td>routing</td>
<td>用于定义模块接口开关所链接的dapm通路，</td>
</tr>
<tr>
<td></td>
<td>需参考驱动代码dapm进行设定。</td>
</tr>
<tr>
<td>jack-support</td>
<td>指定支持的耳机检测类型。</td>
</tr>
<tr>
<td></td>
<td>0：不支持耳机，1：内置codec耳机检测，</td>
</tr>
<tr>
<td></td>
<td>2：extcon耳机检测，3：gpio耳机检测。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）。</td>
</tr>
</tbody>
</table>
<h3 id="board-dts-">board.dts配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash">&amp;codec {
    <span class="hljs-comment">/* note: power settings */</span>
    rglt-max                = &lt;n&gt;<span class="hljs-comment">;          /* n: rglt cnt */</span>
    rglt(n)-mode            = <span class="hljs-string">"xxx"</span><span class="hljs-comment">;        /* PMU; AUDIO; */</span>
    rglt(n)-voltage         = &lt;n&gt;<span class="hljs-comment">;          /* n: vcc voltage */</span>
    rglt(n)-supply          = &lt;&amp;pmu_node&gt;<span class="hljs-comment">;  /* AUDIO mode unnecessary */</span>
    <span class="hljs-comment">/* note: volume settings */</span>
    dac-vol                 = &lt;<span class="hljs-number">63</span>&gt;<span class="hljs-comment">;</span>
    dac(n)-vol              = &lt;<span class="hljs-number">160</span>&gt;<span class="hljs-comment">;</span>
    adc(n)-vol              = &lt;<span class="hljs-number">160</span>&gt;<span class="hljs-comment">;</span>
    adc(n)-gain             = &lt;<span class="hljs-number">31</span>&gt;<span class="hljs-comment">;</span>
    lineout-gain            = &lt;<span class="hljs-number">31</span>&gt;<span class="hljs-comment">;</span>
    hpout-gain              = &lt;<span class="hljs-number">7</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: pa settings */</span>
    pa-pin-max              = &lt;<span class="hljs-number">2</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* 0: level contrl; 1: pulse contrl; 0xFF: user contrl. */</span>
    pa-cfg-mode-0           = &lt;<span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-0                = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    pa-pin-level-0          = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-msleep-0         = &lt;<span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-1                = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    pa-cfg-mode-1           = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-level-1          = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-msleep-1         = &lt;n&gt;<span class="hljs-comment">;</span>
    pa-pin-duty-1           = &lt;n&gt;<span class="hljs-comment">;</span>
    pa-pin-period-1         = &lt;n&gt;<span class="hljs-comment">;</span>
    pa-pin-polarity-1       = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    pa-pin-periodcnt-1      = &lt;n&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: jack param -&gt; gpio */</span>
    hp-det-gpio             = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: jack param -&gt; codec */</span>
    <span class="hljs-keyword">jack-det-level </span>         = &lt;<span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-det-threshold </span>     = &lt;<span class="hljs-number">8</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-det-debouce </span>       = &lt;<span class="hljs-number">250</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: jack param -&gt; extcon */</span>
    <span class="hljs-keyword">extcon </span>                 = &lt;&amp;usb_power_supply&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-swpin-max </span>         = &lt;<span class="hljs-number">3</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-swpin-0 </span>           = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-swpin-1 </span>           = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-swpin-2 </span>           = &lt;&amp;pio xxx&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-mode-off </span>          = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">0</span> <span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-mode-usb </span>          = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">1</span> <span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-mode-audio </span>        = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">1</span> <span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-mode-micn </span>         = &lt;<span class="hljs-number">1</span> <span class="hljs-number">0xf</span> <span class="hljs-number">0xf</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-mode-mici </span>         = &lt;<span class="hljs-number">0</span> <span class="hljs-number">0xf</span> <span class="hljs-number">0xf</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-det-level </span>         = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-det-threshold </span>     = &lt;<span class="hljs-number">8</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-det-debounce </span>      = &lt;<span class="hljs-number">250</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* jack-key-det-voltage = &lt;min max&gt; */</span>
    <span class="hljs-keyword">jack-key-det-voltage-hook </span>  = &lt;<span class="hljs-number">0</span> <span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-key-det-voltage-up </span>    = &lt;<span class="hljs-number">2</span> <span class="hljs-number">2</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-key-det-voltage-down </span>  = &lt;<span class="hljs-number">4</span> <span class="hljs-number">5</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-key-det-voltage-voice </span> = &lt;<span class="hljs-number">1</span> <span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: sdbp - headset detective based on headphone */</span>
    <span class="hljs-keyword">jack-sdbp-method </span>           = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">jack-sdbp-scan-single-time </span> = &lt;<span class="hljs-number">1000</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* note: other settings */</span>
    adc-delay-time          = &lt;<span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    tx-hub-en<span class="hljs-comment">;</span>
    rx-<span class="hljs-keyword">sync-en;
</span>    status                  = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;codec_plat {
    status = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;codec_mach {
    status = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p><strong>配置项说明</strong></p>
<p>::: note</p>
<ul>
<li>功放有三类：电平控制类、脉冲控制类、用户自定义实现控制类，根据原理图确认使用功放类型；</li>
<li><p>耳机有三类：3.5mm耳机(使用micdet引脚)、typec模拟耳机、3.5mm耳机-gpio</p>
<p>  —— 若硬件上有3.5mm耳机孔，且使用固定的micdec引脚，使用3.5mm耳机配置；</p>
<p>  —— 若硬件上有3.5mm耳机孔，且无micdet引脚，需使用额外空闲的gpio检测耳机插拔，使用3.5mm耳机-gpio配置；</p>
<p>  —— 若硬件上没有3.5mm耳机孔，使用typec模拟耳机配置。</p>
</li>
<li><p>增益或音量的配置值范围每个平台可能存在差异，需通过spec或驱动确认。</p>
</li>
</ul>
<p>:::</p>
<p>Table: AudioCodec 模块板级配置项-通用类</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>status</td>
<td>&quot;okay&quot;, &quot;disabled&quot;</td>
<td>使能或关闭该节点驱动。</td>
</tr>
<tr>
<td>dac-vol</td>
<td>0~63</td>
<td>dac 总数字音量，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：-73.08-&gt;0dB。</td>
</tr>
<tr>
<td>dac(n)-vol</td>
<td>0~255</td>
<td>dac(n) 数字音量，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围-119.25-&gt;71.25dB。</td>
</tr>
<tr>
<td>adc(n)-vol</td>
<td>0~255</td>
<td>adc(n) 数字音量，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：-119.25-&gt;71.25dB。</td>
</tr>
<tr>
<td>adc(n)-gain</td>
<td>0~31</td>
<td>adc(n) 模拟增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：0-&gt;36dB。</td>
</tr>
<tr>
<td>lineout-gain</td>
<td>0~31</td>
<td>lineout 输出增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：-43.5-&gt;0,0dB。</td>
</tr>
<tr>
<td>hpout-gain</td>
<td>0~7</td>
<td>hpout 输出增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：-42-&gt;0dB。</td>
</tr>
<tr>
<td>fminl-gain</td>
<td>0~7</td>
<td>fminl 模拟增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：0-&gt;6dB。</td>
</tr>
<tr>
<td>fminr-gain</td>
<td>0~7</td>
<td>fminr 模拟增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：0-&gt;6dB。</td>
</tr>
<tr>
<td>lineinl-gain</td>
<td>0~1</td>
<td>lineinl 模拟增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：0-&gt;6dB。</td>
</tr>
<tr>
<td>lineinr-gain</td>
<td>0~1</td>
<td>lineinr 模拟增益，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>音量范围：0-&gt;6dB。</td>
</tr>
<tr>
<td>adc-delay-time</td>
<td>0,5,10,20,30</td>
<td>设置adc录音延迟时长，单位ms。</td>
</tr>
<tr>
<td>tx-hub-en</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册txhub控件。</td>
</tr>
<tr>
<td>rx-sync-en</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册rxsync控件。</td>
</tr>
<tr>
<td>pa-cfg-mode-(n)</td>
<td>0, 1, 0xff</td>
<td>指定第(n)个功放的控制方式。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>0：电平控制。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>1：脉冲控制。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>0xff：脉冲控制。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-电源类</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>rglt-max</td>
<td>u32</td>
<td>模块需要电源的总数。</td>
</tr>
<tr>
<td>rglt(n)-mode</td>
<td>&quot;PMU&quot;, &quot;AUDIO&quot;</td>
<td>模块所需第n路电的供电模式。</td>
</tr>
<tr>
<td>rglt(n)-voltage</td>
<td>1800000， 3300000</td>
<td>模块所需第n路电的电压值，单位uV。</td>
</tr>
<tr>
<td>rglt(n)-supply</td>
<td>缺省， pmu节点</td>
<td>模块所需第n路电的供电来源。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-3.5mm耳机配置(使用micdet)</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>jack-det-level</td>
<td>0~1</td>
<td>耳机插入检测电平。</td>
</tr>
<tr>
<td>jack-det-threshold</td>
<td>u32</td>
<td>耳机mic检测阈值，默认8。</td>
</tr>
<tr>
<td>jack-det-debounce</td>
<td>u32</td>
<td>耳机检测防抖时间ms，默认值250。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-type-c模拟耳机配置</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>extcon</td>
<td>事件链提供的节点</td>
<td>通过耳机插拔事件链通知节点。</td>
</tr>
<tr>
<td>jack-swpin-max</td>
<td>u32</td>
<td>标定type-c耳机检测输出引脚数量。</td>
</tr>
<tr>
<td>jack-swpin-(n)</td>
<td>pio引脚</td>
<td>usb audio 转换控制使能引脚。</td>
</tr>
<tr>
<td>jack-mode-off</td>
<td>0,1,0xf</td>
<td>usb audio 转换关闭真值。</td>
</tr>
<tr>
<td>jack-mode-usb</td>
<td>0,1,0xf</td>
<td>usb 模式真值。</td>
</tr>
<tr>
<td>jack-mode-hp</td>
<td>0,1,0xf</td>
<td>audio 模式真值。</td>
</tr>
<tr>
<td>jack-mode-micn</td>
<td>0,1,0xf</td>
<td>正插模式真值。</td>
</tr>
<tr>
<td>jack-mode-mici</td>
<td>0,1,0xf</td>
<td>反插模式真值。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-3.5mm耳机配置-gpio</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>hp-det-gpio</td>
<td>pio引脚</td>
<td>耳机插入gpio检测引脚。</td>
</tr>
<tr>
<td>jack-det-level</td>
<td>0~1</td>
<td>耳机插入检测电平。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-耳机通用配置</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>jack-key-det-voltage-hook</td>
<td>u32</td>
<td>耳机 hook 按键检测电压范围，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：mic: 0, max: 0。</td>
</tr>
<tr>
<td>jack-key-det-voltage-up</td>
<td>u32</td>
<td>耳机 up 按键检测电压范围，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：mic: 2, max: 2。</td>
</tr>
<tr>
<td>jack-key-det-voltage-down</td>
<td>u32</td>
<td>耳机 down 按键检测电压范围，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：mic: 4, max: 5。</td>
</tr>
<tr>
<td>jack-key-det-voltage-voice</td>
<td>u32</td>
<td>耳机 voice 按键检测电压范围，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：mic: 1, max: 1。</td>
</tr>
<tr>
<td>jack-sdbp-method</td>
<td>0~2</td>
<td>三节耳机检测四节耳机的检测方式，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>0：关闭检测，1：中断检测，2：轮询检测。</td>
</tr>
<tr>
<td>jack-sdbp-scan-single-time</td>
<td>u32（&gt;1000）</td>
<td>轮询检测的时间间隔，单位ms。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-电平控制类功放</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>pa-pin-max</td>
<td>u32</td>
<td>标定外部功放芯片使能引脚数量。</td>
</tr>
<tr>
<td>pa-pin-(n)</td>
<td>pio引脚</td>
<td>指定第(n)个功放使能引脚。</td>
</tr>
<tr>
<td>pa-pin-level-(n)</td>
<td>0~1</td>
<td>指定功放芯片使能电平。</td>
</tr>
<tr>
<td>pa-pin-msleep-(n)</td>
<td>u32</td>
<td>设置功放芯片使能延迟时长，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>单位ms,正常小于200，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>常用于规避pop声。</td>
</tr>
<tr>
<td>pa-pin-msleep1-(n)</td>
<td>u32</td>
<td>设置功放芯片关闭后,</td>
</tr>
<tr>
<td></td>
<td></td>
<td>soc音频输出延迟时长，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>单位ms,正常小于200，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>常用于规避功放关闭时的pop声。</td>
</tr>
</tbody>
</table>
<p>Table: AudioCodec 模块板级配置项-脉冲控制类功放</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>pa-pin-duty-(n)</td>
<td>u32</td>
<td>脉冲的宽度, 单位us。</td>
</tr>
<tr>
<td>pa-pin-period-(n)</td>
<td>u32</td>
<td>脉冲的周期，单位us。</td>
</tr>
<tr>
<td>pa-pin-polarity-(n)</td>
<td>1</td>
<td>脉冲的极性。</td>
</tr>
<tr>
<td>pa-pin-periodcnt-(n)</td>
<td>u32</td>
<td>脉冲的个数。</td>
</tr>
</tbody>
</table>
<h3 id="audiocodec-kernel-menuconfig-">AudioCodec kernel menuconfig 配置说明</h3>
<p>linux-4.9 ~ linux-5.4，menuconfig必选配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner AAUDIO support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig必选配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner AAUDIO support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: AudioCodec kernel menuconfig可选配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner AAUDIO support</td>
<td>AudioCodec模块。</td>
</tr>
<tr>
<td>Allwinner function components</td>
<td>功能组件模块。</td>
</tr>
<tr>
<td>Components SFX</td>
<td>供cedarSE使用功能组件（硬件算法更改寄存器）。</td>
</tr>
<tr>
<td>Components Debug</td>
<td>调试节点功能组件（查看音频寄存器）。</td>
</tr>
<tr>
<td>Enable audio dynamic debug</td>
<td>使能AUDIO模块DYNAMIC DEBUG模式。</td>
</tr>
</tbody>
</table>
<p>::: note</p>
<ol>
<li>Components SFX与Components Debug依赖于Allwinner function components，可按照实际需求选择。</li>
</ol>
<p>:::</p>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分五大类驱动如下。</p>
<ul>
<li>特定功能组件（sfx）</li>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件 -&gt; 特殊功能组件 -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>AudioCodec 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># 特殊功能组件(具体根据实际打开的component加载)</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_component_sfx.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动 和 ASoC codec 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_aaudio.ko
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_internal_codec.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件介绍</h3>
<p>不同平台的AudioCodec的声卡控件不完全相同，声卡控件及常用使用方法见<a href="#AudioCodec声卡使用">AudioCodec声卡使用</a>。AudioCodec的声卡控件分为三类，分别是特殊功能类控件，音量调节类控件，通路开关类控件。</p>
<p>Table: 特殊功能类控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>tx hub mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>rx sync mode</td>
<td>同步录音开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>ADC DRC{n} Mode</td>
<td>ADC DRC{n} 开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>ADC HPF{n} Mode</td>
<td>ADC HPF{n} 开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>DAC DRC Mode</td>
<td>DAC DRC 开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>DAC HPF Mode</td>
<td>DAC HPF 开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>ADDA Loop Mode</td>
<td>ADC DAC 回路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>DAC{n} DAC{m} Swap</td>
<td>DAC{n}&amp;{m} 通道交换开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>ADC{n} ADC{m} swap</td>
<td>ADC{n}&amp;{m} 通道交换开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<p>Table: 音量调节类控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>DAC Volume</td>
<td>dac 数字音量调节</td>
<td>0-&gt;63 (-73.08-&gt;0dB)</td>
</tr>
<tr>
<td>DAC{n} Volume</td>
<td>dac{n} 数字音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>ADC{n} Volume</td>
<td>adc1 数字音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>ADC{n} Gain</td>
<td>adc{n} 模拟增益调节</td>
<td>0~31(0-&gt;36dB)</td>
</tr>
<tr>
<td>LINEOUT{n} Gain</td>
<td>lineout{n} 输出增益调节</td>
<td>0~31(-43.5-&gt;0,0dB)</td>
</tr>
<tr>
<td>HPOUT Gain</td>
<td>hpout 输出增益调节</td>
<td>0~7(-42-&gt;0dB)</td>
</tr>
</tbody>
</table>
<p>Table: 通路开关类控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>MIC{n} Switch</td>
<td>MIC{n} 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>LINEOUT{n} Switch</td>
<td>LINEOUT{n} 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>HPOUT Switch</td>
<td>HPOUT 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>SPK Switch</td>
<td>SPK 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>FMIN{n} Switch</td>
<td>FMIN{n} 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>LINEIN{n} Switch</td>
<td>LINEIN{n} 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>SPK Switch</td>
<td>SPK 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>Output{n} Mixer DAC{n} Switch</td>
<td>DAC{n}─&gt;LINEOUT{n} 通路开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>LINEOUT{n} Output Select</td>
<td>LINEOUT{n} 输出模式</td>
<td>Single;Differ</td>
</tr>
<tr>
<td>MIC{n} Input Select</td>
<td>MIC{n} 输入模式</td>
<td>Single;Differ</td>
</tr>
<tr>
<td>Input{n} Mux</td>
<td>Input{n} 输入源</td>
<td>MIC{n};FMINL;LINEINL</td>
</tr>
<tr>
<td>X Output/Input Mixer Y Switch</td>
<td>Y-&gt;X 通路开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<h2 id="i2s-pcm">I2S/PCM</h2>
<h3 id="device-tree-">Device Tree 配置</h3>
<h4 id="-">配置路径</h4>
<p>设备树中定义的是该类芯片对应于IC规格的所有配置，设备树文件路径如下：</p>
<ul>
<li>linux-4.9、linux-5.4：</li>
</ul>
<blockquote>
<p>32位平台：kernel/{KERNEl_VER}/arch/arm/boot/dts/{CHIP}.dtsi</p>
<p>64位平台：kernel/{KERNEl_VER}/arch/arm64/boot/dts/sunxi/{CHIP}.dtsi</p>
</blockquote>
<ul>
<li>linux-5.10（含linux-5.10）之后：</li>
</ul>
<blockquote>
<p>32/64位平台：bsp/configs/{KERNEl_VER}/{CHIP}.dtsi</p>
</blockquote>
<p>:::note</p>
<ol>
<li>{KERNEl_VER}为内核版本，如linux-5.15；</li>
<li>{CHIP}.dtsi为具体芯片型号，如sun55iw3p1.dtsi。</li>
</ol>
<p>:::</p>
<h4 id="-">配置示例</h4>
<ul>
<li>第n组I2S配置如下。</li>
</ul>
<pre><code class="lang-bash">i2s{n}_plat:i2s{n}_plat@{module_base_reg} {
    <span class="hljs-comment">#sound-dai-cells = &lt;0&gt;;</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-plat-i2s"</span><span class="hljs-comment">;</span>
    reg             = &lt;<span class="hljs-number">0x0</span> {module_base_reg} <span class="hljs-number">0x0</span> <span class="hljs-number">0xA0</span>&gt;<span class="hljs-comment">;</span>
    resets          = &lt;&amp;ccu RST_BUS_{module}&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clocks </span>         = &lt;&amp;ccu CLK_PLL_xxx1&gt;,
                      &lt;&amp;ccu CLK_PLL_xxx2&gt;,
                      &lt;&amp;ccu CLK_xx1&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clock-names </span>    = <span class="hljs-string">"clk_pll_xxx1"</span>,
                      <span class="hljs-string">"clk_pll_xxx2"</span>,
                      <span class="hljs-string">"clk_{module}"</span><span class="hljs-comment">;</span>
    dmas            = &lt;&amp;dma1 {DRQ_PORT}&gt;, &lt;&amp;dma1 {DRQ_PORT}&gt;<span class="hljs-comment">;</span>
    dma-names       = <span class="hljs-string">"tx"</span>, <span class="hljs-string">"rx"</span><span class="hljs-comment">;</span>
    playback-cma    = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    capture-cma     = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    tx-fifo-size    = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    rx-fifo-size    = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    status          = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

i2s{n}_mach:i2s{n}_mach{
    compatible                  = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span><span class="hljs-comment">;</span>
    soundcard-mach,name         = <span class="hljs-string">"sndi2s{n}"</span><span class="hljs-comment">;</span>
    soundcard-mach,format       = <span class="hljs-string">"i2s"</span><span class="hljs-comment">;</span>
    soundcard-mach,slot-num     = &lt;<span class="hljs-number">2</span>&gt;<span class="hljs-comment">;</span>
    soundcard-mach,slot-width   = &lt;<span class="hljs-number">32</span>&gt;<span class="hljs-comment">;</span>
    status                      = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai = &lt;&amp;i2s{n}_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>本部分仅为示例，具体内容根据实际情况填写。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>I2S/PCM 模块由2个或3个设备树节点构建。</p>
<p>1、ASoC层codec: 非必须节点，若无，则绑定虚拟codec节点。</p>
<p>2、ASoC层platform: i2s(n)_plat</p>
<p>Table: I2S/PCM i2s(n)_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>reg</td>
<td>设置I2S/PCM寄存器起始地址和地址长度。</td>
</tr>
<tr>
<td>clocks</td>
<td>设置I2S/PCM所需的时钟源和模块时钟。</td>
</tr>
<tr>
<td>clock-names</td>
<td>对clocks属性内容进行名称定义，用于辅助clocks属性获取。</td>
</tr>
<tr>
<td>dmas</td>
<td>设置模块所绑定的dma通道号。</td>
</tr>
<tr>
<td>dma-names</td>
<td>对dmas属性内容进行名称定义，用于辅助dmas属性获取。</td>
</tr>
<tr>
<td>playback-cma</td>
<td>设置播放流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>capture-cma</td>
<td>设置录音流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>tx-fifo-size</td>
<td>设置播放流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>rx-fifo-size</td>
<td>设置录音流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: i2s(n)_mach</p>
<p>Table: I2S/PCM i2s(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层）。</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点，</td>
</tr>
<tr>
<td></td>
<td>若该子节点下无sound-dai属性，即代表使用虚拟codec，用于辅助生成声卡。</td>
</tr>
</tbody>
</table>
<h3 id="board-dts-">board.dts 配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash">&amp;i2s{n}_plat {
    tdm-num            = &lt;{n}&gt;<span class="hljs-comment">;</span>
    tx-pin             = &lt;{m} ...&gt;<span class="hljs-comment">;</span>
    tx-pin{m}-chmap    = &lt;{num1} {num2} ...&gt;<span class="hljs-comment">;</span>
    rxfifo-pinmap      = &lt;{m} ...&gt;<span class="hljs-comment">;</span>
    rxfifo-chmap       = &lt;{num1} {num2} ...&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* pinctrl-used; */</span>
    <span class="hljs-comment">/* pinctrl-names   = "default","sleep"; */</span>
    <span class="hljs-comment">/* pinctrl-0       = &lt;&amp;i2s{n}_pins_a&gt;; */</span>
    <span class="hljs-comment">/* pinctrl-1       = &lt;&amp;i2s{n}_pins_b&gt;; */</span>
    tx-hub-en<span class="hljs-comment">;</span>
    rx-<span class="hljs-keyword">sync-en;
</span>    status             = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;i2s{n}_mach {
    soundcard-mach,format           = <span class="hljs-string">"i2s"</span><span class="hljs-comment">;</span>
    soundcard-mach,frame-master     = &lt;&amp;i2s{n}_cpu&gt;<span class="hljs-comment">;</span>
    soundcard-mach,<span class="hljs-keyword">bitclock-master </span> = &lt;&amp;i2s{n}_cpu&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* soundcard-mach,frame-inversion; */</span>
    <span class="hljs-comment">/* soundcard-mach,bitclock-inversion; */</span>
    soundcard-mach,slot-num         = &lt;<span class="hljs-number">2</span>&gt;<span class="hljs-comment">;</span>
    soundcard-mach,slot-width       = &lt;<span class="hljs-number">32</span>&gt;<span class="hljs-comment">;</span>
    soundcard-mach,pin-<span class="hljs-keyword">switches </span>    = <span class="hljs-string">"SPK"</span><span class="hljs-comment">;</span>
    soundcard-mach,routing          = <span class="hljs-string">"SPK"</span>, <span class="hljs-string">"I2S_PIN"</span><span class="hljs-comment">;</span>
    soundcard-mach,capture-only<span class="hljs-comment">;</span>
    status        = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
    i2s{n}_cpu: soundcard-mach,cpu {
        sound-dai = &lt;&amp;i2s{n}_plat&gt;<span class="hljs-comment">;</span>
        <span class="hljs-comment">/* note: pll freq = 24.576M or 22.5792M * pll-fs */</span>
        soundcard-mach,pll-fs    = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
        <span class="hljs-comment">/* note:
         * "mclk-fs"
         * if not defined it or equal to 0, disable mclk.
         *
         * "mclk-fp" (if defined "mclk-fs")
         * 1. if not defined "mclk-fp", mclk_freq = mclk-fs * sample_rate;
         * 2. if defined "mclk-fp" but no value, mclk_freq = mclk-fs * 11.2896M or 12.288M;
         * 3. if defined "mclk-fp" with 2 value, like: mclk-fp = &lt;val1 val2&gt;;
         *    it means: mclk_freq(44.1k fp) = mclk-fs * val1;
         *              mclk_freq(48k fp)   = mclk-fs * val2.
         */</span>
        soundcard-mach,mclk-fs    = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
        soundcard-mach,mclk-<span class="hljs-built_in">fp</span>    = &lt;<span class="hljs-number">11289600</span> <span class="hljs-number">12288000</span>&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    i2s{n}_codec: soundcard-mach,codec {
        sound-dai               = &lt;&amp;ac107&gt;<span class="hljs-comment">;</span>
        soundcard-mach,pll-fs   = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>gpio部分请参考附件<a href="#GPIO功能复用配置">GPIO功能复用配置</a></p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: I2S/PCM 模块板级配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>status</td>
<td>&quot;okay&quot;, &quot;disabled&quot;</td>
<td>使能或关闭该节点驱动。</td>
</tr>
<tr>
<td>tdm-num</td>
<td>0~1</td>
<td>指定I2S序号，需和i2s(n)_plat的(n)对应。</td>
</tr>
<tr>
<td>tx-pin</td>
<td>0~3</td>
<td>指定I2S所使用的DOUT引脚序号。</td>
</tr>
<tr>
<td>tx-pin{m}-chmap</td>
<td>0~15</td>
<td>指定第m个DOUT脚txfifo通道序号到tx slot序号的映射</td>
</tr>
<tr>
<td>rxfifo-pinmap</td>
<td>0~3</td>
<td>指定每个slot数据来源于哪路DIN引脚。</td>
</tr>
<tr>
<td>rxfifo-chmap</td>
<td>0~15</td>
<td>指定rx slot序号到rxfifo通道序号的映射。</td>
</tr>
<tr>
<td>tx-hub-en</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册txhub控件。</td>
</tr>
<tr>
<td>rx-sync-en</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册rxsync控件。</td>
</tr>
<tr>
<td>capture-only</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册录音的设备节点。</td>
</tr>
<tr>
<td>pin-switches</td>
<td>&quot;SPK&quot;</td>
<td>用于控制外挂codec/PA。</td>
</tr>
<tr>
<td>routing</td>
<td>&quot;SPK&quot;, &quot;I2S_PIN&quot;</td>
<td>SPK开关所链接的dapm通路。</td>
</tr>
<tr>
<td>format</td>
<td>&quot;i2s&quot;,&quot;right_j&quot;,&quot;left_j&quot;,</td>
<td>选择tdm协议格式。</td>
</tr>
<tr>
<td></td>
<td>&quot;dsp_a&quot;,&quot;dsp_b&quot;</td>
<td></td>
</tr>
<tr>
<td>frame-master</td>
<td>cpu子节点，codec子节点</td>
<td>选择LRCK信号主模式。</td>
</tr>
<tr>
<td>bitclock-master</td>
<td>cpu子节点，codec子节点</td>
<td>选择BCLK信号主模式。</td>
</tr>
<tr>
<td>frame-inversion</td>
<td>注释为false, 反之为ture</td>
<td>LRCK信号是否翻转。</td>
</tr>
<tr>
<td>bitclock-inversion</td>
<td>注释为false, 反之为ture</td>
<td>BCLK信号是否翻转。</td>
</tr>
<tr>
<td>slot-num</td>
<td>1~16</td>
<td>slot数量，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>可简单理解为支持最大通道数。</td>
</tr>
<tr>
<td>slot-width</td>
<td>8, 16, 24, 32</td>
<td>单个slot宽度，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>可简单理解为支持最大数据精度。</td>
</tr>
<tr>
<td>mclk-fp</td>
<td>注释为false, 反之为ture</td>
<td>ture: 指定mclk-fp[0]与mclk-fp[1]</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：11289600 12288000</td>
</tr>
<tr>
<td></td>
<td></td>
<td>false: mclk以采样率倍数输出。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>默认值：0 0</td>
</tr>
<tr>
<td>mclk-fs</td>
<td>u32</td>
<td>固定频段：mclk =</td>
</tr>
<tr>
<td></td>
<td></td>
<td>mclk-fs * mclk-fp[0] or</td>
</tr>
<tr>
<td></td>
<td></td>
<td>mclk-fs * mclk-fp[1]。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>采样率倍数：mclk =</td>
</tr>
<tr>
<td></td>
<td></td>
<td>mclk-fs * pcm rate。</td>
</tr>
</tbody>
</table>
<h3 id="i2s-kernel-menuconfig-">I2S kernel menuconfig 配置说明</h3>
<p>linux-5.10以下内核版本，menuconfig必选配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner I2S Support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig必选配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner I2S Support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: I2S/PCM menuconfig可选配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner I2S support</td>
<td>I2S/PCM模块。</td>
</tr>
<tr>
<td>Allwinner HDMIAUDIO Support</td>
<td>HDMI AUDIO 模块。</td>
</tr>
<tr>
<td>Allwinner EDPAUDIO Support</td>
<td>EDP AUDIO 模块。</td>
</tr>
<tr>
<td>Allwinner AVAUDIO Support</td>
<td>AV AUDIO 模块(new edp driver,应用于sun60iw2以后的新平台)。</td>
</tr>
<tr>
<td>Allwinner function components</td>
<td>功能组件模块。</td>
</tr>
<tr>
<td>Components Debug</td>
<td>调试节点功能组件（查看音频寄存器与i2s格式调试）。</td>
</tr>
<tr>
<td>Enable audio dynamic debug</td>
<td>使能AUDIO模块DYNAMIC DEBUG模式。</td>
</tr>
</tbody>
</table>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分五大类驱动如下。</p>
<ul>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件  -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>I2S/PCM 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_i2s.ko

<span class="hljs-comment"># HDMI Audio驱动（若使用HDMI Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_hdmi.ko
<span class="hljs-comment"># EDP Audio驱动（若使用EDP Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_edp.ko
<span class="hljs-comment"># AV Audio驱动（若使用AV Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_av.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件介绍</h3>
<h4 id="i2s-">I2S 控件</h4>
<pre><code class="lang-bash">Mixer name: <span class="hljs-comment">'sndi2s0'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">3</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx sync mode                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">2</span>       BOOL    <span class="hljs-number">1</span>       loopback debug                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">3</span>       BOOL    <span class="hljs-number">1</span>       SPK Switch                               <span class="hljs-keyword">Off</span>
</code></pre>
<p>::: note</p>
<p>SPK Switch通过设备树使能，默认无此控件。</p>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>tx hub mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>rx sync mode</td>
<td>同步录音开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>SPK Swich</td>
<td>SPK通路开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<h4 id="hdmi-audio-">HDMI Audio 控件</h4>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'sndhdmi'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">3</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                        <span class="hljs-keyword">value</span>
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       audio <span class="hljs-keyword">data</span> <span class="hljs-keyword">format</span>           NULL &gt;PCM AC3 MPEG1 MP3 MPEG2 AAC DTS ATRAC ONE_BIT_AUDIO
                                                    DOLBY_DIGITAL_PLUS DTS_HD MAT DST WMAPRO
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                 &gt;Off On
<span class="hljs-number">2</span>       BOOL    <span class="hljs-number">1</span>       loopback debug              Off
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>audio data format</td>
<td>设置音频数据格式</td>
<td>NULL PCM AC3 MPEG1 MP3 MPEG2 AAC DTS</td>
</tr>
<tr>
<td></td>
<td></td>
<td>ATRAC ONE_BIT_AUDIO DOLBY_DIGITAL_PLUS</td>
</tr>
<tr>
<td></td>
<td></td>
<td>DTS_HD MAT DST WMAPRO</td>
</tr>
<tr>
<td>tx hub mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<h3 id="-">使用方法</h3>
<blockquote>
<p>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节；</p>
</blockquote>
<h4 id="i2s-">I2S常见使用方法</h4>
<pre><code class="lang-bash"><span class="hljs-comment"># 确认声卡序号</span>
cat /proc/asound/cards
3 [sndi2s0     ]: sndi2s0 - sndi2s0
                  sndi2s0

<span class="hljs-comment"># sndi2s0 声卡录音，2channel 16bit 48000hz</span>
tinycap test.wav -D<span class="hljs-number"> 3 </span>-c<span class="hljs-number"> 2 </span>-b<span class="hljs-number"> 16 </span>-r<span class="hljs-number"> 48000 </span>-T 10

<span class="hljs-comment"># sndi2s0 声卡播放</span>
tinyplay test.wav -D 3

<span class="hljs-comment"># sndi2s0 声卡内部回环，播录音频格式需保持一致</span>
tinymix -D<span class="hljs-number"> 3 </span>"loopback debug" 1
tinyplay test_play.wav -D<span class="hljs-number"> 3 </span>&amp;
tinycap test_cap.wav -D<span class="hljs-number"> 3 </span>-c<span class="hljs-number"> 2 </span>-b<span class="hljs-number"> 16 </span>-r<span class="hljs-number"> 48000 </span>-T 10
</code></pre>
<h4 id="hdmi-audio-">hdmi audio 常见使用方法</h4>
<pre><code class="lang-bash"><span class="hljs-comment"># 确认声卡序号</span>
cat /<span class="hljs-keyword">proc</span>/asound/cards
4 [sndhdmi     ]:<span class="hljs-title"> sndhdmi</span> -<span class="hljs-title"> sndhdmi</span>
<span class="hljs-title">                  sndhdmi</span>

#<span class="hljs-title"> sndhdmi</span> 声卡播放<span class="hljs-title">
tinyplay</span> test.wav -D 4

#<span class="hljs-title"> sndhdmi</span> 数据透传<span class="hljs-title">
tinymix</span> -D 4 "audio<span class="hljs-title"> data</span> format"<span class="hljs-title"> DTS</span>
tinyplay<span class="hljs-title"> test.dts</span> -D 4
</code></pre>
<h2 id="i2s-pcm-with-dam">I2S/PCM with DAM</h2>
<p>I2S/PCM with DAM 音频集线器接口简称为AHUB接口，该接口内部集成I2S接口及DAM混音器等，可实现多路输入输出及硬件混音功能。</p>
<h3 id="device-tree-">Device Tree 配置</h3>
<h4 id="-">配置路径</h4>
<p>设备树中定义的是该类芯片对应于IC规格的所有配置，设备树文件路径如下：</p>
<ul>
<li>linux-4.9、linux-5.4：</li>
</ul>
<blockquote>
<p>32位平台：kernel/{KERNEl_VER}/arch/arm/boot/dts/{CHIP}.dtsi</p>
<p>64位平台：kernel/{KERNEl_VER}/arch/arm64/boot/dts/sunxi/{CHIP}.dtsi</p>
</blockquote>
<ul>
<li>linux-5.10（含linux-5.10）之后：</li>
</ul>
<blockquote>
<p>32/64位平台：bsp/configs/{KERNEl_VER}/{CHIP}.dtsi</p>
</blockquote>
<p>:::note</p>
<ol>
<li>{KERNEl_VER}为内核版本，如linux-5.15；</li>
<li>{CHIP}.dtsi为具体芯片型号，如sun55iw3p1.dtsi。</li>
</ol>
<p>:::</p>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">ahub_dam_plat:</span>ahub_dam_plat@{module_base_reg} {
    <span class="hljs-comment">#sound-dai-cells = &lt;0&gt;;</span>
    <span class="hljs-comment">/* sound card without pcm for hardware mix setting */</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-plat-ahub_dam"</span><span class="hljs-comment">;</span>
    reg             = &lt;<span class="hljs-number">0x0</span> {module_base_reg} <span class="hljs-number">0x0</span> <span class="hljs-number">0xAEC</span>&gt;<span class="hljs-comment">;</span>
    resets          = &lt;&amp;ccu RST_BUS_AUDIO_{module}&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clocks </span>         = &lt;&amp;ccu CLK_BUS_AUDIO_{module}&gt;,
                      &lt;&amp;ccu CLK_PLL_AUDIO(n)&gt;,
                      &lt;&amp;ccu CLK_AUDIO_{module}&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clock-names </span>    = <span class="hljs-string">"clk_bus_audio_{module}"</span>,
                      <span class="hljs-string">"clk_pll_audio(n)"</span>,
                      <span class="hljs-string">"clk_audio_{module}"</span><span class="hljs-comment">;</span>
    status = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
<span class="hljs-symbol">
ahub_dam_mach:</span>ahub_dam_mach {
    compatible             = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span><span class="hljs-comment">;</span>
    soundcard-mach,name    = <span class="hljs-string">"ahubdam"</span><span class="hljs-comment">;</span>
    status                 = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai = &lt;&amp;ahub_dam_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

ahub{n}_plat:ahub{<span class="hljs-number">0</span>}_plat {
    <span class="hljs-comment">#sound-dai-cells = &lt;0&gt;;</span>
    compatible             = <span class="hljs-string">"allwinner,sunxi-snd-plat-ahub"</span><span class="hljs-comment">;</span>
    apb-num                = &lt;{n}&gt;<span class="hljs-comment">;</span>
    dmas                   = &lt;&amp;dma {DRQ_PORT}&gt;, &lt;&amp;dma {DRQ_PORT}&gt;<span class="hljs-comment">;</span>
    dma-names              = <span class="hljs-string">"tx"</span>, <span class="hljs-string">"rx"</span><span class="hljs-comment">;</span>
    playback-cma           = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    capture-cma            = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    tx-fifo-size           = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    rx-fifo-size           = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    status                 = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

ahub{n}_mach:ahub{n}_mach {
    compatible          = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span><span class="hljs-comment">;</span>
    soundcard-mach,name = <span class="hljs-string">"ahubi2s{n}"</span><span class="hljs-comment">;</span>
    status              = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai = &lt;&amp;ahub{n}_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>本部分仅为示例，具体内容根据实际情况填写。</p>
<p>:::</p>
<p><strong>配置项说明：</strong></p>
<p>AHUB DAM 由2个设备树节点构建，AHUB 模块由2个或3个设备树节点构建。</p>
<p>1、ASoC层codec: 非必须节点，若无，则绑定虚拟codec节点。</p>
<p>2、ASoC层platform: ahub_dam_plat 和 ahub(n)_plat</p>
<p>Table: AHUB DAM ahub_dam_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>reg</td>
<td>设置AHUB寄存器起始地址和地址长度。</td>
</tr>
<tr>
<td>resets</td>
<td>设置AHUB所需的复位时钟。</td>
</tr>
<tr>
<td>clocks</td>
<td>设置AHUB所需的时钟源和模块时钟。</td>
</tr>
<tr>
<td>clock-names</td>
<td>对clocks属性内容进行名称定义，用于辅助clocks属性获取。</td>
</tr>
</tbody>
</table>
<p>Table: AHUB ahub(n)_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>apb-num</td>
<td>设置模块所绑定的apb通道号（对应一组DMA）。</td>
</tr>
<tr>
<td>dmas</td>
<td>设置模块所绑定的dma通道号。</td>
</tr>
<tr>
<td>dma-names</td>
<td>对dmas属性内容进行名称定义，用于辅助dmas属性获取。</td>
</tr>
<tr>
<td>playback-cma</td>
<td>设置播放流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>capture-cma</td>
<td>设置录音流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>tx-fifo-size</td>
<td>设置播放流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>rx-fifo-size</td>
<td>设置录音流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: daudio(n)_mach</p>
<p>Table: 混音部分 ahub_dam_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点若该子节点下无sound-dai属性，</td>
</tr>
<tr>
<td></td>
<td>即代表使用虚拟codec，用于辅助生成声卡。</td>
</tr>
</tbody>
</table>
<p>Table: I2S/PCM ahub(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点若该子节点下无sound-dai属性，</td>
</tr>
<tr>
<td></td>
<td>即代表使用虚拟codec，用于辅助生成声卡。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）。</td>
</tr>
</tbody>
</table>
<h3 id="board-dts-">board.dts 配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash">&amp;ahub_dam_plat {
    status = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;ahub_dam_mach {
    status = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;ahub{n}_plat {
    tdm-num         = &lt;{n}&gt;<span class="hljs-comment">;</span>
    tx-pin          = &lt;{m}&gt;<span class="hljs-comment">;</span>
    rx-pin          = &lt;{m}&gt;<span class="hljs-comment">;</span>
    pinctrl-used<span class="hljs-comment">;</span>
    pinctrl-names   = <span class="hljs-string">"default"</span>,<span class="hljs-string">"sleep"</span><span class="hljs-comment">;</span>
    pinctrl-0       = &lt;&amp;ahub_i2s{n}_pins_a&gt;<span class="hljs-comment">;</span>
    pinctrl-1       = &lt;&amp;ahub_i2s{n}_pins_b&gt;<span class="hljs-comment">;</span>
    status          = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;ahub{n}_mach {
    soundcard-mach,format        = <span class="hljs-string">"i2s"</span><span class="hljs-comment">;</span>
    soundcard-mach,frame-master    = &lt;&amp;ahub{n}_cpu&gt;<span class="hljs-comment">;</span>
    soundcard-mach,<span class="hljs-keyword">bitclock-master </span>   = &lt;&amp;ahub{n}_cpu&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* soundcard-mach,frame-inversion; */</span>
    <span class="hljs-comment">/* soundcard-mach,bitclock-inversion; */</span>
    soundcard-mach,slot-num        = &lt;<span class="hljs-number">2</span>&gt;<span class="hljs-comment">;</span>
    soundcard-mach,slot-width    = &lt;<span class="hljs-number">32</span>&gt;<span class="hljs-comment">;</span>
    soundcard-mach,capture-only<span class="hljs-comment">;</span>
    status = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
    ahub{n}_cpu: soundcard-mach,cpu {
        sound-dai               = &lt;&amp;ahub{n}_plat&gt;<span class="hljs-comment">;</span>
        soundcard-mach,pll-fs    = &lt;<span class="hljs-number">4</span>&gt;<span class="hljs-comment">;</span>
        soundcard-mach,mclk-fs    = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
        soundcard-mach,mclk-<span class="hljs-built_in">fp</span><span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
<span class="hljs-symbol">    ahub0_codec:</span> soundcard-mach,codec {
        sound-dai               = &lt;&amp;ac107&gt;<span class="hljs-comment">;</span>
        soundcard-mach,pll-fs    = &lt;<span class="hljs-number">1</span>&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>gpio部分请参考附件<a href="#GPIO功能复用配置">GPIO功能复用配置</a></p>
<p>:::</p>
<p><strong>配置项说明：</strong></p>
<p>Table: 带混音I2S/PCM 模块板级配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>status</td>
<td>&quot;okay&quot;, &quot;disabled&quot;</td>
<td>使能或关闭该节点驱动。</td>
</tr>
<tr>
<td>tdm-num</td>
<td>0~3</td>
<td>指定I2S序号，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>需和ahub(n)_plat的(n)对应。</td>
</tr>
<tr>
<td>tx-pin</td>
<td>0~3</td>
<td>指定I2S所使用的DOUT引脚序号。</td>
</tr>
<tr>
<td>rx-pin</td>
<td>0~3</td>
<td>指定I2S所使用的DIN引脚序号。</td>
</tr>
<tr>
<td>dai-type</td>
<td>&quot;I2S&quot;,&quot;HDMI&quot;</td>
<td>指定接口用作I2S或HDMI。</td>
</tr>
<tr>
<td>format</td>
<td>&quot;i2s&quot;,&quot;right_j&quot;,&quot;left_j&quot;,</td>
<td>选择tdm协议格式。</td>
</tr>
<tr>
<td></td>
<td>&quot;dsp_a&quot;,&quot;dsp_b&quot;</td>
<td></td>
</tr>
<tr>
<td>frame-master</td>
<td>cpu子节点，codec子节点</td>
<td>选择LRCK信号主模式。</td>
</tr>
<tr>
<td>bitclock-master</td>
<td>cpu子节点，codec子节点</td>
<td>选择BCLK信号主模式。</td>
</tr>
<tr>
<td>frame-inversion</td>
<td>注释为false, 反之为ture</td>
<td>LRCK信号是否翻转。</td>
</tr>
<tr>
<td>bitclock-inversion</td>
<td>注释为false, 反之为ture</td>
<td>BCLK信号是否翻转。</td>
</tr>
<tr>
<td>slot-num</td>
<td>1~16</td>
<td>slot数量（可简单理解为</td>
</tr>
<tr>
<td></td>
<td></td>
<td>支持最大通道数。）</td>
</tr>
<tr>
<td>slot-width</td>
<td>8, 16, 24, 32</td>
<td>单个slot宽度（可简单理解为</td>
</tr>
<tr>
<td></td>
<td></td>
<td>支持最大数据精度。）</td>
</tr>
<tr>
<td>mclk-fp</td>
<td>注释为false, 反之为ture</td>
<td>ture: mclk以固定频段输出。</td>
</tr>
<tr>
<td></td>
<td></td>
<td>false: mclk以采样率倍数输出。</td>
</tr>
<tr>
<td>mclk-fs</td>
<td>u32</td>
<td>固定频段：mclk =</td>
</tr>
<tr>
<td></td>
<td></td>
<td>mclk-fs * 12.288M or 11.2896M.</td>
</tr>
<tr>
<td></td>
<td></td>
<td>采样率倍数：mclk =</td>
</tr>
<tr>
<td></td>
<td></td>
<td>mclk-fs * pcm rate.</td>
</tr>
</tbody>
</table>
<h3 id="ahub-kernel-menuconfig-">AHUB kernel menuconfig配置说明</h3>
<p>linux-4.9 ~ linux-5.4，menuconfig必选配置如下。</p>
<pre><code>D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner AHUB support
                    &lt;M&gt;   Allwinner HDMIAUDIO Support
</code></pre><p>linux-5.10及其以上内核版本， menuconfig必选配置如下。</p>
<pre><code>A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner AHUB support
                &lt;M&gt;   Allwinner HDMIAUDIO Support
</code></pre><p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: I2S/PCM混音AHUB menuconfig可选配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner AHUB support</td>
<td>AHUB 模块。</td>
</tr>
<tr>
<td>Allwinner HDMIAUDIO Support</td>
<td>HDMI AUDIO 模块。</td>
</tr>
<tr>
<td>Allwinner function components</td>
<td>功能组件模块。</td>
</tr>
<tr>
<td>Components Debug</td>
<td>调试节点功能组件（查看音频寄存器）。</td>
</tr>
<tr>
<td>Enable audio dynamic debug</td>
<td>使能AUDIO模块DYNAMIC DEBUG模式。</td>
</tr>
</tbody>
</table>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分四大类驱动如下。</p>
<ul>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件 -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>AHUB DAM 和 AHUB 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动 和 ASoC codec 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_ahub_dam.ko
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_ahub.ko
<span class="hljs-comment"># HDMI Audio驱动（若使用HDMI Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_hdmi.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件介绍</h3>
<p><strong>控件列表</strong></p>
<p>ahub_dam 声卡</p>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'ahubdam'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">13</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                            <span class="hljs-keyword">value</span>
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF0 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF1 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF2 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S0 Src <span class="hljs-keyword">Select</span>                 <span class="hljs-keyword">NONE</span>
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S1 Src <span class="hljs-keyword">Select</span>                 <span class="hljs-keyword">NONE</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S2 Src <span class="hljs-keyword">Select</span>                 <span class="hljs-keyword">NONE</span>
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S3 Src <span class="hljs-keyword">Select</span>                 <span class="hljs-keyword">NONE</span>
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C0 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C1 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">9</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C2 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">10</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C0 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">11</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C1 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>
<span class="hljs-number">12</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C2 Src <span class="hljs-keyword">Select</span>               <span class="hljs-keyword">NONE</span>

# <span class="hljs-keyword">value</span> 可选项均为以下<span class="hljs-number">10</span>个
# <span class="hljs-keyword">NONE</span>
# APBIF_TXDIF0 APBIF_TXDIF1 APBIF_TXDIF2
# I2S0_TXDIF I2S1_TXDIF I2S2_TXDIF I2S3_TXDIF
# DAM0_TXDIF DAM1_TXDIF
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>APBIF0 Src Select</td>
<td>APBIF0 数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>APBIF1 Src Select</td>
<td>APBIF1数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>APBIF2 Src Select</td>
<td>APBIF2数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>I2S0 Src Select</td>
<td>I2S0数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>I2S1 Src Select</td>
<td>I2S1数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>I2S2 Src Select</td>
<td>I2S2数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>I2S3 Src Select</td>
<td>I2S3数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM0C0 Src Select</td>
<td>DAM0C0数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM0C1 Src Select</td>
<td>DAM0C1数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM0C2 Src Select</td>
<td>DAM0C2数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM1C0 Src Select</td>
<td>DAM1C0数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM1C1 Src Select</td>
<td>DAM1C1数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
<tr>
<td>DAM1C2 Src Select</td>
<td>DAM1C2数据源选择</td>
<td>0~9(对应控件value枚举值)</td>
</tr>
</tbody>
</table>
<p><strong>ahubi2s(n) 声卡(以ahubi2s0为例)</strong></p>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'ahubi2s0'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">1</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                             value
<span class="hljs-number">0</span>       BOOL    <span class="hljs-number">1</span>       loopback debug                   Off
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<p><strong>ahubhdmi 声卡</strong></p>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'ahubhdmi'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">2</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                                <span class="hljs-keyword">value</span>
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       audio <span class="hljs-keyword">data</span> <span class="hljs-keyword">format</span>                   PCM
<span class="hljs-number">1</span>       BOOL    <span class="hljs-number">1</span>       loopback debug                      Off

# 控件 <span class="hljs-number">0</span> <span class="hljs-keyword">value</span> 可选项如下。
# NULL PCM AC3 MPEG1 MP3 MPEG2 AAC DTS ATRAC ONE_BIT_AUDIO DOLBY_DIGITAL_PLUS DTS_HD MAT DST WMAPRO
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>audio data format</td>
<td>设置音频数据格式</td>
<td>0~14(对应控件value枚举值)</td>
</tr>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<h3 id="-">常见使用方法</h3>
<blockquote>
<p>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</p>
</blockquote>
<p><strong>确认声卡序号</strong></p>
<pre><code class="lang-bash">cat /<span class="hljs-keyword">proc</span>/asound/cards
0 [ahubdam   ]:<span class="hljs-title"> ahubdam</span> -<span class="hljs-title"> ahubdam</span>
<span class="hljs-title">                ahubdam</span>
1 [ahubi2s0  ]:<span class="hljs-title"> ahubi2s0</span> -<span class="hljs-title"> ahubi2s0</span>
<span class="hljs-title">                ahubi2s0</span>
2 [ahubhdmi  ]:<span class="hljs-title"> ahubhdmi</span> -<span class="hljs-title"> ahubhdmi</span>
<span class="hljs-title">                ahubhdmi</span>
3 [ahubi2s2  ]:<span class="hljs-title"> ahubi2s2</span> -<span class="hljs-title"> ahubi2s2</span>
<span class="hljs-title">                ahubi2s2</span>
</code></pre>
<p><strong>无混音播录</strong></p>
<p>无混音播录情况下，ahubdam 声卡控件保持默认，不做更改。</p>
<pre><code class="lang-bash"># ahubdam 声卡控件默认值
Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'ahubdam'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">13</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                                     <span class="hljs-keyword">value</span>

<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF0 Src <span class="hljs-keyword">Select</span>                        I2S0_TXDIF
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF1 Src <span class="hljs-keyword">Select</span>                        I2S1_TXDIF
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       APBIF2 Src <span class="hljs-keyword">Select</span>                        I2S2_TXDIF
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S0 Src <span class="hljs-keyword">Select</span>                          APBIF_TXDIF0
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S1 Src <span class="hljs-keyword">Select</span>                          APBIF_TXDIF1
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S2 Src <span class="hljs-keyword">Select</span>                          APBIF_TXDIF2
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       I2S3 Src <span class="hljs-keyword">Select</span>                          <span class="hljs-keyword">NONE</span>
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C0 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C1 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>
<span class="hljs-number">9</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM0C2 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>
<span class="hljs-number">10</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C0 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>
<span class="hljs-number">11</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C1 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>
<span class="hljs-number">12</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAM1C2 Src <span class="hljs-keyword">Select</span>                        <span class="hljs-keyword">NONE</span>

# ahubi2s0 声卡录音，channel 6bit 8000hz
tinycap test.wav -D <span class="hljs-number">1</span> -c <span class="hljs-number">2</span> -b <span class="hljs-number">16</span> -r <span class="hljs-number">48000</span> -T <span class="hljs-number">10</span>

# ahubi2s0 声卡播放
tinyplay test.wav -D <span class="hljs-number">1</span>

# ahubi2s0 声卡内部回环，播录音频格式需保持一致
tinymix -D <span class="hljs-number">1</span> <span class="hljs-string">"loopback debug"</span> <span class="hljs-number">1</span>
tinyplay test_play.wav -D <span class="hljs-number">1</span> &amp;
tinycap test_cap.wav -D <span class="hljs-number">1</span> -c <span class="hljs-number">2</span> -b <span class="hljs-number">16</span> -r <span class="hljs-number">48000</span> -T <span class="hljs-number">10</span>

# ahubhdmi 声卡播放（PCM 数据格式）
tinymix -D <span class="hljs-number">2</span> <span class="hljs-string">"audio data format"</span> PCM
tinyplay test.wav -D <span class="hljs-number">2</span>

# ahubhdmi 声卡播放（透传数据格式，根据所需格式设置，以DTS格式为例）
tinymix -D <span class="hljs-number">2</span> <span class="hljs-string">"audio data format"</span> DTS
tinyplay test.wav -D <span class="hljs-number">2</span>
</code></pre>
<p>::: note</p>
<p>ahubhdmi 声卡透传数据格式播放时，需所接的外部HDMI设备支持所选透传格式播放。</p>
<p>:::</p>
<p><strong>混音播录</strong></p>
<blockquote>
<p>前提条件1：假设按照默认配置I2S和APB绑定关系如下。</p>
<ul>
<li>APB0 &lt;-&gt; I2S0</li>
<li>APB1 &lt;-&gt; I2S1(HDMI AUDIO)</li>
<li>APB2 &lt;-&gt; I2S2</li>
</ul>
<p>前提条件2：所有混音的音频格式(channel,bit,rate)均需保持一致</p>
</blockquote>
<p>混音播录情况下，ahubdam 声卡控件需根据实际需求更改，此处只做部分示例说明。</p>
<ul>
<li>使用DAM0混音器实现 ahubi2s0播放 + ahubhdmi播放 + ahubi2s2播放 从 ahubhdmi 混音播放</li>
</ul>
<pre><code class="lang-bash"><span class="hljs-meta"># 通路需求设置</span>
<span class="hljs-meta"># Playback ──&gt; APBIF0_TX ──&gt; DAM0C0 ──&gt; DAM0_TX ──&gt; I2S1(HDMI AUDIO)</span>
<span class="hljs-meta"># Playback ──&gt; APBIF1_TX ──&gt; DAM0C1 ──^</span>
<span class="hljs-meta"># Playback ──&gt; APBIF2_TX ──&gt; DAM0C2 ──^</span>

<span class="hljs-meta"># 设置 DAM0 混音器数据源</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C0 Src Select"</span> APBIF_TXDIF0
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C1 Src Select"</span> APBIF_TXDIF1
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C2 Src Select"</span> APBIF_TXDIF2

<span class="hljs-meta"># 设置 I2S1(HDMI AUDIO) 播放所需数据源</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"I2S1 Src Select"</span> DAM0_TXDIF

<span class="hljs-meta"># test1.wav test2.wav test3.wav 从 ahubhdmi 声卡混音播放</span>
tinyplay test1.wav -D <span class="hljs-number">1</span>
tinyplay test2.wav -D <span class="hljs-number">2</span>
tinyplay test3.wav -D <span class="hljs-number">3</span>
</code></pre>
<ul>
<li>使用DAM0混音器实现 ahubi2s0录音 + ahubhdmi播放 + ahubi2s2播放 从 ahubhdmi 混音播放</li>
</ul>
<pre><code class="lang-bash"><span class="hljs-meta"># 通路需求设置</span>
<span class="hljs-meta"># Capture  ──&gt; I2S0_RX   ──&gt; DAM0C0 ──&gt; DAM0_TX ──&gt; I2S1(HDMI AUDIO)</span>
<span class="hljs-meta"># Playback ──&gt; APBIF1_TX ──&gt; DAM0C1 ──^</span>
<span class="hljs-meta"># Playback ──&gt; APBIF2_TX ──&gt; DAM0C2 ──^</span>

<span class="hljs-meta"># 设置 DAM0 混音器数据源</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C0 Src Select"</span> I2S0_TXDIF
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C1 Src Select"</span> APBIF_TXDIF1
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"DAM0C2 Src Select"</span> APBIF_TXDIF2

<span class="hljs-meta"># 设置 I2S1(HDMI AUDIO) 播放所需数据源</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"I2S1 Src Select"</span> DAM0_TXDIF

<span class="hljs-meta"># test1.wav test2.wav test3.wav 从 ahubhdmi 声卡混音播放</span>
tinycap test1.wav -D <span class="hljs-number">1</span> -c <span class="hljs-number">2</span> -b <span class="hljs-number">16</span> -r <span class="hljs-number">48000</span> -T <span class="hljs-number">10</span>
tinyplay test2.wav -D <span class="hljs-number">2</span>
tinyplay test3.wav -D <span class="hljs-number">3</span>
</code></pre>
<h2 id="dmic">DMIC</h2>
<h3 id="device-tree-">Device Tree 配置</h3>
<h4 id="-">配置路径</h4>
<p>设备树中定义的是该类芯片对应于IC规格的所有配置，设备树文件路径如下：</p>
<ul>
<li>linux-4.9、linux-5.4：</li>
</ul>
<blockquote>
<p>32位平台：kernel/{KERNEl_VER}/arch/arm/boot/dts/{CHIP}.dtsi</p>
<p>64位平台：kernel/{KERNEl_VER}/arch/arm64/boot/dts/sunxi/{CHIP}.dtsi</p>
</blockquote>
<ul>
<li>linux-5.10（含linux-5.10）之后：</li>
</ul>
<blockquote>
<p>32/64位平台：bsp/configs/{KERNEl_VER}/{CHIP}.dtsi</p>
</blockquote>
<p>:::note</p>
<ol>
<li>{KERNEl_VER}为内核版本，如linux-5.15；</li>
<li>{CHIP}.dtsi为具体芯片型号，如sun55iw3p1.dtsi。</li>
</ol>
<p>:::</p>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">dmic_plat:</span>dmic_plat@{module_base_reg} {
    <span class="hljs-comment">#sound-dai-cells = &lt;0&gt;;</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-plat-dmic"</span><span class="hljs-comment">;</span>
    reg             = &lt;<span class="hljs-number">0x0</span> {module_base_reg} <span class="hljs-number">0x0</span> <span class="hljs-number">0x50</span>&gt;<span class="hljs-comment">;</span>
    resets          = &lt;&amp;ccu RST_BUS_DMIC&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clocks </span>         = &lt;&amp;ccu CLK_BUS_{module}&gt;,
                      &lt;&amp;ccu CLK_PLL_AUDIO{n}&gt;,
                      &lt;&amp;ccu CLK_{module}&gt;<span class="hljs-comment">;</span>
    <span class="hljs-keyword">clock-names </span>    = <span class="hljs-string">"clk_bus_{module}"</span>,
                      <span class="hljs-string">"clk_pll_audio{n}"</span>,
                      <span class="hljs-string">"clk_{module}"</span><span class="hljs-comment">;</span>
    dmas            = &lt;&amp;dma1 {DRQ_PORT}&gt;<span class="hljs-comment">;</span>
    dma-names       = <span class="hljs-string">"rx"</span><span class="hljs-comment">;</span>
    capture-cma     = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    rx-fifo-size    = &lt;<span class="hljs-number">128</span>&gt;<span class="hljs-comment">;</span>
    status          = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
<span class="hljs-symbol">
dmic_mach:</span>dmic_mach{
    compatible              = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span><span class="hljs-comment">;</span>
    soundcard-mach,name     = <span class="hljs-string">"snddmic"</span><span class="hljs-comment">;</span>
    soundcard-mach,capture-only<span class="hljs-comment">;</span>
    status = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai = &lt;&amp;dmic_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>本部分仅为示例，具体内容根据实际情况填写。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>DMIC 模块由2个设备树节点构建。</p>
<p>1、ASoC层codec: 无，绑定虚拟codec节点。</p>
<p>2、ASoC层platform: dmic_plat</p>
<p>Table: DMIC dmic_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>reg</td>
<td>设置DMIC寄存器起始地址和地址长度。</td>
</tr>
<tr>
<td>resets</td>
<td>设置DMIC所需的复位时钟。</td>
</tr>
<tr>
<td>clocks</td>
<td>设置DMIC所需的时钟源和模块时钟。</td>
</tr>
<tr>
<td>clock-names</td>
<td>对clocks属性内容进行名称定义，用于辅助clocks属性获取。</td>
</tr>
<tr>
<td>capture-cma</td>
<td>设置录音流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>rx-fifo-size</td>
<td>设置录音流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>dmas</td>
<td>设置模块所绑定的dma通道号。</td>
</tr>
<tr>
<td>dma-names</td>
<td>对dmas属性内容进行名称定义，用于辅助dmas属性获取。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: dmic_mach</p>
<p>Table: DMIC dmic_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点（使用虚拟codec）。</td>
</tr>
<tr>
<td>capture-only</td>
<td>设置仅录音，不进行播放流设备创建。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）。</td>
</tr>
</tbody>
</table>
<h3 id="board-dts-">board.dts 配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash">&amp;dmic_plat {
    rx-chmap          = &lt;<span class="hljs-number">0x76543210</span>&gt;<span class="hljs-comment">;</span>
    data-vol          = &lt;<span class="hljs-number">0xB0</span>&gt;<span class="hljs-comment">;</span>
    rxdelaytime       = &lt;<span class="hljs-number">0</span>&gt;<span class="hljs-comment">;</span>
    <span class="hljs-comment">/* pinctrl-used; */</span>
    <span class="hljs-comment">/* pinctrl-names  = "default","sleep"; */</span>
    <span class="hljs-comment">/* pinctrl-0      = &lt;&amp;dmic_pins_a&gt;; */</span>
    <span class="hljs-comment">/* pinctrl-1      = &lt;&amp;dmic_pins_b&gt;; */</span>
    rx-<span class="hljs-keyword">sync-en;
</span>    status            = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;dmic_mach {
    status        = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai = &lt;&amp;dmic_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>gpio部分请参考附件<a href="#GPIO功能复用配置">GPIO功能复用配置</a></p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: DMIC 模块板级配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>status</td>
<td>&quot;okay&quot;, &quot;disabled&quot;</td>
<td>使能或关闭该节点驱动。</td>
</tr>
<tr>
<td>data-vol</td>
<td>0-&gt;255(-119.25-&gt;71.25dB)</td>
<td>录音数字端音量调节。</td>
</tr>
<tr>
<td>rxdelaytime</td>
<td>0,5,10,20,30</td>
<td>设置录音延迟时长，单位ms。</td>
</tr>
<tr>
<td>rx-chmap</td>
<td>u32(默认值为0x76543210)</td>
<td>四条 DATA 线的通道映射。</td>
</tr>
</tbody>
</table>
<h3 id="dmic-kernel-menuconfig-">DMIC kernel menuconfig 配置说明</h3>
<p>linux-5.10以下内核版本，menuconfig必选配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner DMIC support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig必选配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner DMIC support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: DMIC menuconfig可选配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner DMIC support</td>
<td>DMIC 模块。</td>
</tr>
<tr>
<td>Allwinner function components</td>
<td>功能组件模块。</td>
</tr>
<tr>
<td>Components Debug</td>
<td>调试节点功能组件（查看音频寄存器）。</td>
</tr>
<tr>
<td>Enable audio dynamic debug</td>
<td>使能AUDIO模块DYNAMIC DEBUG模式。</td>
</tr>
</tbody>
</table>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分四大类驱动如下。</p>
<ul>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件 -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>DMIC 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动 和 ASoC codec 驱动(NULL)</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_dmic.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件介绍</h3>
<pre><code class="lang-bash">Mixer name: <span class="hljs-string">'snddmic'</span>
Number of controls: <span class="hljs-number">9</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       rx sync mode                             &gt;Off On
<span class="hljs-number">1</span>       INT     <span class="hljs-number">1</span>       L0 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">2</span>       INT     <span class="hljs-number">1</span>       R0 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">3</span>       INT     <span class="hljs-number">1</span>       L1 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">4</span>       INT     <span class="hljs-number">1</span>       R1 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">5</span>       INT     <span class="hljs-number">1</span>       L2 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">6</span>       INT     <span class="hljs-number">1</span>       R2 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">7</span>       INT     <span class="hljs-number">1</span>       L3 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)
</span><span class="hljs-number">8</span>       INT     <span class="hljs-number">1</span>       R3 <span class="hljs-keyword">volume</span><span class="bash">                                176 (dsrange 0-&gt;255)</span>
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>rx sync mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>L0 volume</td>
<td>DIN0 左声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>R0 volume</td>
<td>DIN0 右声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>L1 volume</td>
<td>DIN1 左声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>R1 volume</td>
<td>DIN1 右声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>L2 volume</td>
<td>DIN2 左声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>R2 volume</td>
<td>DIN2 右声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>L3 volume</td>
<td>DIN3 左声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
<tr>
<td>R3 volume</td>
<td>DIN3 右声道音量调节</td>
<td>0-&gt;255 (-119.25-&gt;71.25dB)</td>
</tr>
</tbody>
</table>
<h3 id="-">常用使用方法</h3>
<blockquote>
<p>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</p>
</blockquote>
<p><strong>录音</strong></p>
<pre><code class="lang-bash"><span class="hljs-comment"># 确认声卡序号</span>
cat /proc/asound/cards
5 [sndi2s0     ]: sndhdmi - sndhdmi
                  sndhdmi

<span class="hljs-comment"># 2channel 16bit 48000rate</span>
tinycap test.wav -D<span class="hljs-number"> 5 </span>-c<span class="hljs-number"> 2 </span>-b<span class="hljs-number"> 16 </span>-r<span class="hljs-number"> 48000 </span>-T 10

<span class="hljs-comment"># 8channel 16bit 48000rate</span>
tinycap test.wav -D<span class="hljs-number"> 5 </span>-c<span class="hljs-number"> 8 </span>-b<span class="hljs-number"> 16 </span>-r<span class="hljs-number"> 48000 </span>-T 10
</code></pre>
<h2 id="owa">OWA</h2>
<h3 id="device-tree-">Device Tree 配置</h3>
<h4 id="-">配置路径</h4>
<p>设备树中定义的是该类芯片对应于IC规格的所有配置，设备树文件路径如下：</p>
<ul>
<li>linux-4.9、linux-5.4：</li>
</ul>
<blockquote>
<p>32位平台：kernel/{KERNEl_VER}/arch/arm/boot/dts/{CHIP}.dtsi</p>
<p>64位平台：kernel/{KERNEl_VER}/arch/arm64/boot/dts/sunxi/{CHIP}.dtsi</p>
</blockquote>
<ul>
<li>linux-5.10（含linux-5.10）之后：</li>
</ul>
<blockquote>
<p>32/64位平台：bsp/configs/{KERNEl_VER}/{CHIP}.dtsi</p>
</blockquote>
<p>:::note</p>
<ol>
<li>{KERNEl_VER}为内核版本，如linux-5.15;</li>
<li>{CHIP}.dtsi为具体芯片型号，如sun50iw10p1.dtsi。</li>
</ol>
<p>:::</p>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">owa_plat:</span>owa_plat@{module_base_reg} {
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible      = <span class="hljs-string">"allwinner,sunxi-snd-plat-owa"</span>;
    reg             = <span class="hljs-params">&lt;<span class="hljs-number">0x0</span> {module_base_reg} <span class="hljs-number">0x0</span> <span class="hljs-number">0x58</span>&gt;</span>;
    resets          = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> RST_BUS_OWA&gt;</span>;
    clocks          = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_BUS_{module}&gt;</span>,
                      <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_PLL_DSP_AUDIO{n}&gt;</span>,
                      <span class="hljs-params">&lt;<span class="hljs-variable">&amp;ccu</span> CLK_{modele}&gt;</span>;
    clock-names     = <span class="hljs-string">"clk_bus_owa"</span>,
                      <span class="hljs-string">"clk_pll_audio{n}"</span>,
                      <span class="hljs-string">"clk_{module}"</span>;
    dmas            = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;dma1</span> {DRQ_PORT}&gt;</span>, <span class="hljs-params">&lt;<span class="hljs-variable">&amp;dma1</span> {DRQ_PORT}&gt;</span>;
    dma-names       = <span class="hljs-string">"tx"</span>, <span class="hljs-string">"rx"</span>;
    playback-cma    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    capture-cma     = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    tx-fifo-size    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    rx-fifo-size    = <span class="hljs-params">&lt;<span class="hljs-number">128</span>&gt;</span>;
    status          = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-symbol">
owa_mach:</span><span class="hljs-class">owa_mach </span>{
    compatible              = <span class="hljs-string">"allwinner,sunxi-snd-mach"</span>;
    soundcard-mach,name     = <span class="hljs-string">"sndowa"</span>;
    status                  = <span class="hljs-string">"disabled"</span>;
    soundcard-mach,<span class="hljs-class">cpu </span>{
        sound-dai = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;owa_plat</span>&gt;</span>;
    };
    soundcard-mach,<span class="hljs-class">codec </span>{
    };
};
</code></pre>
<p>::: note</p>
<p>本部分仅为示例，具体内容根据实际情况填写。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>OWA 模块由2个设备树节点构建。</p>
<p>1、ASoC层codec: 无，绑定虚拟codec节点。</p>
<p>2、ASoC层platform: owa_plat</p>
<p>Table: OWA owa_plat 节点配置项(linux-5.10)</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>reg</td>
<td>设置OWA寄存器起始地址和地址长度。</td>
</tr>
<tr>
<td>resets</td>
<td>设置OWA所需的复位时钟。</td>
</tr>
<tr>
<td>clocks</td>
<td>设置OWA所需的时钟源和模块时钟。</td>
</tr>
<tr>
<td>clock-names</td>
<td>对clocks属性内容进行名称定义，用于辅助clocks属性获取。</td>
</tr>
<tr>
<td>playback-cma</td>
<td>设置播放流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>capture-cma</td>
<td>设置录音流DMA申请size大小，为(2^n)Kbyte，单位Kb。</td>
</tr>
<tr>
<td>tx-fifo-size</td>
<td>设置播放流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>rx-fifo-size</td>
<td>设置录音流的fifo_size大小，用于声卡参数限定，单位Kb。</td>
</tr>
<tr>
<td>dmas</td>
<td>设置模块所绑定的dma通道号。</td>
</tr>
<tr>
<td>dma-names</td>
<td>对dmas属性内容进行名称定义，用于辅助dmas属性获取。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: owa_mach</p>
<p>Table: OWA owa_mach 节点配置项(linux-5.10)</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>soundcard-mach,</td>
<td>machine层配置前缀。</td>
</tr>
<tr>
<td>name</td>
<td>声卡名字。</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的cpu节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点（使用虚拟codec）。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）。</td>
</tr>
</tbody>
</table>
<h3 id="board-dts-">board.dts 配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="-">配置示例</h4>
<pre><code class="lang-bash">&amp;owa_plat {
    <span class="hljs-comment">/* pinctrl-used; */</span>
    <span class="hljs-comment">/* pinctrl-names    = "default","sleep"; */</span>
    <span class="hljs-comment">/* pinctrl-0        = &lt;&amp;owa_pins_a&gt;; */</span>
    <span class="hljs-comment">/* pinctrl-1        = &lt;&amp;owa_pins_b&gt;; */</span>
    tx-hub-en<span class="hljs-comment">;</span>
    status              = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;owa_mach {
    status              = <span class="hljs-string">"disabled"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai   = &lt;&amp;owa_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>::: note</p>
<p>gpio部分请参考附件<a href="#GPIO功能复用配置">GPIO功能复用配置</a></p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: OWA 模块板级配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>status</td>
<td>&quot;okay&quot;, &quot;disabled&quot;</td>
<td>使能或关闭该节点驱动</td>
</tr>
<tr>
<td>tx-hub-en</td>
<td>注释为false, 反之为ture</td>
<td>选择是否注册txhub控件</td>
</tr>
</tbody>
</table>
<h3 id="owa-in-owa-out-">OWA-IN 与 OWA-OUT 调试说明</h3>
<p>1、配置owa-in时，只需从原理图中查询到所使用的GPIO，完成board.dts中的pinctrl配置，且开机后可正常注册owa声卡即可使用；配置owa-out同理；</p>
<pre><code class="lang-bash">&amp;pio {
    <span class="hljs-comment">/* 假设PH6为owa-in，PH7为owa-out */</span>
<span class="hljs-symbol">    owa_pins_a:</span> owa@<span class="hljs-number">0</span> {
        pins = <span class="hljs-string">"PH6"</span>, <span class="hljs-string">"PH7"</span><span class="hljs-comment">;</span>
        function = <span class="hljs-string">"owa"</span><span class="hljs-comment">;</span>
        drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
        <span class="hljs-keyword">bias-disable;
</span>    }<span class="hljs-comment">;</span>
<span class="hljs-symbol">
    owa_pins_b:</span> owa@<span class="hljs-number">1</span> {
        pins = <span class="hljs-string">"PH6"</span>, <span class="hljs-string">"PH7"</span><span class="hljs-comment">;</span>
        function = <span class="hljs-string">"io_disabled"</span><span class="hljs-comment">;</span>
        drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
        <span class="hljs-keyword">bias-disable;
</span>    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;owa_plat {
    pinctrl-used<span class="hljs-comment">;</span>
    pinctrl-names    = <span class="hljs-string">"default"</span>,<span class="hljs-string">"sleep"</span><span class="hljs-comment">;</span>
    pinctrl-0        = &lt;&amp;owa_pins_a&gt;<span class="hljs-comment">;</span>
    pinctrl-1        = &lt;&amp;owa_pins_b&gt;<span class="hljs-comment">;</span>
    tx-hub-en<span class="hljs-comment">;</span>
    status              = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>

&amp;owa_mach {
    status              = <span class="hljs-string">"okay"</span><span class="hljs-comment">;</span>
    soundcard-mach,cpu {
        sound-dai   = &lt;&amp;owa_plat&gt;<span class="hljs-comment">;</span>
    }<span class="hljs-comment">;</span>
    soundcard-mach,codec {
    }<span class="hljs-comment">;</span>
}<span class="hljs-comment">;</span>
</code></pre>
<p>2、测试owa输入输出功能时，无需打开任何控件，直接使用tinyplay或tinycap即可；</p>
<p>3、owa-in若无法采集数据，一是需要测量owa-in端口是否有信号，二是需确认输入数据格式是否符合IEC-60958或IEC-61937协议格式；</p>
<p>3、调试soc端的owa功能是否正常，可短接soc的owa-in与owa-out引脚，先执行tinyplay使用owa声卡播放，再执行tinycap使用owa声卡录音，若能够录到正常声音，说明soc端的owa配置是正常的；</p>
<p>4、若播放时出现声音播放速度明显变慢或变快的情况，可能是时钟配置错误导致，此时请提AService咨询全志工程师处理。</p>
<h3 id="owa-kernel-menuconfig-">OWA kernel menuconfig 配置说明</h3>
<p>linux-5.10以下内核版本，menuconfig必选配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner OWA Support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig必须配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner OWA Support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: OWA menuconfig可选配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner OWA support</td>
<td>OWA 模块。</td>
</tr>
<tr>
<td>Components Rx Sync</td>
<td>同步采样功能组件。</td>
</tr>
<tr>
<td>Allwinner function components</td>
<td>功能组件模块。</td>
</tr>
<tr>
<td>Components Debug</td>
<td>调试节点功能组件（查看音频寄存器）。</td>
</tr>
<tr>
<td>Enable audio dynamic debug</td>
<td>使能AUDIO模块DYNAMIC DEBUG模式。</td>
</tr>
</tbody>
</table>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分四大类驱动如下。</p>
<ul>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件 -&gt; 特殊功能组件 -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>OWA 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动 和 ASoC codec 驱动(NULL)</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_owa.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件</h3>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'sndowa'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">9</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                                     <span class="hljs-keyword">value</span>
        <span class="hljs-built_in">range</span>/values
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              &gt;Off On
<span class="hljs-number">1</span>       BOOL    <span class="hljs-number">1</span>       loopback debug                           Off
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       audio tx <span class="hljs-keyword">data</span> <span class="hljs-keyword">format</span>                     &gt;PCM RAW
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       audio rx <span class="hljs-keyword">data</span> <span class="hljs-keyword">format</span>                     &gt;PCM RAW
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx channel cnt                           &gt;<span class="hljs-number">0</span> <span class="hljs-number">1</span> <span class="hljs-number">2</span> <span class="hljs-number">3</span> <span class="hljs-number">4</span> <span class="hljs-number">5</span> <span class="hljs-number">6</span> <span class="hljs-number">7</span> <span class="hljs-number">8</span> <span class="hljs-number">9</span> <span class="hljs-number">10</span> <span class="hljs-number">11</span> <span class="hljs-number">12</span> <span class="hljs-number">13</span> <span class="hljs-number">14</span> <span class="hljs-number">15</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx word length                           &gt;Null 6bits 7bits 8bits 9bits 0bits 1bits 2bits 3bits 4bits
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx rate                                  &gt;Null <span class="hljs-number">22.</span>5kHz 4kHz 2kHz <span class="hljs-number">44.</span>kHz 8kHz 6kHz <span class="hljs-number">176.</span>kHz 92kHz 68kHz
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx origin rate                           &gt;Null kHz <span class="hljs-number">11.</span>25kHz 2kHz 6kHz <span class="hljs-number">22.</span>5kHz 4kHz 2kHz <span class="hljs-number">44.</span>kHz 8kHz <span class="hljs-number">88.</span>kHz 6kHz <span class="hljs-number">176.</span>kHz 92kHz
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx <span class="hljs-keyword">data</span> <span class="hljs-keyword">type</span>                             &gt;Linear_PCM Nonlinear_PCM
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>tx hub mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>audio tx data format</td>
<td>设置播放音频数据格式</td>
<td>0~1(对应控件value枚举值)</td>
</tr>
<tr>
<td>audio rx data format</td>
<td>设置录音音频数据格式</td>
<td>0~1(对应控件value枚举值)</td>
</tr>
<tr>
<td>rx channel cnt</td>
<td>获取当前录音通道数（IEC格式信息）</td>
<td>0~15（只读）</td>
</tr>
<tr>
<td>rx word length</td>
<td>获取当前录音数据位深（IEC格式信息)</td>
<td>16~24bits（只读）</td>
</tr>
<tr>
<td>rx rate</td>
<td>获取当前录音采样率（IEC格式信息）</td>
<td>8~768kHz（只读）</td>
</tr>
<tr>
<td>rx origin rate</td>
<td>获取当前录音采样率（IEC格式信息)</td>
<td>8~192kHz（只读）</td>
</tr>
<tr>
<td>rx data type</td>
<td>获取当前录音数据格式（IEC格式信息）</td>
<td>Linear_PCM;Nonlinear_PCM（只读)</td>
</tr>
</tbody>
</table>
<h3 id="-">常用使用方法</h3>
<blockquote>
<p>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</p>
</blockquote>
<p><strong>播放</strong></p>
<pre><code class="lang-bash"><span class="hljs-comment"># 确认声卡序号</span>
cat /<span class="hljs-keyword">proc</span>/asound/cards
1 [sndi2s0     ]:<span class="hljs-title"> sndowa</span> -<span class="hljs-title"> sndowa</span>
<span class="hljs-title">                  sndowa</span>
<span class="hljs-title">
tinyplay</span> test.wav -D 1

#透传播放<span class="hljs-title">
tinymix</span> -D 0 "audio<span class="hljs-title"> tx</span> data<span class="hljs-title"> format"</span> RAW<span class="hljs-title">
tinyplay</span> test.dts -D 1
</code></pre>
<h2 id="hdmi-edp-av">HDMI EDP AV</h2>
<h3 id="board-dts-">board.dts 配置</h3>
<h4 id="-">配置路径</h4>
<p>board.dts 用于保存每一个板级平台的设备信息（如demo板，perf1板等），里面的配置信息会覆盖上面的Device Tree中dtsi默认配置信息。
不同IC、版型及内核版本对应的board.dts具体路径如下。</p>
<blockquote>
<p>device/config/chips/{PLATFORM}/configs/{BOARD}/{KERNEl_VER}/board.dts</p>
</blockquote>
<h4 id="hdmi-tx-">HDMI TX配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">hdmi_codec:</span><span class="hljs-class">hdmi_codec </span>{
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible    = <span class="hljs-string">"allwinner,sunxi-snd-codec-hdmi"</span>;
    status = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_plat </span>{
    tx-pin          = <span class="hljs-params">&lt;<span class="hljs-number">0</span> <span class="hljs-number">1</span> <span class="hljs-number">2</span> <span class="hljs-number">3</span>&gt;</span>;
    dai-type        = <span class="hljs-string">"hdmi"</span>;
    status            = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_mach </span>{
    ...
    soundcard-mach,playback-only;
    i2s{n}_cpu: soundcard-mach,<span class="hljs-class">cpu </span>{
        sound-dai = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;i2s</span>{n}_plat&gt;</span>;
        soundcard-mach,pll-fs    = <span class="hljs-params">&lt;<span class="hljs-number">1</span>&gt;</span>;
        soundcard-mach,mclk-fs    = <span class="hljs-params">&lt;<span class="hljs-number">0</span>&gt;</span>;
    };
    i2s{n}_codec: soundcard-mach,<span class="hljs-class">codec </span>{
        sound-dai               = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;hdmi_codec</span>&gt;</span>;
    };
};
</code></pre>
<p>::: note</p>
<p>HDMI TX连接哪一路I2S需根据I2S SPEC说明或公版SDK DTS示例确定。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>HDMI TX 音频组成模块由3个设备树节点构建。</p>
<p>1、ASoC层codec: hdmi_codec</p>
<p>Table: hdmi_codec 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
<tr>
<td>tx-pin</td>
<td>I2S的各路DOUT引脚均与HDMI模块连接，为固定配置。</td>
</tr>
<tr>
<td>dai-type</td>
<td>指定接口类型为HDMI，为固定配置。</td>
</tr>
</tbody>
</table>
<p>2、ASoC层platform: i2s(n)_plat</p>
<p>Table: i2s(n)_plat 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
</tbody>
</table>
<p>3、ASoC层machine: i2s(n)_mach</p>
<p>Table: I2S/PCM i2s(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>playback-only</td>
<td>HDMI TX仅用于播放</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的platform节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
</tbody>
</table>
<h4 id="hdmi-rx-">HDMI RX配置示例</h4>
<pre><code class="lang-bash">&amp;i2s{n}_plat {
    ...
};
&amp;i2s{n}_mach {
    ...
    soundcard-mach,capture-only;
    i2s{n}_cpu: soundcard-mach,cpu {
        sound-dai = &lt;&amp;i2s{n}_plat&gt;;
        ...
    };
    i2s{n}_codec: soundcard-mach,codec {
    };
};
</code></pre>
<p>::: note</p>
<p>HDMI RX连接哪一路I2S需根据I2S SPEC说明或公版SDK DTS示例确定。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>HDMI RX 音频组成模块由2个设备树节点构建，无需打开hdmi_codec节点，根据手册配置正确的I2S格式。</p>
<p>1、ASoC层platform: i2s(n)_plat</p>
<p>2、ASoC层machine: i2s(n)_mach</p>
<p>Table: I2S/PCM i2s(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>capture-only</td>
<td>HDMI RX仅用于录音</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的platform节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点，</td>
</tr>
<tr>
<td></td>
<td>若该子节点下无sound-dai属性，即代表使用虚拟codec，用于辅助生成声卡。</td>
</tr>
</tbody>
</table>
<h4 id="edp-tx-">EDP TX配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-symbol">edp_codec:</span><span class="hljs-class">edp_codec </span>{
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible    = <span class="hljs-string">"allwinner,sunxi-snd-codec-edp"</span>;
    status = <span class="hljs-string">"disabled"</span>;
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_plat </span>{
    ...
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_mach </span>{
    ...
    soundcard-mach,playback-only;
    i2s{n}_cpu: soundcard-mach,<span class="hljs-class">cpu </span>{
        sound-dai = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;i2s</span>{n}_plat&gt;</span>;
        soundcard-mach,pll-fs    = <span class="hljs-params">&lt;<span class="hljs-number">4</span>&gt;</span>;
        soundcard-mach,mclk-fs    = <span class="hljs-params">&lt;<span class="hljs-number">512</span>&gt;</span>;
    };
    i2s{n}_codec: soundcard-mach,<span class="hljs-class">codec </span>{
        sound-dai               = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;edp_codec</span>&gt;</span>;
    };
};
</code></pre>
<p>::: note</p>
<p>EDP TX连接哪一路I2S需根据I2S SPEC说明或公版SDK DTS示例确定。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>EDP TX 音频组成模块由3个设备树节点构建。</p>
<p>1、ASoC层codec: edp_codec</p>
<p>Table: edp_codec 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
</tbody>
</table>
<p>2、ASoC层platform: i2s(n)_plat</p>
<p>3、ASoC层machine: i2s(n)_mach</p>
<p>Table: I2S/PCM i2s(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>playback-only</td>
<td>EDP TX仅用于播放</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的platform节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）,需设置此值为4。</td>
</tr>
<tr>
<td>mclk-fs</td>
<td>本节点无mclk-fp属性,故mclk以采样率倍数输出（mclk = mclk-fs * pcm rate），</td>
</tr>
<tr>
<td></td>
<td>需设置此值为512。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
</tbody>
</table>
<h4 id="av-tx-">AV TX配置示例</h4>
<pre><code class="lang-bash"><span class="hljs-variable">&amp;drm_edp</span> {
    <span class="hljs-meta">#sound-dai-cells = &lt;0&gt;;</span>
    compatible = <span class="hljs-string">"allwinner,drm-edp"</span>;
    status = <span class="hljs-string">"disabled"</span>;
    ...
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_plat </span>{
    ...
};
<span class="hljs-variable">&amp;i2s</span>{n}<span class="hljs-class">_mach </span>{
    ...
    soundcard-mach,playback-only;
    i2s{n}_cpu: soundcard-mach,<span class="hljs-class">cpu </span>{
        sound-dai = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;i2s</span>{n}_plat&gt;</span>;
        soundcard-mach,pll-fs    = <span class="hljs-params">&lt;<span class="hljs-number">4</span>&gt;</span>;
        soundcard-mach,mclk-fs    = <span class="hljs-params">&lt;<span class="hljs-number">512</span>&gt;</span>;
    };
    i2s{n}_codec: soundcard-mach,<span class="hljs-class">codec </span>{
        sound-dai               = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;drm_edp</span>&gt;</span>;
    };
};
</code></pre>
<p>::: note</p>
<p>AV TX连接哪一路I2S需根据I2S SPEC说明或公版SDK DTS示例确定。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>AV TX 音频组成模块由2个设备树节点构建，另外需要使用edp模块的设备树节点辅助构建，AV TX由EDP TX升级而来，供1903平台及往后新平台应用。</p>
<p>1、ASoC层codec: drm_edp</p>
<p>Table: drm_edp 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#sound-dai-cells</td>
<td>machine层检测codec和platform节点的标志。</td>
</tr>
</tbody>
</table>
<p>2、ASoC层platform: i2s(n)_plat</p>
<p>3、ASoC层machine: i2s(n)_mach</p>
<p>Table: I2S/PCM i2s(n)_mach 节点配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>playback-only</td>
<td>AV TX仅用于播放</td>
</tr>
<tr>
<td>cpu</td>
<td>machine层所绑定的platform节点（即platform层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
<tr>
<td>pll-fs</td>
<td>指定模块时钟源频率（24.576M or 22.5792M * pll-fs）,需设置此值为4。</td>
</tr>
<tr>
<td>mclk-fs</td>
<td>本节点无mclk-fp属性,故mclk以采样率倍数输出（mclk = mclk-fs * pcm rate），</td>
</tr>
<tr>
<td></td>
<td>需设置此值为512。</td>
</tr>
<tr>
<td>codec</td>
<td>machine层所绑定的codec节点（即codec层），</td>
</tr>
<tr>
<td></td>
<td>用sound-dai属性指定节点。</td>
</tr>
</tbody>
</table>
<h3 id="hdmi-tx-kernel-menuconfig-">HDMI TX kernel menuconfig 配置说明</h3>
<p>linux-5.10以下内核版本，menuconfig配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner I2S support
                    &lt;M&gt;   Allwinner HDMIAUDIO Support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner I2S support
                &lt;M&gt;   Allwinner HDMIAUDIO Support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: menuconfig配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner I2S support</td>
<td>I2S 模块。</td>
</tr>
<tr>
<td>Allwinner HDMIAUDIO Support</td>
<td>HDMI TX 模块。</td>
</tr>
</tbody>
</table>
<h3 id="hdmi-rx-kernel-menuconfig-">HDMI RX kernel menuconfig 配置说明</h3>
<p>linux-5.10以下内核版本，menuconfig配置如下。</p>
<pre><code class="lang-bash">D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner I2S support
</code></pre>
<p>linux-5.10及其以上内核版本， menuconfig配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner I2S support
</code></pre>
<p>::: note</p>
<ol>
<li>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</li>
<li>HDMI RX无需打开Allwinner HDMIAUDIO Support。</li>
</ol>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: menuconfig配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner I2S support</td>
<td>I2S 模块。</td>
</tr>
</tbody>
</table>
<h3 id="edp-tx-kernel-menuconfig-">EDP TX kernel menuconfig 配置说明</h3>
<p>该驱动仅支持linux-5.10及其以上内核版本， menuconfig配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner I2S support
                &lt;M&gt;   Allwinner EDPAUDIO support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: menuconfig配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner I2S support</td>
<td>I2S 模块。</td>
</tr>
<tr>
<td>Allwinner EDPAUDIO Support</td>
<td>EDP TX 模块。</td>
</tr>
</tbody>
</table>
<h3 id="av-tx-kernel-menuconfig-">AV TX kernel menuconfig 配置说明</h3>
<p>该驱动仅支持linux-5.10及其以上内核版本， menuconfig配置如下。</p>
<pre><code class="lang-bash">A<span class="hljs-function"><span class="hljs-title">llwinner</span> BSP  ---&gt;</span>
    D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
        SOUND D<span class="hljs-function"><span class="hljs-title">rivers</span>  ---&gt;</span>
            P<span class="hljs-function"><span class="hljs-title">latform</span> drivers  ---&gt;</span>
                &lt;M&gt; Allwinner I2S support
                &lt;M&gt;   Allwinner AVAUDIO support
</code></pre>
<p>::: note</p>
<p>选择需要的模块，可选择直接编译进内核（Y），也可编译成模块（M）。</p>
<p>:::</p>
<p><strong>配置项说明</strong></p>
<p>Table: menuconfig配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>Allwinner I2S support</td>
<td>I2S 模块。</td>
</tr>
<tr>
<td>Allwinner AVAUDIO Support</td>
<td>AV TX 模块。</td>
</tr>
</tbody>
</table>
<h3 id="-ko-">加载与卸载方法（ko方式）</h3>
<p>sunxi音频驱动模块分四大类驱动如下。</p>
<ul>
<li>PCM 驱动</li>
<li>ASoC platfrom 驱动</li>
<li>ASoC codec 驱动</li>
<li>ASoC machine 驱动</li>
</ul>
<p>ko加载顺序遵循 <strong>“公共组件  -&gt; PCM 驱动 -&gt; ASoC platfrom 驱动 或 ASoC codec 驱动 -&gt; ASoC machine 驱动”</strong> 顺序，卸载顺序则相反。</p>
<p>I2S/PCM 声卡加载顺序如下。</p>
<pre><code class="lang-bash"><span class="hljs-comment"># 公共组件，提供公共接口</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_common.ko

<span class="hljs-comment"># PCM 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_pcm.ko

<span class="hljs-comment"># ASoC platfrom 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_i2s.ko

<span class="hljs-comment"># HDMI TX Audio驱动（若使用HDMI TX Audio，HDMI RX Audio无需加载）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_hdmi.ko
<span class="hljs-comment"># EDP TX Audio驱动（若使用EDP TX Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_edp.ko
<span class="hljs-comment"># AV TX Audio驱动（若使用AV TX Audio）</span>
<span class="hljs-keyword">insmod </span>snd_soc_codec_av.ko

<span class="hljs-comment"># ASoC machine 驱动</span>
<span class="hljs-keyword">insmod </span>snd_soc_sunxi_machine.ko
</code></pre>
<h3 id="-">声卡控件</h3>
<p><strong>控件列表</strong></p>
<h4 id="hdmi-tx-">HDMI TX 控件</h4>
<pre><code class="lang-bash">Mixer <span class="hljs-keyword">name</span>: <span class="hljs-string">'sndhdmi'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">3</span>
ctl     <span class="hljs-keyword">type</span>    num     <span class="hljs-keyword">name</span>                        <span class="hljs-keyword">value</span>
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       audio <span class="hljs-keyword">data</span> <span class="hljs-keyword">format</span>           NULL &gt;PCM AC3 MPEG1 MP3 MPEG2 AAC DTS ATRAC ONE_BIT_AUDIO
                                                    DOLBY_DIGITAL_PLUS DTS_HD MAT DST WMAPRO
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                 &gt;Off On
<span class="hljs-number">2</span>       BOOL    <span class="hljs-number">1</span>       loopback debug              Off
</code></pre>
<p>Table: 控件说明</p>
<table>
<thead>
<tr>
<th>控件名称</th>
<th>功能</th>
<th>数值</th>
</tr>
</thead>
<tbody>
<tr>
<td>audio data format</td>
<td>设置音频数据格式</td>
<td>NULL PCM AC3 MPEG1 MP3 MPEG2 AAC DTS</td>
</tr>
<tr>
<td></td>
<td></td>
<td>ATRAC ONE_BIT_AUDIO DOLBY_DIGITAL_PLUS</td>
</tr>
<tr>
<td></td>
<td></td>
<td>DTS_HD MAT DST WMAPRO</td>
</tr>
<tr>
<td>tx hub mode</td>
<td>同源播放开关</td>
<td>Off;On</td>
</tr>
<tr>
<td>loopback debug</td>
<td>内部回录开关</td>
<td>Off;On</td>
</tr>
</tbody>
</table>
<p>::: note</p>
<p>HDMI RX or EDP TX or AV TX均无特殊控件。</p>
<p>:::</p>
<h3 id="-">常用使用方法</h3>
<blockquote>
<p>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节；</p>
</blockquote>
<h4 id="hdmi-tx-">hdmi TX 常见使用方法</h4>
<pre><code class="lang-bash"><span class="hljs-comment"># 确认声卡序号</span>
cat /<span class="hljs-keyword">proc</span>/asound/cards
4 [sndhdmi     ]:<span class="hljs-title"> sndhdmi</span> -<span class="hljs-title"> sndhdmi</span>
<span class="hljs-title">                  sndhdmi</span>

#<span class="hljs-title"> sndhdmi</span> 声卡播放<span class="hljs-title">
tinyplay</span> test.wav -D 4

#<span class="hljs-title"> sndhdmi</span> 数据透传<span class="hljs-title">
tinymix</span> -D 4 "audio<span class="hljs-title"> data</span> format"<span class="hljs-title"> DTS</span>
tinyplay<span class="hljs-title"> test.dts</span> -D 4
</code></pre>
<p>::: note</p>
<p>HDMI RX or EDP TX or AV TX按照普通I2S声卡使用即可。</p>
<p>:::</p>
<h1 id="-">驱动架构介绍</h1>
<h2 id="-">软件框图</h2>
<h3 id="alsa-">ALSA 音频架构</h3>
<p><img src="figures/ALSA音频架构.png" alt="ALSA 音频架构"></p>
<h3 id="asoc-">ASoC 驱动框架</h3>
<p><img src="figures/ASoC驱动框架.png" alt="ASoC 驱动框架"></p>
<h3 id="-asoc-sunxi-">基于 ASoC 的 sunxi 驱动框架</h3>
<p><img src="figures/ASoC-SUNXI-驱动框架.png" alt="ASoC SUNXI 驱动框架"></p>
<h3 id="-asoc-sunxi-">基于 ASoC 的 sunxi 代码结构</h3>
<p><img src="figures/ASoC-SUNXI-代码结构.png" alt="ASoC SUNXI 代码结构"></p>
<h2 id="-">源码结构介绍</h2>
<h3 id="-">驱动源码</h3>
<pre><code class="lang-bash">.
├── Kconfig
├── Makefile
├── platforms
│   ├── snd_{CHIP}_i2s<span class="hljs-selector-class">.c</span>
│   ├── snd_{CHIP}_dmic<span class="hljs-selector-class">.c</span>
│   └── snd_{CHIP}_owa<span class="hljs-selector-class">.c</span>
├── snd_{CHIP}_codec<span class="hljs-selector-class">.c</span>
├── snd_{CHIP}_codec<span class="hljs-selector-class">.h</span>
├── snd_sunxi_aaudio<span class="hljs-selector-class">.c</span>
├── snd_sunxi_ahub<span class="hljs-selector-class">.c</span>
├── snd_sunxi_ahub_dam<span class="hljs-selector-class">.c</span>
├── snd_sunxi_ahub_dam<span class="hljs-selector-class">.h</span>
├── snd_sunxi_ahub<span class="hljs-selector-class">.h</span>
├── snd_sunxi_codec_edp<span class="hljs-selector-class">.c</span>
├── snd_sunxi_codec_hdmi<span class="hljs-selector-class">.c</span>
├── snd_sunxi_codec_av<span class="hljs-selector-class">.c</span>
├── snd_sunxi_common<span class="hljs-selector-class">.c</span>
├── snd_sunxi_common<span class="hljs-selector-class">.h</span>
├── snd_sunxi_dap<span class="hljs-selector-class">.c</span>
├── snd_sunxi_dap<span class="hljs-selector-class">.h</span>
├── snd_sunxi_dmic<span class="hljs-selector-class">.c</span>
├── snd_sunxi_dmic<span class="hljs-selector-class">.h</span>
├── snd_sunxi_dummy_codec<span class="hljs-selector-class">.c</span>
├── snd_sunxi_i2s<span class="hljs-selector-class">.c</span>
├── snd_sunxi_i2s<span class="hljs-selector-class">.h</span>
├── snd_sunxi_jack_advance<span class="hljs-selector-class">.c</span>
├── snd_sunxi_jack<span class="hljs-selector-class">.c</span>
├── snd_sunxi_jack_codec<span class="hljs-selector-class">.c</span>
├── snd_sunxi_jack_extcon<span class="hljs-selector-class">.c</span>
├── snd_sunxi_jack_gpio<span class="hljs-selector-class">.c</span>
├── snd_sunxi_jack<span class="hljs-selector-class">.h</span>
├── snd_sunxi_log<span class="hljs-selector-class">.h</span>
├── snd_sunxi_mach<span class="hljs-selector-class">.c</span>
├── snd_sunxi_mach_utils<span class="hljs-selector-class">.c</span>
├── snd_sunxi_mach_utils<span class="hljs-selector-class">.h</span>
├── snd_sunxi_owa<span class="hljs-selector-class">.c</span>
├── snd_sunxi_owa<span class="hljs-selector-class">.h</span>
├── snd_sunxi_owa_rx61937<span class="hljs-selector-class">.c</span>
├── snd_sunxi_owa_rx61937<span class="hljs-selector-class">.h</span>
├── snd_sunxi_pcm<span class="hljs-selector-class">.c</span>
├── snd_sunxi_pcm<span class="hljs-selector-class">.h</span>
├── snd_sunxi_rxsync<span class="hljs-selector-class">.c</span>
├── snd_sunxi_rxsync<span class="hljs-selector-class">.h</span>
├── snd_sunxi_sfx<span class="hljs-selector-class">.c</span>
└── snd_sunxi_sfx.h
</code></pre>
<h3 id="-">源码说明</h3>
<h4 id="platform-">platform 层 --&gt; 公共部分</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_pcm</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_pcm</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责音频流传输，使用 DMA 方式，提供注册 platform 设备的公共函数。</p>
<pre><code>snd_sunxi_hdmi<span class="hljs-selector-class">.c</span>
snd_sunxi_hdmi<span class="hljs-selector-class">.h</span>
snd_sunxi_pcm.h
</code></pre><p>负责音频流传输，使用 DMA 结合 HDMI audio 方式，提供注册 platform 设备的公共函数。</p>
<h4 id="platform-audiocodec">platform 层 --&gt; AudioCodec</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_aaudio</span><span class="hljs-selector-class">.c</span>
</code></pre><p>负责 AudioCodec 模块 DMA 相关配置。</p>
<h4 id="platform-i2s-pcm">platform 层 --&gt; I2S/PCM</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_i2s</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_i2s</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责 I2S/PCM 模块硬件参数、DMA 相关配置。</p>
<h4 id="platform-ahub">platform 层 --&gt; AHUB</h4>
<pre><code>snd_sunxi_ahub<span class="hljs-selector-class">.c</span>
snd_sunxi_ahub<span class="hljs-selector-class">.h</span>
snd_sunxi_ahub_dam<span class="hljs-selector-class">.c</span>
snd_sunxi_ahub_dam.h
</code></pre><p>负责 AHUB 模块硬件参数、DMA 相关配置。</p>
<h4 id="platform-owa">platform 层 --&gt; OWA</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_owa</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_owa</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责 OWA 模块硬件参数、DMA 相关配置。</p>
<h4 id="platform-dmic">platform 层 --&gt; DMIC</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_dmic</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_dmic</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责 DMIC 模块硬件参数、DMA 相关配置。</p>
<h4 id="codec-">codec 层 --&gt; 公共部分</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_common</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_common</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责 AudioCodec 模块公共功能配置。</p>
<ul>
<li>外部功放控制</li>
</ul>
<h4 id="codec-audiocodec">codec 层 --&gt; AudioCodec</h4>
<pre><code><span class="xml">snd_</span><span class="hljs-template-variable">{CHIP}</span><span class="xml">_codec.c
snd_</span><span class="hljs-template-variable">{CHIP}</span><span class="xml">_codec.h</span>
</code></pre><p>负责 {CHIP} 平台AudioCodec 模块硬件参数配置。</p>
<h4 id="machine-">machine 层</h4>
<pre><code>snd_sunxi_mach<span class="hljs-selector-class">.c</span>
snd_sunxi_mach<span class="hljs-selector-class">.h</span>
snd_sunxi_mach_utils<span class="hljs-selector-class">.c</span>
snd_sunxi_mach_utils.h
</code></pre><p>负责 platform 层和 codec 层绑定。</p>
<h4 id="-">特殊功能组件</h4>
<pre><code><span class="hljs-selector-tag">snd_sunxi_rxsync</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_rxsync</span><span class="hljs-selector-class">.h</span>
</code></pre><p>负责多声卡同步录音。</p>
<pre><code><span class="hljs-selector-tag">snd_sunxi_jack_codec</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_jack</span><span class="hljs-selector-class">.h</span>
</code></pre><p>复杂内置 AudioCodec 耳机插拔和耳机按键检测。</p>
<pre><code><span class="hljs-selector-tag">snd_sunxi_jack_extcon</span><span class="hljs-selector-class">.c</span>
<span class="hljs-selector-tag">snd_sunxi_jack</span><span class="hljs-selector-class">.h</span>
</code></pre><p>复杂 extcon 事件耳机插拔和耳机按键检测。</p>
<p>::: note</p>
<p>暂仅支持 typec 接口模拟耳机。</p>
<p>:::</p>
<h4 id="-">平台基础资源</h4>
<pre><code><span class="xml">platforms/snd_</span><span class="hljs-template-variable">{CHIP}</span><span class="xml">_i2s.h     --&gt; </span><span class="hljs-template-variable">{CHIP}</span><span class="xml">平台I2S资源配置。
platforms/snd_</span><span class="hljs-template-variable">{CHIP}</span><span class="xml">_dmic.h    --&gt; </span><span class="hljs-template-variable">{CHIP}</span><span class="xml">平台DMIC资源配置。
platforms/snd_</span><span class="hljs-template-variable">{CHIP}</span><span class="xml">_owa.h     --&gt; </span><span class="hljs-template-variable">{CHIP}</span><span class="xml">平台OWA资源配置。</span>
</code></pre><h2 id="-">关键数据结构</h2>
<blockquote>
<p>仅说明自定义相关结构体和全局变量，alsa 框架内部结构体不做说明。</p>
</blockquote>
<h3 id="pcm-">pcm 数据类结构体</h3>
<p><strong>sunxi_dma_params</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">sunxi_dma_params</span> </span>{
    ···
};
</code></pre>
<p>定义 audio dai dma 相关参数。</p>
<h3 id="platform-">platform 类结构体</h3>
<p><strong>sunxi_i2s</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">sunxi_i2s</span> </span>{
    ···
};
</code></pre>
<p>I2S/PCM 模块总结构体，包含基础平台资源、特定功能私有参数。</p>
<p><strong>sunxi_owa</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">sunxi_owa</span> </span>{
    ···
};
</code></pre>
<p>OWA 模块总结构体，包含基础平台资源、特定功能私有参数。</p>
<p><strong>sunxi_dmic</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">sunxi_dmic</span> </span>{
    ···
};
</code></pre>
<p>DMIC 模块总结构体，包含基础平台资源、特定功能私有参数。</p>
<h3 id="codec-">codec 类结构体</h3>
<p><strong>sunxi_codec</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">sunxi_codec</span> </span>{
    ···
};
</code></pre>
<p>AudioCodec 模块总结构体，包含基础平台资源、特定功能私有参数。</p>
<h3 id="machine-">machine 类结构体</h3>
<p><strong>asoc_simple_priv</strong></p>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">asoc_simple_priv</span> </span>{
    ···
};
</code></pre>
<p>Machine 总结构体，包含 codec dai、cpu dai、特定功能私有参数。</p>
<h2 id="-">接口说明</h2>
<blockquote>
<p>仅说明自定义软件接口，alsa 框架内部接口不做说明。</p>
</blockquote>
<h3 id="pcm-">pcm 相关接口</h3>
<p><strong>sunxi_pcm_new {linux-4.9~linux-5.4}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_new(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_pcm_runtime</span></span> *rtd)
</code></pre>
<ul>
<li>功能描述：创建 pcm 设备</li>
<li>参数说明：<ul>
<li>rtd: pcm 流信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_construct {linux-5.10~linux-5.15}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_construct(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_pcm_runtime</span></span> *rtd)
</code></pre>
<ul>
<li>功能描述：创建 pcm 设备</li>
<li>参数说明：<ul>
<li>component: platform 层组件</li>
<li>rtd: pcm 流信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_free {linux-4.9~linux-5.4}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">sunxi_pcm_free</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_pcm *pcm)</span></span>
</code></pre>
<ul>
<li>功能描述：释放 pcm 设备</li>
<li>参数说明：<ul>
<li>pcm: pcm 设备</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<p><strong>sunxi_pcm_destruct {linux-5.10 or later}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c">void sunxi_pcm_destruct(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm</span></span> *pcm)
</code></pre>
<ul>
<li>功能描述：释放 pcm 设备</li>
<li>参数说明：<ul>
<li>component: platform 层组件</li>
<li>pcm: pcm 设备</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<p><strong>sunxi_pcm_open</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_open(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream)
</code></pre>
<ul>
<li>功能描述：开启 pcm 设备</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_close</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">sunxi_pcm_close</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream)</span></span>
</code></pre>
<ul>
<li>功能描述：关闭 pcm 设备</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<p><strong>sunxi_pcm_ioctl</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">int</span> <span class="hljs-title">sunxi_pcm_ioctl</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> cmd, <span class="hljs-keyword">void</span> *arg)</span></span>
</code></pre>
<ul>
<li>功能描述：pcm 设备操作</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 操作命令</li>
<li>arg: 命令参数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_hw_params</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_hw_params(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_hw_params</span></span> *params)
</code></pre>
<ul>
<li>功能描述：设置 pcm 设备参数</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>params: pcm 硬件参数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_hw_free</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_hw_free(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream)
</code></pre>
<ul>
<li>功能描述：释放 pcm 设备参数</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_trigger</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_trigger(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-keyword">int</span> cmd)
</code></pre>
<ul>
<li>功能描述：触发 pcm 设备运行</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 触发命令</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_pointer</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">snd_pcm_uframes_t</span> sunxi_pcm_pointer(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream)
</code></pre>
<ul>
<li>功能描述：获取 pcm 设备帧点</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：当前 DMA 缓冲指针</li>
</ul>
<p><strong>sunxi_pcm_hw_params_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_hw_params_raw(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_hw_params</span></span> *params)
</code></pre>
<ul>
<li>功能描述：设置 pcm 设备参数(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>params: pcm 硬件参数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_hw_free_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_hw_free_raw(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream)
</code></pre>
<ul>
<li>功能描述：释放 pcm 设备参数(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_prepare_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_prepare_raw(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream)
</code></pre>
<ul>
<li>功能描述：触发 pcm 设备运行准备工作(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 触发命令</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_trigger_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_trigger_raw(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-keyword">int</span> cmd)
</code></pre>
<ul>
<li>功能描述：触发 pcm 设备运行(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 触发命令</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_pcm_pointer_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">snd_pcm_uframes_t</span> sunxi_pcm_pointer_raw(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream)
</code></pre>
<ul>
<li>功能描述：获取 pcm 设备帧点(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：当前 DMA 缓冲指针</li>
</ul>
<p><strong>sunxi_pcm_copy_raw</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">snd_pcm_uframes_t</span> sunxi_pcm_copy_raw(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream)
</code></pre>
<ul>
<li>功能描述：音频数据传输和透传数据处理(for HDMI audio)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
</ul>
</li>
<li>返回值：当前 DMA 缓冲指针</li>
</ul>
<p><strong>sunxi_pcm_mmap</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_pcm_mmap(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">vm_area_struct</span></span> *vma)
</code></pre>
<ul>
<li>功能描述：创建 pcm 设备内存映射</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>vma: VMM 内存区域</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<h3 id="platform-">platform 层接口</h3>
<blockquote>
<p>{module} 表示模块名称，有aaudio、i2s、ahub、owa、dmic。</p>
</blockquote>
<p><strong>sunxi_{module}_component_probe</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_component_probe(<span class="hljs-keyword">struct</span> snd_soc_component *component)
</code></pre>
<ul>
<li>功能描述：初始化 I2S/PCM 声卡相关信息（如控件初始化）</li>
<li>参数说明：<ul>
<li>component: platform 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_suspend {linux-4.9}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_suspend(<span class="hljs-keyword">struct</span> snd_soc_dai *dai)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块休眠（保存寄存器、关闭电源、关闭时钟）</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_component_suspend {linux-5.4 or later}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_component_suspend(<span class="hljs-keyword">struct</span> snd_soc_component *component)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块休眠（保存寄存器、关闭电源、关闭时钟）</li>
<li>参数说明：<ul>
<li>component: platform 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_resume {linux-4.9}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_resume(<span class="hljs-keyword">struct</span> snd_soc_dai *dai)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块唤醒（开启电源、开启时钟、恢复寄存器）</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_component_resume {linux-5.4 or later}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_component_resume(<span class="hljs-keyword">struct</span> snd_soc_component *component)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块唤醒（开启电源、开启时钟、恢复寄存器）</li>
<li>参数说明：<ul>
<li>component: platform 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_probe</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_probe(<span class="hljs-keyword">struct</span> snd_soc_dai *dai)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块接口初始化</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_remove</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_remove(<span class="hljs-keyword">struct</span> snd_soc_dai *dai)
</code></pre>
<ul>
<li>功能描述：I2S/PCM 模块接口移除</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_set_pll</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_set_pll(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">int</span> pll_id, <span class="hljs-keyword">int</span> source,
              <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> freq_in, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> freq_out)
</code></pre>
<ul>
<li>功能描述：设置模块 pllclk</li>
<li>参数说明：<ul>
<li>dai: cpu dai信息</li>
<li>pll_id: pll 辅助信息</li>
<li>source: pll 源</li>
<li>freq_in: 输入频率</li>
<li>freq_out: 输出频率</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_set_sysclk</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_set_sysclk(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">int</span> clk_id, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> freq, <span class="hljs-keyword">int</span> dir)
</code></pre>
<ul>
<li>功能描述：设置模块工作时钟</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
<li>clk_id: clk 辅助信息</li>
<li>freq: 时钟频率</li>
<li>dir: 时钟输出方向</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_set_bclk_ratio</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_set_bclk_ratio(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> ratio)
</code></pre>
<ul>
<li>功能描述：设置模块 BCLK 时钟</li>
<li>参数说明：<ul>
<li>dai: cpu dai信息</li>
<li>ratio: BCLK 分频数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_set_fmt</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_set_fmt(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> fmt)
</code></pre>
<ul>
<li>功能描述：设置模块I2S格式</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
<li>fmt: I2S 格式信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_set_tdm_slot</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_set_tdm_slot(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> tx_mask,
                   <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> rx_mask, <span class="hljs-keyword">int</span> slots, <span class="hljs-keyword">int</span> slot_width)
</code></pre>
<ul>
<li>功能描述：设置模块 I2S slot 数和 slot 宽度</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
<li>tx_mask: tx 掩码</li>
<li>rx_mask: rx 掩码</li>
<li>slots: I2S slot 数</li>
<li>slot_width: I2S slot 宽度</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_startup</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{module}_dai_startup(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：设置模块开启工作资源(DMA 参数、组件功能等)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_hw_params</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{module}_dai_hw_params(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
                <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_hw_params</span></span> *params,
                <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：设置模块硬件参数</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>params: 硬件参数</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_prepare</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{module}_dai_prepare(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：清除模块 fifo</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_trigger</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_{module}_dai_trigger(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream, <span class="hljs-keyword">int</span> cmd, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：触发模块工作</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 触发命令</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_{module}_dai_shutdown</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">void</span> sunxi_{<span class="hljs-keyword">module</span>}_dai_shutdown(<span class="hljs-keyword">struct</span> snd_pcm_substream *substream, <span class="hljs-keyword">struct</span> snd_soc_dai *dai)
</code></pre>
<ul>
<li>功能描述：设置模块关闭工作资源(组件功能等)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<h3 id="codec-audiocodec">codec 层接口 --&gt; AudioCodec</h3>
<p><strong>sunxi_internal_codec_probe</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_probe(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component)
</code></pre>
<ul>
<li>功能描述：初始化 AudioCodec 声卡相关信息（如控件初始化）</li>
<li>参数说明：<ul>
<li>component: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_remove</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_remove(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component)
</code></pre>
<ul>
<li>功能描述：模块资源释放</li>
<li>参数说明：<ul>
<li>component: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_suspend {linux-4.9}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_suspend(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_codec</span></span> *snd_codec)
</code></pre>
<ul>
<li>功能描述：模块休眠（保存寄存器、关闭电源、关闭时钟）</li>
<li>参数说明：<ul>
<li>snd_codec: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_suspend {linux-5.4~linux-5.15}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_suspend(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component)
</code></pre>
<ul>
<li>功能描述：模块休眠（保存寄存器、关闭电源、关闭时钟）</li>
<li>参数说明：<ul>
<li>component: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_resume {linux-4.9}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_resume(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_codec</span></span> *snd_codec)
</code></pre>
<ul>
<li>功能描述：模块唤醒（开启电源、开启时钟、恢复寄存器）</li>
<li>参数说明：<ul>
<li>snd_codec: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_resume {linux-5.4~linux-5.15}</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_resume(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_component</span></span> *component)
</code></pre>
<ul>
<li>功能描述：模块唤醒（开启电源、开启时钟、恢复寄存器）</li>
<li>参数说明：<ul>
<li>component: codec 层组件</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_set_pll</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">int</span> <span class="hljs-title">sunxi_internal_codec_dai_set_pll</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_soc_dai *dai, <span class="hljs-keyword">int</span> pll_id, <span class="hljs-keyword">int</span> source,
                     <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> freq_in, <span class="hljs-keyword">unsigned</span> <span class="hljs-keyword">int</span> freq_out)</span></span>
</code></pre>
<ul>
<li>功能描述：设置模块 pllclk</li>
<li>参数说明：<ul>
<li>dai: cpu dai 信息</li>
<li>pll_id: pll 辅助信息</li>
<li>source: pll 源</li>
<li>freq_in: 输入频率</li>
<li>freq_out: 输出频率</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_startup</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_dai_startup(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
                     <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：设置模块开启工作资源(DMA 参数、组件功能等)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_hw_params</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_dai_hw_params(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
                       <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_hw_params</span></span> *params,
                       <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：设置模块硬件参数</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>params: 硬件参数</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_prepare</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_dai_prepare(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
                     <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：清除模块 fifo</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_trigger</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> sunxi_internal_codec_dai_trigger(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
                     <span class="hljs-keyword">int</span> cmd, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span></span> *dai)
</code></pre>
<ul>
<li>功能描述：触发模块工作</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>cmd: 触发命令</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>sunxi_internal_codec_dai_shutdown</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c">void sunxi_internal_codec_dai_shutdown(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span> *<span class="hljs-title">substream</span>,</span>
                       <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai</span> *<span class="hljs-title">dai</span>)</span>
</code></pre>
<ul>
<li>功能描述：设置模块关闭工作资源(组件功能等)</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>dai: cpu dai 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<h3 id="machine-">machine 层接口</h3>
<p><strong>simple_soc_probe</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> simple_soc_probe(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_card</span></span> *card)
</code></pre>
<ul>
<li>功能描述：初始化声卡相关信息（如 jack）</li>
<li>参数说明：<ul>
<li>card: 声卡</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>simple_soc_remove</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> simple_soc_remove(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_card</span></span> *card)
</code></pre>
<ul>
<li>功能描述：释放声卡相关信息（如 jack）</li>
<li>参数说明：<ul>
<li>card: 声卡</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>simple_dai_link_of</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> simple_dai_link_of(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *node, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">asoc_simple_priv</span></span> *<span class="hljs-keyword">priv</span>)
</code></pre>
<ul>
<li>功能描述：解析 codec 和 platform 节点，并绑定</li>
<li>参数说明：<ul>
<li>node: machine 节点</li>
<li>priv: simple card 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_widgets</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_widgets(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_card</span></span> *card, <span class="hljs-keyword">char</span> *prefix)
</code></pre>
<ul>
<li>功能描述：解析 widget 部件</li>
<li>参数说明：<ul>
<li>card: 声卡</li>
<li>prefix: 设备树属性名称前缀</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_routing</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_routing(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_card</span></span> *card, <span class="hljs-keyword">char</span> *prefix)
</code></pre>
<ul>
<li>功能描述：解析 route 路径</li>
<li>参数说明：<ul>
<li>card: 声卡</li>
<li>prefix: 设备树属性名称前缀</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_pin_switches</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_pin_switches(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_card</span></span> *card, <span class="hljs-keyword">char</span> *prefix)
</code></pre>
<ul>
<li>功能描述：解析dai开关</li>
<li>参数说明：<ul>
<li>card: 声卡</li>
<li>prefix: 设备树属性名称前缀</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_daifmt</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_daifmt(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *node, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *codec,
                 <span class="hljs-keyword">char</span> *prefix, unsigned <span class="hljs-keyword">int</span> *retfmt)
</code></pre>
<ul>
<li>功能描述：解析 I2S 格式</li>
<li>参数说明：<ul>
<li>node: machine 节点</li>
<li>codec: codec 节点</li>
<li>prefix: 设备树属性名称前缀</li>
<li>retfmt: I2S 格式</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_daistream</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_daistream(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *node, <span class="hljs-keyword">char</span> *prefix,
                <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai_link</span></span> *dai_link)
</code></pre>
<ul>
<li>功能描述：解析音频流</li>
<li>参数说明：<ul>
<li>node: machine 节点</li>
<li>prefix: 设备树属性名称前缀</li>
<li>dai_link: dai 链接信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_tdm_slot</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_tdm_slot(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *node, <span class="hljs-keyword">char</span> *prefix,
                   <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">asoc_simple_dai</span></span> *dais)
</code></pre>
<ul>
<li>功能描述：解析 I2S slot 个数和 slot 宽度</li>
<li>参数说明：<ul>
<li>node: machine 节点</li>
<li>prefix: 设备树属性名称前缀</li>
<li>dais: I2S 信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_parse_tdm_clk</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_parse_tdm_clk(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *cpu, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device_node</span></span> *codec,
                  <span class="hljs-keyword">char</span> *prefix, <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">simple_dai_props</span></span> *dai_props)
</code></pre>
<ul>
<li>功能描述：解析I2S clk</li>
<li>参数说明：<ul>
<li>node: cpu 节点</li>
<li>node: codec  节点</li>
<li>prefix: 设备树属性名称前缀</li>
<li>dai_props: dai 参数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_set_dailink_name</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_set_dailink_name(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">device</span></span> *dev,
                 <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_dai_link</span></span> *dai_link,
                 <span class="hljs-keyword">const</span> <span class="hljs-keyword">char</span> *fmt, ...)
</code></pre>
<ul>
<li>功能描述：设置声卡名字</li>
<li>参数说明：<ul>
<li>dai_link: dai 链接信息</li>
<li>fmt: cpu dai 和 codec dai 名</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_dai_init</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_dai_init(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_soc_pcm_runtime</span></span> *rtd)
</code></pre>
<ul>
<li>功能描述：初始化 dai</li>
<li>参数说明：<ul>
<li>rtd: dai 运行信息</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>asoc_simple_hw_params</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> asoc_simple_hw_params(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_substream</span></span> *substream,
              <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_pcm_hw_params</span></span> *params)
</code></pre>
<ul>
<li>功能描述：dai 硬件参数设置</li>
<li>参数说明：<ul>
<li>substream: pcm 子流信息</li>
<li>params: 硬件参数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<h3 id="common-">common 层接口 --&gt; 公共部分</h3>
<p><strong>snd_sunxi_pa_pin_init</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">pa_config</span></span> *snd_sunxi_pa_pin_init(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">platform_device</span></span> *pdev, <span class="hljs-keyword">u32</span> *pa_pin_max)
</code></pre>
<ul>
<li>功能描述：获取并初始化功放引脚</li>
<li>参数说明：<ul>
<li>pa_pin_max: 功放使能引脚个数</li>
</ul>
</li>
<li>返回值：非空-成功，空-失败</li>
</ul>
<p><strong>snd_sunxi_pa_pin_exit</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c">void snd_sunxi_pa_pin_exit(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">platform_device</span></span> *pdev,
               <span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">pa_config</span></span> *pa_cfg,
               <span class="hljs-keyword">u32</span> pa_pin_max)
</code></pre>
<ul>
<li>功能描述：释放功放引脚资源</li>
<li>参数说明：<ul>
<li>pa_cfg: 功放引脚配置</li>
<li>pa_pin_max: 功放使能引脚个数</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<p><strong>snd_sunxi_pa_pin_enable</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c">int snd_sunxi_p<span class="hljs-built_in">a_pin</span>_enable(struct p<span class="hljs-built_in">a_config</span> *p<span class="hljs-built_in">a_cfg</span>, u32 p<span class="hljs-built_in">a_pin</span>_max)
</code></pre>
<ul>
<li>功能描述：使能功放</li>
<li>参数说明：<ul>
<li>pa_cfg: 功放引脚配置</li>
<li>pa_pin_max: 功放使能引脚个数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>snd_sunxi_pa_pin_disable</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c">int snd_sunxi_p<span class="hljs-built_in">a_pin</span>_disable(struct p<span class="hljs-built_in">a_config</span> *p<span class="hljs-built_in">a_cfg</span>, u32 p<span class="hljs-built_in">a_pin</span>_max)
</code></pre>
<ul>
<li>功能描述：失能功放</li>
<li>参数说明：<ul>
<li>pa_cfg: 功放引脚配置</li>
<li>pa_pin_max: 功放使能引脚个数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>snd_sunxi_regulator_init</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_sunxi_rglt</span></span> *snd_sunxi_regulator_init(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">platform_device</span></span> *pdev)
</code></pre>
<ul>
<li>功能描述：电源控制初始化</li>
<li>参数说明：<ul>
<li>pdev: 字符设备</li>
</ul>
</li>
<li>返回值：非空-成功，空-失败</li>
</ul>
<p><strong>snd_sunxi_regulator_exit</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">snd_sunxi_regulator_exit</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_sunxi_rglt *rglt)</span></span>
</code></pre>
<ul>
<li>功能描述：电源控制销毁</li>
<li>参数说明：<ul>
<li>rglt: 电源控制句柄</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<p><strong>snd_sunxi_regulator_enable</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> snd_sunxi_regulator_enable(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_sunxi_rglt</span></span> *rglt)
</code></pre>
<ul>
<li>功能描述：电源控制使能</li>
<li>参数说明：<ul>
<li>rglt: 电源控制句柄</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>snd_sunxi_regulator_disable</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">snd_sunxi_regulator_disable</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_sunxi_rglt *rglt)</span></span>
</code></pre>
<ul>
<li>功能描述：电源控制失能</li>
<li>参数说明：<ul>
<li>rglt: 电源控制句柄</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<h3 id="-">软件调试接口</h3>
<p><strong>snd_sunxi_dump_register</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-keyword">int</span> snd_sunxi_dump_register(<span class="hljs-class"><span class="hljs-keyword">struct</span> <span class="hljs-title">snd_sunxi_dump</span></span> *dump)
</code></pre>
<ul>
<li>功能描述：注册调试</li>
<li>参数说明：<ul>
<li>dump-&gt;dump_version: 版本查看函数</li>
<li>dump-&gt;dump_help: 调试帮助信息显示函数</li>
<li>dump-&gt;dump_show: 调试显示函数</li>
<li>dump-&gt;dump_store: 调试输入函数</li>
</ul>
</li>
<li>返回值：0-成功，其它-失败</li>
</ul>
<p><strong>snd_sunxi_dump_unregister</strong></p>
<ul>
<li>函数原型：</li>
</ul>
<pre><code class="lang-c"><span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">snd_sunxi_dump_unregister</span><span class="hljs-params">(<span class="hljs-keyword">struct</span> snd_sunxi_dump *dump)</span></span>
</code></pre>
<ul>
<li>功能描述：注销调试</li>
<li>参数说明：<ul>
<li>dump: 调试接口函数集</li>
</ul>
</li>
<li>返回值：void</li>
</ul>
<h1 id="-">测试工具介绍</h1>
<h2 id="tinyalsa-tinyalsa-">tinyalsa工具 {#tinyalsa工具}</h2>
<p>tinyalsa主要提供五个工具。</p>
<ol>
<li>tinymix:可以得到音频通路相关的各项配置参数，也可通过传参设置参数；</li>
<li>tinyplay:是一个简易的音乐播放器，一般用于播放测试；</li>
<li>tinycap:是一个简易的录音软件，一般用于录音测试；</li>
<li>tinypcminfo:用于查看 pcm 通道的相关信息；</li>
<li>tinyloop:通过软件的方式，将录制的声音实时播放。</li>
</ol>
<h3 id="tinymix">tinymix</h3>
<pre><code>tinymix -D cardx       <span class="hljs-comment">/* 查看声卡x的控件列表 */</span>
tinymix -D cardx y     <span class="hljs-comment">/* 查看声卡x序号为y的控件的可选值 */</span>
tinymix -D cardx y z   <span class="hljs-comment">/* 设置声卡x序号为y的控件的值为z */</span>
</code></pre><ul>
<li>查看声卡0的控件</li>
</ul>
<pre><code class="lang-bash">/ <span class="hljs-comment"># tinymix -D 0</span>
Mixer name: <span class="hljs-string">'audiocodec'</span>
Number of controls: <span class="hljs-number">8</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       tx hub mode                              Off
<span class="hljs-number">1</span>       INT     <span class="hljs-number">1</span>       digital <span class="hljs-keyword">volume</span><span class="bash">                           63
</span><span class="hljs-number">2</span>       INT     <span class="hljs-number">1</span>       lineout <span class="hljs-keyword">volume</span><span class="bash">                           31
</span>...
</code></pre>
<ul>
<li>查看声卡0的控件 “lineout volume” 可设置范围</li>
</ul>
<pre><code class="lang-bash">/ <span class="hljs-comment"># tinymix -D 0 2</span>
LINEOUT <span class="hljs-keyword">volume</span><span class="bash">: 31 (range 0-&gt;31)</span>
</code></pre>
<ul>
<li>设置声卡0的控件 “lineout volume” 为26</li>
</ul>
<pre><code class="lang-bash">/ # tinymix -D <span class="hljs-number">0</span> <span class="hljs-number">2</span> <span class="hljs-number">26</span>
或
/ # tinymix -D <span class="hljs-number">0</span> “lineout volume” <span class="hljs-number">26</span>
</code></pre>
<h3 id="tinyplay">tinyplay</h3>
<pre><code><span class="hljs-comment">/* 播放 wav 文件，[]选项为可选项，不带则为默认值 */</span>
<span class="hljs-selector-tag">tinyplay</span> <span class="hljs-selector-tag">file</span><span class="hljs-selector-class">.wav</span> <span class="hljs-selector-attr">[-D card]</span> <span class="hljs-selector-attr">[-d device]</span> <span class="hljs-selector-attr">[-p period_size]</span> <span class="hljs-selector-attr">[-n n_periods]</span>
</code></pre><ul>
<li>用声卡0播放 test.wav</li>
</ul>
<pre><code class="lang-bash">/ <span class="hljs-comment"># tinyplay test.wav -D 0</span>
Playing sample:<span class="hljs-number"> 2 </span>ch,<span class="hljs-number"> 48000 </span>hz,<span class="hljs-number"> 16 </span>bit<span class="hljs-number"> 36678428 </span>bytes
</code></pre>
<h3 id="tinycap">tinycap</h3>
<pre><code><span class="hljs-comment">/* 录音并保存数据到 wav 文件，[]选项为可选项，不带则为默认值 */</span>
<span class="hljs-selector-tag">tinycap</span> <span class="hljs-selector-tag">file</span><span class="hljs-selector-class">.wav</span> <span class="hljs-selector-attr">[-D card]</span> <span class="hljs-selector-attr">[-d device]</span> <span class="hljs-selector-attr">[-c channels]</span> <span class="hljs-selector-attr">[-r rate]</span> <span class="hljs-selector-attr">[-d bits]</span> <span class="hljs-selector-attr">[-p period_size]</span> <span class="hljs-selector-attr">[-n n_periods]</span> <span class="hljs-selector-attr">[-T capture time]</span>
</code></pre><ul>
<li>用声卡3录音并将数据保存到 test.wav</li>
</ul>
<pre><code class="lang-bash">/ <span class="hljs-comment"># tinycap test.wav -D 3</span>
Capturing sample:<span class="hljs-number"> 2 </span>ch,<span class="hljs-number"> 44100 </span>hz,<span class="hljs-number"> 16 </span>bit
^CCaptured<span class="hljs-number"> 131072 </span>frames
</code></pre>
<h3 id="tinypcminfo">tinypcminfo</h3>
<pre><code><span class="hljs-comment">/* 查看指定声卡、设备的 pcm 通道信息 */</span>
tinypcminfo -D <span class="hljs-keyword">card</span> -d device
</code></pre><ul>
<li>查看声卡2设备0的 pcm 通道信息</li>
</ul>
<pre><code class="lang-bash">/ # tinypcminfo -D <span class="hljs-number">2</span> -d <span class="hljs-number">0</span>
Info for card <span class="hljs-number">2</span>, device <span class="hljs-number">0</span>:

PCM <span class="hljs-keyword">out</span>:
    <span class="hljs-keyword">Access</span>:   x000009
    <span class="hljs-keyword">Format</span>[<span class="hljs-number">0</span>]:   x000444
    <span class="hljs-keyword">Format</span>[<span class="hljs-number">1</span>]:   <span class="hljs-number">00000000</span>
    ...

PCM <span class="hljs-keyword">in</span>:
    <span class="hljs-keyword">Access</span>:   x000009
    <span class="hljs-keyword">Format</span>[<span class="hljs-number">0</span>]:   x000444
    <span class="hljs-keyword">Format</span>[<span class="hljs-number">1</span>]:   <span class="hljs-number">00000000</span>
    ...
</code></pre>
<h3 id="tinyloop">tinyloop</h3>
<pre><code class="lang-bash"># 用指定的声卡、设备进行录音播放回路测试，<span class="hljs-string">[]</span>选项为可选项，不带为默认值
tinyloop -PD playback card -Pd playback device -CD capture card -Cd capture device
<span class="hljs-string">[-p period_size]</span> <span class="hljs-string">[-n n_periods]</span> <span class="hljs-string">[-c num_channels]</span> <span class="hljs-string">[-r sample_rate]</span>
<span class="hljs-string">[-b format_bit]</span> <span class="hljs-string">[-T playback/capture time]</span>
</code></pre>
<ul>
<li>用声卡0设备0和声卡3设备0进行回路测试</li>
</ul>
<pre><code class="lang-bash">/ <span class="hljs-meta"># tinyloop -PD 0 -Pd 0 -CD 3 -Cd 0</span>
<span class="hljs-symbol">Loopback:</span> Playing device <span class="hljs-number">0</span>, Capture Device <span class="hljs-number">0</span>
<span class="hljs-symbol">Sample:</span> <span class="hljs-number">2</span> ch, <span class="hljs-number">48000</span> hz, <span class="hljs-number">16</span> bit
Duration <span class="hljs-keyword">in</span> <span class="hljs-keyword">sec</span>: forever
</code></pre>
<p>::: note</p>
<p>不同版本 tinyalsa 工具，使用方法可能存在细微差别，可使用以下命令查看其对应版本的具体使用方法。</p>
<p>tinymix -h</p>
<p>tinyplay</p>
<p>tinycap</p>
<p>:::</p>
<h2 id="alsa-utils-">alsa-utils 工具</h2>
<p>alsa-utils 主要提供三个工具：</p>
<ol>
<li>aplay:用于完成与播放相关的操作；</li>
<li>arecord:用于完成与录音相关的操作；</li>
<li>amixer:用于设置相关参数。</li>
</ol>
<h3 id="aplay">aplay</h3>
<pre><code class="lang-bash"># 输入 aplay 或 aplay -h 可打印出使用方法
<span class="hljs-comment">/# aplay</span>
Usage: aplay [OPTION]... [FILE]...

-<span class="ruby">h, --help              help
</span>    -<span class="ruby">-version           print current version
</span>-<span class="ruby">l, --list-devices      list all soundcards <span class="hljs-keyword">and</span> digital audio devices
</span>-<span class="ruby">L, --list-pcms         list device names
</span>-<span class="ruby">D, --device=NAME       select PCM by name
</span>-<span class="ruby">q, --quiet             quiet mode
</span>-<span class="ruby">t, --file-type TYPE    file type (voc, wav, raw <span class="hljs-keyword">or</span> au)
</span>-<span class="ruby">c, --channels=<span class="hljs-comment">#        channels</span>
</span>-<span class="ruby">f, --format=FORMAT     sample format (<span class="hljs-keyword">case</span> insensitive)
</span>-<span class="ruby">r, --rate=<span class="hljs-comment">#            sample rate</span>
</span>-<span class="ruby">d, --duration=<span class="hljs-comment">#        interrupt after # seconds</span>
</span>...
</code></pre>
<ul>
<li>查看可以用于播放的声卡</li>
</ul>
<pre><code class="lang-bash">/<span class="hljs-comment"># aplay -l</span>
**** List of PLAYBACK Hardware Devices ****
card <span class="hljs-number">0</span>: audiocodec [audiocodec], device <span class="hljs-number">0</span>: soc<span class="hljs-variable">@03000000</span><span class="hljs-symbol">:codec_plat-sunxi-snd-codec</span> sunxi-snd-codec-<span class="hljs-number">0</span> []
  <span class="hljs-symbol">Subdevices:</span> <span class="hljs-number">1</span>/<span class="hljs-number">1</span>
  Subdevice <span class="hljs-comment">#0: subdevice #0</span>
card <span class="hljs-number">2</span>: sndi2s<span class="hljs-number">0</span> [sndi2s<span class="hljs-number">0</span>], device <span class="hljs-number">0</span>: <span class="hljs-number">2032000</span>.i2s0_plat-snd-soc-dummy-dai snd-soc-dummy-dai-<span class="hljs-number">0</span> []
  <span class="hljs-symbol">Subdevices:</span> <span class="hljs-number">1</span>/<span class="hljs-number">1</span>
  Subdevice <span class="hljs-comment">#0: subdevice #0</span>
</code></pre>
<ul>
<li>用声卡0设备0播放 test.wav (用 ctrl c 退出)</li>
</ul>
<pre><code class="lang-bash">/# aplay -D hw:<span class="hljs-number">0</span>,<span class="hljs-number">0</span> test.wav
Playing WAVE <span class="hljs-symbol">'test</span>.wav' : <span class="hljs-built_in">Signed</span> <span class="hljs-number">16</span> <span class="hljs-built_in">bit</span> Little Endian, Rate <span class="hljs-number">8000</span> Hz, Mono
^CAborted by <span class="hljs-keyword">signal</span> Interrupt...
</code></pre>
<h3 id="arecord">arecord</h3>
<pre><code class="lang-bash"># 输入 arecord 或 arecord -h 可打印出使用方法
<span class="hljs-comment">/# arecord</span>
Usage: arecord [OPTION]... [FILE]...

-<span class="ruby">h, --help              help
</span>    -<span class="ruby">-version           print current version
</span>-<span class="ruby">l, --list-devices      list all soundcards <span class="hljs-keyword">and</span> digital audio devices
</span>-<span class="ruby">L, --list-pcms         list device names
</span>-<span class="ruby">D, --device=NAME       select PCM by name
</span>-<span class="ruby">q, --quiet             quiet mode
</span>-<span class="ruby">t, --file-type TYPE    file type (voc, wav, raw <span class="hljs-keyword">or</span> au)
</span>-<span class="ruby">c, --channels=<span class="hljs-comment">#        channels</span>
</span>-<span class="ruby">f, --format=FORMAT     sample format (<span class="hljs-keyword">case</span> insensitive)
</span>-<span class="ruby">r, --rate=<span class="hljs-comment">#            sample rate</span>
</span>-<span class="ruby">d, --duration=<span class="hljs-comment">#        interrupt after # seconds</span>
</span>-<span class="ruby">M, --mmap              mmap stream
</span>-<span class="ruby">N, --nonblock          nonblocking mode
</span>-<span class="ruby">F, --period-time=<span class="hljs-comment">#     distance between interrupts is # microseconds</span>
</span>-<span class="ruby">B, --buffer-time=<span class="hljs-comment">#     buffer duration is # microseconds</span>
</span>...
</code></pre>
<ul>
<li>查看可以用于录音的声卡</li>
</ul>
<pre><code class="lang-bash">/<span class="hljs-comment"># arecord -l</span>
**** List of CAPTURE Hardware Devices ****
card <span class="hljs-number">0</span>: audiocodec [audiocodec], device <span class="hljs-number">0</span>: soc<span class="hljs-variable">@03000000</span><span class="hljs-symbol">:codec_plat-sunxi-snd-codec</span> sunxi-snd-codec-<span class="hljs-number">0</span> []
  <span class="hljs-symbol">Subdevices:</span> <span class="hljs-number">1</span>/<span class="hljs-number">1</span>
  Subdevice <span class="hljs-comment">#0: subdevice #0</span>
card <span class="hljs-number">1</span>: snddmic [snddmic], device <span class="hljs-number">0</span>: <span class="hljs-number">2031000</span>.dmic_plat-snd-soc-dummy-dai snd-soc-dummy-dai-<span class="hljs-number">0</span> []
  <span class="hljs-symbol">Subdevices:</span> <span class="hljs-number">1</span>/<span class="hljs-number">1</span>
  Subdevice <span class="hljs-comment">#0: subdevice #0</span>
card <span class="hljs-number">2</span>: sndi2s<span class="hljs-number">0</span> [sndi2s<span class="hljs-number">0</span>], device <span class="hljs-number">0</span>: <span class="hljs-number">2032000</span>.i2s0_plat-snd-soc-dummy-dai snd-soc-dummy-dai-<span class="hljs-number">0</span> []
  <span class="hljs-symbol">Subdevices:</span> <span class="hljs-number">1</span>/<span class="hljs-number">1</span>
  Subdevice <span class="hljs-comment">#0: subdevice #0</span>
...
</code></pre>
<ul>
<li>用声卡1的设备0进行采样位数为16的录音，并把数据保存在 test.wav (用 ctrl c 退出)</li>
</ul>
<pre><code class="lang-bash">/# arecord -D hw:<span class="hljs-number">1</span>,<span class="hljs-number">0</span> -f S16_LE test.wav
Recording WAVE <span class="hljs-symbol">'test</span>.wav' : <span class="hljs-built_in">Signed</span> <span class="hljs-number">16</span> <span class="hljs-built_in">bit</span> Little Endian, Rate <span class="hljs-number">8000</span> Hz, Mono
^CAborted by <span class="hljs-keyword">signal</span> Interrupt...
</code></pre>
<h3 id="amixer">amixer</h3>
<pre><code class="lang-bash"><span class="hljs-comment"># 输入 amixer 或 amixer -h 可打印出使用方法</span>
/<span class="hljs-comment"># amixer -h</span>
Usage: amixer &lt;options&gt; [command]

Available options:
  -h,<span class="hljs-comment">--help       this help</span>
  -c,<span class="hljs-comment">--card N     select the card</span>
  -D,<span class="hljs-comment">--device N   select the device, default 'default'</span>
  -d,<span class="hljs-comment">--debug      debug mode</span>
  -n,<span class="hljs-comment">--nocheck    do not perform range checking</span>
  -v,<span class="hljs-comment">--version    print version of this program</span>
  -q,<span class="hljs-comment">--quiet      be quiet</span>
  -i,<span class="hljs-comment">--inactive   show also inactive controls</span>
  -a,<span class="hljs-comment">--abstract L select abstraction level (none or basic)</span>
  -s,<span class="hljs-comment">--stdin      Read and execute commands from stdin sequentially</span>
  -R,<span class="hljs-comment">--raw-volume Use the raw value (default)</span>
  -M,<span class="hljs-comment">--mapped-volume Use the mapped volume</span>

Available commands:
  scontrols       show all mixer simple controls
  scontents       show <span class="hljs-built_in">contents</span> <span class="hljs-keyword">of</span> all mixer simple controls (default command)
  sset sID P      <span class="hljs-keyword">set</span> <span class="hljs-built_in">contents</span> <span class="hljs-keyword">for</span> one mixer simple control
  sget sID        <span class="hljs-keyword">get</span> <span class="hljs-built_in">contents</span> <span class="hljs-keyword">for</span> one mixer simple control
  controls        show all controls <span class="hljs-keyword">for</span> <span class="hljs-keyword">given</span> card
  <span class="hljs-built_in">contents</span>        show <span class="hljs-built_in">contents</span> <span class="hljs-keyword">of</span> all controls <span class="hljs-keyword">for</span> <span class="hljs-keyword">given</span> card
  cset cID P      <span class="hljs-keyword">set</span> control <span class="hljs-built_in">contents</span> <span class="hljs-keyword">for</span> one control
  cget cID        <span class="hljs-keyword">get</span> control <span class="hljs-built_in">contents</span> <span class="hljs-keyword">for</span> one control
</code></pre>
<ul>
<li>查看声卡1的控件</li>
</ul>
<pre><code class="lang-bash">/# amixer -c <span class="hljs-number">1</span> scontrols
Simple mixer <span class="hljs-section">control</span> 'L0 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'L1 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'L2 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'L3 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'R0 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'R1 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'R2 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'R3 volume',<span class="hljs-number">0</span>
Simple mixer <span class="hljs-section">control</span> 'rx sync mode',<span class="hljs-number">0</span>
</code></pre>
<ul>
<li>查看声卡1的控件的具体配置</li>
</ul>
<pre><code class="lang-bash">/# amixer -<span class="hljs-keyword">c</span> <span class="hljs-number">1</span> scontents
Simple mixer control <span class="hljs-string">'L0 volume'</span>,<span class="hljs-number">0</span>
  Capabilitie<span class="hljs-variable">s:</span> volume volume-joined
  Playback channel<span class="hljs-variable">s:</span> Mono
  Capture channel<span class="hljs-variable">s:</span> Mono
  Limit<span class="hljs-variable">s:</span> <span class="hljs-number">0</span> - <span class="hljs-number">255</span>
  Mono: <span class="hljs-number">176</span> [<span class="hljs-number">69</span>%]
Simple mixer control <span class="hljs-string">'L1 volume'</span>,<span class="hljs-number">0</span>
  Capabilitie<span class="hljs-variable">s:</span> volume volume-joined
  Playback channel<span class="hljs-variable">s:</span> Mono
  Capture channel<span class="hljs-variable">s:</span> Mono
  Limit<span class="hljs-variable">s:</span> <span class="hljs-number">0</span> - <span class="hljs-number">255</span>
  Mono: <span class="hljs-number">176</span> [<span class="hljs-number">69</span>%]
Simple mixer control <span class="hljs-string">'L2 volume'</span>,<span class="hljs-number">0</span>
  Capabilitie<span class="hljs-variable">s:</span> volume volume-joined
  Playback channel<span class="hljs-variable">s:</span> Mono
  Capture channel<span class="hljs-variable">s:</span> Mono
  Limit<span class="hljs-variable">s:</span> <span class="hljs-number">0</span> - <span class="hljs-number">255</span>
  Mono: <span class="hljs-number">176</span> [<span class="hljs-number">69</span>%]
...
</code></pre>
<ul>
<li>设置声卡1第一个控件的值</li>
</ul>
<pre><code class="lang-bash"># 拿到声卡<span class="hljs-number">1</span>所有控件
/# amixer -c <span class="hljs-number">1</span> controls
numid=<span class="hljs-number">2</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L0 volume'</span>
numid=<span class="hljs-number">4</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L1 volume'</span>
numid=<span class="hljs-number">6</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L2 volume'</span>
numid=<span class="hljs-number">8</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L3 volume'</span>
numid=<span class="hljs-number">3</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'R0 volume'</span>
numid=<span class="hljs-number">5</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'R1 volume'</span>
numid=<span class="hljs-number">7</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'R2 volume'</span>
numid=<span class="hljs-number">9</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'R3 volume'</span>
numid=<span class="hljs-number">1</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'rx sync mode'</span>

# 拿到控件内容
/# amixer cget numid=<span class="hljs-number">2</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L0 volume'</span>
numid=<span class="hljs-number">2</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'rx sync mode'</span>
  ; <span class="hljs-keyword">type</span>=ENUMERATED,access=rw------,values=<span class="hljs-number">1</span>,items=<span class="hljs-number">2</span>
  ; Item <span class="hljs-string">#0</span> <span class="hljs-string">'Off'</span>
  ; Item <span class="hljs-string">#1</span> <span class="hljs-string">'On'</span>
  : values=<span class="hljs-number">0</span>

# 设置控件值
/# amixer cset numid=<span class="hljs-number">2</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'L0 volume'</span> <span class="hljs-number">1</span>
numid=<span class="hljs-number">2</span>,iface=MIXER,<span class="hljs-keyword">name</span>=<span class="hljs-string">'rx sync mode'</span>
  ; <span class="hljs-keyword">type</span>=ENUMERATED,access=rw------,values=<span class="hljs-number">1</span>,items=<span class="hljs-number">2</span>
  ; Item <span class="hljs-string">#0</span> <span class="hljs-string">'Off'</span>
  ; Item <span class="hljs-string">#1</span> <span class="hljs-string">'On'</span>
  : values=<span class="hljs-number">1</span>
</code></pre>
<h1 id="faq">FAQ</h1>
<h2 id="-">调试方法</h2>
<h3 id="-">查看所有声卡</h3>
<p>可输入命令<strong>cat /proc/asound/cards</strong>查看系统挂载上的声卡.</p>
<pre><code class="lang-bash">cat /<span class="hljs-keyword">proc</span>/asound/cards

0 [audiocodec ]:<span class="hljs-title"> audiocodec</span> -<span class="hljs-title"> audiocodec</span>
<span class="hljs-title">                   audiocodec</span>
1 [sndowa     ]:<span class="hljs-title"> sndowa</span> -<span class="hljs-title"> sndowa</span>
<span class="hljs-title">                   sndowa</span>
2 [snddmic    ]:<span class="hljs-title"> snddmic</span> -<span class="hljs-title"> snddmic</span>
<span class="hljs-title">                   snddmic</span>
3 [sndi2s0    ]:<span class="hljs-title"> sndi2s0</span> -<span class="hljs-title"> sndi2s0</span>
<span class="hljs-title">                   sndi2s0</span>
4 [sndhdmi    ]:<span class="hljs-title"> sndhdmi</span> -<span class="hljs-title"> sndhdmi</span>
<span class="hljs-title">                   sndhdmi</span>
5 [sndi2s2    ]:<span class="hljs-title"> sndi2s2</span> -<span class="hljs-title"> sndi2s2</span>
<span class="hljs-title">                   sndi2s2</span>
6 [ahubdam    ]:<span class="hljs-title"> ahubdam</span> -<span class="hljs-title"> ahubdam</span>
<span class="hljs-title">                   ahubdam</span>
7 [ahubi2s0   ]:<span class="hljs-title"> ahubi2s0</span> -<span class="hljs-title"> ahubi2s0</span>
<span class="hljs-title">                   ahubi2s0</span>
8 [ahubhdmi   ]:<span class="hljs-title"> ahubhdmi</span> -<span class="hljs-title"> ahubhdmi</span>
<span class="hljs-title">                   ahubhdmi</span>
9 [ahubi2s2   ]:<span class="hljs-title"> ahubi2s2</span> -<span class="hljs-title"> ahubi2s2</span>
<span class="hljs-title">                   ahubi2s2</span>
</code></pre>
<p>::: tip</p>
<p>可通过修改 <strong>“soundcard-mach,name”</strong> 属性，设定声卡名称。</p>
<p>:::</p>
<p>::: note</p>
<ol>
<li>该声卡列表仅为示范作用，部分声卡并非平台共有，取决于芯片规格；</li>
<li>sndi2s0 和 ahubi2s0 均为 i2s 接口，ahub 前缀代表该声卡具备混音功能。</li>
</ol>
<p>:::</p>
<h3 id="-">查看声卡的具体信息</h3>
<pre><code class="lang-bash"><span class="hljs-meta"># 查看声卡号</span>
cat <span class="hljs-meta-keyword">/proc/</span>asound<span class="hljs-meta-keyword">/audiocodec/</span>id
audiocodec

<span class="hljs-meta"># 查看播放设备信息</span>
cat <span class="hljs-meta-keyword">/proc/</span>asound<span class="hljs-meta-keyword">/audiocodec/</span>pcm0p/info
<span class="hljs-symbol">card:</span> <span class="hljs-number">0</span>                    <span class="hljs-meta"># 声卡0</span>
<span class="hljs-symbol">device:</span> <span class="hljs-number">0</span>                <span class="hljs-meta"># 设备0</span>
<span class="hljs-symbol">stream:</span> PLAYBACK        <span class="hljs-meta"># 播放流</span>
...

<span class="hljs-meta"># 查看录音设备信息</span>
cat <span class="hljs-meta-keyword">/proc/</span>asound<span class="hljs-meta-keyword">/audiocodec/</span>pcm0c/info
<span class="hljs-symbol">card:</span> <span class="hljs-number">0</span>
<span class="hljs-symbol">device:</span> <span class="hljs-number">0</span>
<span class="hljs-symbol">stream:</span> CAPTURE            <span class="hljs-meta"># 录音流</span>
...
</code></pre>
<h3 id="-">查看播放设备参数</h3>
<h4 id="-">播放设备的硬件参数</h4>
<pre><code class="lang-bash">cat <span class="hljs-meta-keyword">/proc/</span>asound<span class="hljs-meta-keyword">/card0/</span>pcm0p<span class="hljs-meta-keyword">/sub0/</span>hw_params
<span class="hljs-symbol">access:</span> RW_INTERLEAVED
<span class="hljs-symbol">format:</span> S16_LE            <span class="hljs-meta"># 位深</span>
<span class="hljs-symbol">subformat:</span> STD
<span class="hljs-symbol">channels:</span> <span class="hljs-number">1</span>                <span class="hljs-meta"># 通道数</span>
<span class="hljs-symbol">rate:</span> <span class="hljs-number">48000</span> (<span class="hljs-number">48000</span>/<span class="hljs-number">1</span>)    <span class="hljs-meta"># 采样率</span>
<span class="hljs-symbol">period_size:</span> <span class="hljs-number">1024</span>
<span class="hljs-symbol">buffer_size:</span> <span class="hljs-number">4096</span>
</code></pre>
<h4 id="-">播放设备的软件参数</h4>
<pre><code class="lang-bash">cat <span class="hljs-meta-keyword">/proc/</span>asound<span class="hljs-meta-keyword">/card0/</span>pcm0p<span class="hljs-meta-keyword">/sub0/</span>sw_params
<span class="hljs-symbol">tstamp_mode:</span> ENABLE
<span class="hljs-symbol">period_step:</span> <span class="hljs-number">1</span>
<span class="hljs-symbol">avail_min:</span> <span class="hljs-number">1</span>
<span class="hljs-symbol">start_threshold:</span> <span class="hljs-number">2048</span>
<span class="hljs-symbol">stop_threshold:</span> <span class="hljs-number">4096</span>
<span class="hljs-symbol">silence_threshold:</span> <span class="hljs-number">0</span>
<span class="hljs-symbol">silence_size:</span> <span class="hljs-number">0</span>
<span class="hljs-symbol">boundary:</span> <span class="hljs-number">4611686018427387904</span>
</code></pre>
<h4 id="-">播放设备的状态</h4>
<pre><code class="lang-bash">cat <span class="hljs-regexp">/proc/</span>asound<span class="hljs-regexp">/card0/</span>pcm0p<span class="hljs-regexp">/sub0/</span>status
<span class="hljs-string">state:</span> RUNNING
<span class="hljs-string">owner_pid   :</span> <span class="hljs-number">29385</span>
<span class="hljs-string">trigger_time:</span> <span class="hljs-number">1477225.134078038</span>
<span class="hljs-string">tstamp      :</span> <span class="hljs-number">1477265.465387349</span>
<span class="hljs-string">delay       :</span> <span class="hljs-number">3520</span>
<span class="hljs-string">avail       :</span> <span class="hljs-number">576</span>
<span class="hljs-string">avail_max   :</span> <span class="hljs-number">1024</span>
-----
<span class="hljs-string">hw_ptr      :</span> <span class="hljs-number">1935936</span>
<span class="hljs-string">appl_ptr    :</span> <span class="hljs-number">1939456</span>
</code></pre>
<p>::: note</p>
<p>录音设备的信息查看方法类似。</p>
<p>:::</p>
<h3 id="-">调试节点</h3>
<p><strong>开启调试配置</strong></p>
<pre><code>D<span class="hljs-function"><span class="hljs-title">evice</span> Drivers  ---&gt;</span>
    &lt;*&gt; S<span class="hljs-function"><span class="hljs-title">ound</span> card support  ---&gt;</span>
        &lt;*&gt; A<span class="hljs-function"><span class="hljs-title">dvanced</span> Linux Sound Architecture  ---&gt;</span>
            &lt;*&gt; ALSA <span class="hljs-function"><span class="hljs-title">for</span> SoC audio support  ---&gt;</span>
                A<span class="hljs-function"><span class="hljs-title">llwinner</span> SoC Audio support V2  ---&gt;</span>
                    &lt;M&gt; Allwinner Function Components
                    &lt;M&gt;   Components Debug
</code></pre><p>将 &quot;Components Debug&quot; 选为 Y 或 M，使能调试节点加载。</p>
<p><strong>进入调试节点目录</strong></p>
<pre><code class="lang-bash">cd /sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span></span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> <span class="hljs-comment"># ls</span></span>
dump     help     <span class="hljs-class"><span class="hljs-keyword">module</span>   <span class="hljs-title">version</span></span>
</code></pre>
<ul>
<li>version: 查看所使用的各个音频模块版本；</li>
<li>module: 模块查看和模块选择节点；</li>
<li>help: 当前模块使用帮助信息；</li>
<li>dump: 模块操作节点。</li>
</ul>
<p><strong>使用说明</strong></p>
<ol>
<li>通过 cat module 确认可用模块，通过 echo {module name} &gt; module 选择模块；</li>
<li>通过 cat help 查看当前模块使用方式；</li>
<li>根据使用帮助信息操作dump节点。</li>
</ol>
<p><strong>示例1：读写AudioCodec模块寄存器</strong></p>
<pre><code class="lang-bash"># 查看可使用模块
/sys/class/snd_sunxi # <span class="hljs-keyword">cat</span> module
optional module<span class="hljs-variable">s:</span>
<span class="hljs-number">1</span>. AudioCodec
<span class="hljs-number">2</span>. I2S0
<span class="hljs-number">3</span>. machine-sndi2s0
current module(NULL)

# 选择AudioCodec模块
/sys/class/snd_sunxi # <span class="hljs-keyword">echo</span> AudioCodec &gt; module

# 查看当前模块帮助信息
/sys/class/snd_sunxi # <span class="hljs-keyword">cat</span> <span class="hljs-keyword">help</span>
== module <span class="hljs-keyword">help</span> ==
<span class="hljs-number">1</span>. <span class="hljs-built_in">get</span> optional module<span class="hljs-variable">s:</span> <span class="hljs-keyword">cat</span> module
<span class="hljs-number">2</span>. <span class="hljs-keyword">set</span> current module  : <span class="hljs-keyword">echo</span> {module name} &gt; module
== current module(AudioCodec) <span class="hljs-keyword">help</span> ==
<span class="hljs-number">1</span>. <span class="hljs-keyword">reg</span> <span class="hljs-keyword">read</span> : <span class="hljs-keyword">echo</span> {num} &gt; dump &amp;&amp; <span class="hljs-keyword">cat</span> dump
num: <span class="hljs-number">0</span>(<span class="hljs-keyword">all</span>)
<span class="hljs-number">2</span>. <span class="hljs-keyword">reg</span> <span class="hljs-keyword">write</span>: <span class="hljs-keyword">echo</span> {<span class="hljs-keyword">reg</span>} {value} &gt; dump
eg. <span class="hljs-keyword">echo</span> <span class="hljs-number">0</span>x00 <span class="hljs-number">0</span>xaa &gt; dump

# 根据节点提示说明查看相应寄存器值，如下
/sys/class/snd_sunxi # <span class="hljs-keyword">echo</span> <span class="hljs-number">0</span> &gt; dump &amp;&amp; <span class="hljs-keyword">cat</span> dump
module(AudioCodec)
[<span class="hljs-number">0</span>x000]: <span class="hljs-number">0</span><span class="hljs-keyword">x</span>     <span class="hljs-number">100</span>
[<span class="hljs-number">0</span>x004]: <span class="hljs-number">0</span><span class="hljs-keyword">x</span>   <span class="hljs-number">1</span>a0a0
[<span class="hljs-number">0</span>x010]: <span class="hljs-number">0</span><span class="hljs-keyword">x</span>    <span class="hljs-number">4000</span>
...

# 根据节点提示说明设置相应寄存器值，如下
/sys/class/snd_sunxi # <span class="hljs-keyword">echo</span> <span class="hljs-number">0</span>x4 <span class="hljs-number">0</span>x1a0a1 &gt; dump
<span class="hljs-keyword">reg</span>[<span class="hljs-number">0</span>x004]: <span class="hljs-number">0</span>x1a0a0 (old)
<span class="hljs-keyword">reg</span>[<span class="hljs-number">0</span>x004]: <span class="hljs-number">0</span>x1a0a1 (<span class="hljs-keyword">new</span>)
</code></pre>
<p><strong>示例2：修改I2S格式</strong></p>
<blockquote>
<p>machine-sndi2s{n} 调试节点可修改I2S格式。</p>
<p>注：修改I2S格式，需要重新播放或录音，播放或录音过程中，I2S格式修改不会立即生效。</p>
</blockquote>
<pre><code class="lang-bash"><span class="hljs-comment"># 查看可使用模块</span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">cat</span> <span class="hljs-title">module</span></span>
optional modules:
<span class="hljs-number">1.</span> AudioCodec
<span class="hljs-number">2.</span> I2S0
<span class="hljs-number">3.</span> machine-sndi2s0
current <span class="hljs-built_in">module</span>(AudioCodec)

<span class="hljs-comment"># 选择machine-sndi2s0模块</span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">echo</span> <span class="hljs-title">machine-sndi2s0</span> &gt; <span class="hljs-title">module</span></span>

<span class="hljs-comment"># 查看当前模块帮助信息</span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">cat</span> <span class="hljs-title">help</span></span>
== <span class="hljs-built_in">module</span> help ==
<span class="hljs-number">1.</span> get optional modules: cat <span class="hljs-built_in">module</span>
<span class="hljs-number">2.</span> set current <span class="hljs-built_in">module</span>  : echo {<span class="hljs-built_in">module</span> name} &gt; <span class="hljs-built_in">module</span>
== current <span class="hljs-built_in">module</span>(machine-sndi2s0) help ==
<span class="hljs-number">1.</span> get daifmt: echo {num} &gt; dump &amp;&amp; cat dump
num: <span class="hljs-number">0</span>(all)
<span class="hljs-number">2.</span> set daifmt: echo {Opt num} {Val} &gt; dump
Opt num             Val                                 note
<span class="hljs-number">1</span> CPUPLL FS<span class="hljs-function">      -&gt;</span> <span class="hljs-number">1</span>~n<span class="hljs-function">                              -&gt;</span> <span class="hljs-number">22.5792</span> <span class="hljs-keyword">or</span> <span class="hljs-number">24.576</span> * fs MHz
<span class="hljs-number">2</span> CODECPLL FS<span class="hljs-function">    -&gt;</span> <span class="hljs-number">1</span>~n<span class="hljs-function">                              -&gt;</span> <span class="hljs-number">22.5792</span> <span class="hljs-keyword">or</span> <span class="hljs-number">24.576</span> * fs MHz
<span class="hljs-number">3</span> MCLK FP<span class="hljs-function">        -&gt;</span> Off On<span class="hljs-function">                           -&gt;</span> mclk fs flag
<span class="hljs-number">4</span> MCLK FS<span class="hljs-function">        -&gt;</span> <span class="hljs-number">0</span>~n<span class="hljs-function">                              -&gt;</span> pcm rate * fs(fp Off), (<span class="hljs-number">11.2896</span> <span class="hljs-keyword">or</span> <span class="hljs-number">12.288</span>)MHz * fs(fp On)
<span class="hljs-number">5</span> FMT<span class="hljs-function">            -&gt;</span> i2s right_j left_j dsp_a dsp_b<span class="hljs-function">   -&gt;</span> i2s/pcm format config
<span class="hljs-number">6</span> MASTER<span class="hljs-function">         -&gt;</span> CBM_CFM CBS_CFM CBM_CFS CBS_CFS<span class="hljs-function">  -&gt;</span> bclk&amp;lrck master
<span class="hljs-number">7</span> INVERT<span class="hljs-function">         -&gt;</span> NB_NF NB_IF IB_NF IB_IF<span class="hljs-function">          -&gt;</span> bclk&amp;lrck invert
<span class="hljs-number">8</span> SLOTS<span class="hljs-function">          -&gt;</span> <span class="hljs-number">2</span>~<span class="hljs-number">32</span> <span class="hljs-function"><span class="hljs-params">(must be <span class="hljs-number">2</span>*n)</span>               -&gt;</span> slot number
<span class="hljs-number">9</span> SLOT WIDTH<span class="hljs-function">     -&gt;</span> <span class="hljs-number">16</span> <span class="hljs-number">24</span> <span class="hljs-number">32</span><span class="hljs-function">                         -&gt;</span> slot width
<span class="hljs-number">10</span> ML SEL<span class="hljs-function">        -&gt;</span> RM_TM RM_TL RL_TM RL_TL<span class="hljs-function">          -&gt;</span> RX&amp;TX MSB/LSB first select
<span class="hljs-number">11</span> DATA LATE<span class="hljs-function">     -&gt;</span> <span class="hljs-number">0</span>~<span class="hljs-number">3</span><span class="hljs-function">                              -&gt;</span> data <span class="hljs-keyword">is</span> offset <span class="hljs-keyword">by</span> n BCLKS <span class="hljs-keyword">to</span> LRCK

<span class="hljs-comment"># 根据节点提示说明查看当前I2S格式，如下</span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">echo</span> 0 &gt; <span class="hljs-title">dump</span> &amp;&amp; <span class="hljs-title">cat</span> <span class="hljs-title">dump</span></span>
<span class="hljs-built_in">module</span>(machine-sndi2s0)
CPUPLL FS<span class="hljs-function">   -&gt;</span> <span class="hljs-number">1</span>
CODECPLL FS<span class="hljs-function"> -&gt;</span> <span class="hljs-number">1</span>
MCLK FP<span class="hljs-function">     -&gt;</span> On
MCLK FS<span class="hljs-function">     -&gt;</span> <span class="hljs-number">1</span>
FMT<span class="hljs-function">         -&gt;</span> i2s
MASTER<span class="hljs-function">      -&gt;</span> CBS_CFS
INVERT<span class="hljs-function">      -&gt;</span> NB_NF
SLOTS<span class="hljs-function">       -&gt;</span> <span class="hljs-number">2</span>
SLOT WIDTH<span class="hljs-function">  -&gt;</span> <span class="hljs-number">32</span>
ML SEL<span class="hljs-function">      -&gt;</span> RM_TM
DATA LATE<span class="hljs-function">   -&gt;</span> <span class="hljs-number">1</span>

<span class="hljs-comment"># 根据节点提示说明设置I2S格式，如下</span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">echo</span> 8 4 &gt; <span class="hljs-title">dump</span></span>
/sys/<span class="hljs-class"><span class="hljs-keyword">class</span>/<span class="hljs-title">snd_sunxi</span> # <span class="hljs-title">echo</span> 0 &gt; <span class="hljs-title">dump</span> &amp;&amp; <span class="hljs-title">cat</span> <span class="hljs-title">dump</span></span>
<span class="hljs-built_in">module</span>(machine-sndi2s0)
CPUPLL FS<span class="hljs-function">   -&gt;</span> <span class="hljs-number">1</span>
CODECPLL FS<span class="hljs-function"> -&gt;</span> <span class="hljs-number">1</span>
MCLK FP<span class="hljs-function">     -&gt;</span> On
MCLK FS<span class="hljs-function">     -&gt;</span> <span class="hljs-number">1</span>
FMT<span class="hljs-function">         -&gt;</span> i2s
MASTER<span class="hljs-function">      -&gt;</span> CBS_CFS
INVERT<span class="hljs-function">      -&gt;</span> NB_NF
SLOTS<span class="hljs-function">       -&gt;</span> <span class="hljs-number">4</span>        <span class="hljs-comment"># 该值已修改为4</span>
SLOT WIDTH<span class="hljs-function">  -&gt;</span> <span class="hljs-number">32</span>
ML SEL<span class="hljs-function">      -&gt;</span> RM_TM
DATA LATE<span class="hljs-function">   -&gt;</span> <span class="hljs-number">1</span>
</code></pre>
<h2 id="-">常见问题</h2>
<h3 id="-">录音或播放变速</h3>
<p>【分析步骤一】：确认录音和播放采样率和父时钟 PLL_AUDIO 是否属于同一频段。</p>
<p>【分析步骤二】：以上无法定位，请联系FAE协助分析定位。</p>
<h3 id="audiocodec-">AudioCodec 输入输出无声音</h3>
<p>【分析步骤一】：确认通路设置。</p>
<p>通过 tinymix 查看 route 状态，通过 debugfs 查看 DAPM 状态，是否设置了需要的上下电通路。</p>
<p>【分析步骤二】：对于喇叭，确认功放芯片使能设置。</p>
<p>查看设备树 codec 节点中 pa-pin-n 的 gpio 配置和硬件原理图比对，是否适配了对应的 gpio。</p>
<p>【分析步骤三】：以上无法定位，请联系 FAE 协助分析定位。</p>
<h3 id="dmic-">DMIC 录音异常（静音/通道移位）</h3>
<p>【分析步骤一】：确认GPIO是否正常。</p>
<ol>
<li>通过 datasheet 核对 board.dts 部分的 DMIC pin 设置；</li>
<li>通过 sunxi_dump 来打印出 DMIC 的 gpio 设置是否正常（dump 寄存器的时候请在 DMIC 正在录音的时候）。</li>
</ol>
<p>【分析步骤二】：确认 clk 的频率。</p>
<p>以上正常情况下，示波器查看 DMIC clk 的频率是否满足如下关系。</p>
<pre><code><span class="hljs-attr">clk</span> = sample * over_sample_rate
</code></pre><p>【分析步骤三】：排查硬件连接和 DMIC 物料问题。</p>
<p>【分析步骤四】：以上无法定位，请联系 FAE 协助分析定位。</p>
<h3 id="i2s-codec">I2S 外挂 codec</h3>
<p>【分析步骤一】硬件连接</p>
<p>确保外部 codec 芯片与 SOC I2S 接口正确连接，具体确认连接如下。</p>
<ul>
<li>LRCK, BCLK: 确认该两线是否连接；</li>
<li>MCLK: 确认外部 codec 是否需要 MCLK，若需要，则确认 MCLK 信号线连接；</li>
<li>DIN: 确认外部 codec 是否需要录音功能，若需要，则确认 DIN 信号线连接；</li>
<li>DOUT: 确认外部 codec 是否需要播放功能，若需要，则确认 DOUT 信号线连接。</li>
</ul>
<p>【分析步骤二】 获取外部 codec I2S 协议格式</p>
<p>确认外部 codec I2S 协议格式如下。</p>
<ol>
<li>功能需求：只录音、只播放、录音播放；</li>
<li>引脚确认：I2S 序号、data 引脚序号；</li>
<li>主从模式：SOC 做主(由SOC提供BCLK,LRCK)、外挂 codec 做主(由外挂 codec 提供 BCLK, LRCK)；</li>
<li>I2S模式：标准I2S、I2S_L、I2S_R、DSP_A、DSP_B；</li>
<li>LRCK 信号是否翻转；</li>
<li>BCLK 信号是否翻转；</li>
<li>MCLK 信号：MCLK 频率；</li>
<li>slot 个数：最高要支持多少 slot（音频通道数）；</li>
<li>slot 宽度：最高要支持多少 slot 宽度（音频采样位深）；</li>
<li>位编号：选择MSB first或LSB first。</li>
<li>data late模式：数据采样时相对LRCK脉冲距离多少个BCLK脉冲。</li>
</ol>
<p>查看各模块的 <strong>board.dts 板级配置</strong> 配置项说明，根据 I2S 协议格式进行配置。</p>
<h1 id="-">附录</h1>
<h2 id="gpio-gpio-">GPIO 功能复用配置 {#GPIO功能复用配置}</h2>
<ul>
<li>AudioCodec 模块：<ul>
<li>所用引脚功能均固化，无需进行pin功能复用配置。</li>
</ul>
</li>
<li>通用 I2S/PCM、带混音 I2S/PCM、OWA、DMIC 模块：<ul>
<li>可选择不进行pin功能复用配置，该情况仍可生成声卡，但引脚无实际功能输入输出。</li>
</ul>
</li>
</ul>
<pre><code><span class="hljs-variable">&amp;xxx_plat</span> {
    ···
    pinctrl-used;
    pinctrl-names    = <span class="hljs-string">"default"</span>,<span class="hljs-string">"sleep"</span>;
    pinctrl<span class="hljs-number">-0</span>        = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;xxx_pins_a</span>&gt;</span>;
    pinctrl<span class="hljs-number">-1</span>        = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;xxx_pins_b</span>&gt;</span>;
};
</code></pre><p><strong>配置项说明：</strong></p>
<p>Table: GPIO 功能复用配置项</p>
<table>
<thead>
<tr>
<th>配置项名称</th>
<th>配置值范围</th>
<th>配置项说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>pinctrl-used</td>
<td>注释为 false, 反之为 ture</td>
<td>是否使用引脚复用功能。</td>
</tr>
<tr>
<td>pinctrl-names</td>
<td>&quot;default&quot;,&quot;sleep&quot;</td>
<td>对 pinctrl 属性内容进行名称定义，</td>
</tr>
<tr>
<td></td>
<td></td>
<td>用于辅助 pinctrl 属性获取。</td>
</tr>
<tr>
<td>pinctrl-0</td>
<td>模块pin功能复用节点</td>
<td>对应 pinctrl-names 第0个属性。</td>
</tr>
<tr>
<td>pinctrl-1</td>
<td>模块pin功能复用节点</td>
<td>对应 pinctrl-names 第1个属性。</td>
</tr>
</tbody>
</table>
<p>Table: 模块引脚组定义说明(linux-4.9)</p>
<table>
<thead>
<tr>
<th>节点配置</th>
<th>解释说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>allwinner,pins</td>
<td>模块需要使用到的引脚组定义。</td>
</tr>
<tr>
<td>allwinner,function</td>
<td>模块引脚组复用名称。</td>
</tr>
<tr>
<td>allwinner,muxsel</td>
<td>模块引脚组复用类型，需和function对应。</td>
</tr>
<tr>
<td>allwinner,driver</td>
<td>模块引脚驱动力，可选值为 0,1,2,3，默认配置为1即可。</td>
</tr>
<tr>
<td>allwinner,pull</td>
<td>0:关闭上下拉，1:支持上拉，2:支持下拉（默认配置为0）。</td>
</tr>
</tbody>
</table>
<p>Table: 模块引脚组定义说明(linux-5.4, linux-5.10, linux-5.15, linux-6.6)</p>
<table>
<thead>
<tr>
<th>节点配置</th>
<th>解释说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>pins</td>
<td>模块需要使用到的引脚组定义。</td>
</tr>
<tr>
<td>function</td>
<td>模块引脚组复用名称。</td>
</tr>
<tr>
<td>drive-strength</td>
<td>模块引脚驱动力，可选值为 10,20,30,40，默认配置为20即可。</td>
</tr>
<tr>
<td>bias-disable</td>
<td>关闭上下拉（默认选择该项）。</td>
</tr>
<tr>
<td>bias-pull-up</td>
<td>支持上拉（默认关闭）。</td>
</tr>
<tr>
<td>bias-pull-down</td>
<td>支持下拉（默认关闭）。</td>
</tr>
</tbody>
</table>
<h2 id="-">调试指南</h2>
<h3 id="pinctrl">pinctrl</h3>
<h4 id="-1-">典型问题1：引脚复用配置错误导致声卡注册失败</h4>
<p>由于每个平台针对模块引脚定义中的function定义可能不同，或者pinctrl驱动升级时各模块未完成function适配，若pinctrl出现类似如下报错：</p>
<pre><code class="lang-bash"><span class="hljs-attribute">sunxi</span>:pinctrl_sunxi<span class="hljs-variable">@2000000</span>.pinctrl[ERR]: unsupported function i2s0_i on pin PB7
</code></pre>
<p>说明function属性配置错误，需要从对应平台pinctrl驱动中获取准确的function名称，如：a523项目（sun55iw3）配置i2s0模块引脚i2s0_pins_a时，pins属性中使用到了&quot;PB7&quot;，function属性使用&quot;i2s0&quot;，而引入了如上报错继而导致i2s0声卡无法注册，需要从如下路径中获取PB7引脚准确的i2s引脚复用名称：</p>
<blockquote>
<p>原配置</p>
</blockquote>
<pre><code class="lang-bash">...
<span class="hljs-symbol">        i2s0_pins_a:</span> i2s0@<span class="hljs-number">0</span> {
                pins = <span class="hljs-string">"PB4"</span>, <span class="hljs-string">"PB5"</span>, <span class="hljs-string">"PB6"</span>, <span class="hljs-string">"PB7"</span><span class="hljs-comment">;</span>
                function = <span class="hljs-string">"i2s0"</span><span class="hljs-comment">;</span>
                drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
                <span class="hljs-keyword">bias-disable;
</span>        }<span class="hljs-comment">;</span>
<span class="hljs-symbol">        i2s0_pins_b:</span> i2s0@<span class="hljs-number">1</span> {
                pins = <span class="hljs-string">"PB4"</span>, <span class="hljs-string">"PB5"</span>, <span class="hljs-string">"PB6"</span>, <span class="hljs-string">"PB7"</span><span class="hljs-comment">;</span>
                function = <span class="hljs-string">"io_disabled"</span><span class="hljs-comment">;</span>
                drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
                <span class="hljs-keyword">bias-disable;
</span>        }<span class="hljs-comment">;</span>
...
&amp;i2s0_plat {
...
        pinctrl-used<span class="hljs-comment">;</span>
        pinctrl-names   = <span class="hljs-string">"default"</span>,<span class="hljs-string">"sleep"</span><span class="hljs-comment">;</span>
        pinctrl-0       = &lt;&amp;i2s0_pins_a&gt;<span class="hljs-comment">;</span>
        pinctrl-1       = &lt;&amp;i2s0_pins_b&gt;<span class="hljs-comment">;</span>
...
}<span class="hljs-comment">;</span>
</code></pre>
<blockquote>
<p>从驱动中找出PB7准确的i2s引脚复用名称</p>
</blockquote>
<pre><code>longan/bsp/drivers/pinctrl/pinctrl-sun55iw3.c
...
        SUNXI_PIN(SUNXI_PINCTRL_PIN(B, <span class="hljs-number">7</span>),
...
                SUNXI_FUNCTION(<span class="hljs-number">0x3</span>, <span class="hljs-string">"i2s0_dout"</span>),       <span class="hljs-comment">/* i2s0_dout0 */</span>
                SUNXI_FUNCTION(<span class="hljs-number">0x4</span>, <span class="hljs-string">"i2s0_din"</span>),        <span class="hljs-comment">/* i2s0_din1 */</span>
...
</code></pre><blockquote>
<p>该引脚即可复用为DOUT或DIN，若需要使用DOUT功能，可作如下修改</p>
</blockquote>
<pre><code><span class="hljs-symbol">        i2s0_pins_a:</span> i2s0@<span class="hljs-number">0</span> {
                pins = <span class="hljs-string">"PB4"</span>, <span class="hljs-string">"PB5"</span>, <span class="hljs-string">"PB6"</span><span class="hljs-comment">;</span>
                function = <span class="hljs-string">"i2s0"</span><span class="hljs-comment">;</span>
                drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
                <span class="hljs-keyword">bias-disable;
</span>        }<span class="hljs-comment">;</span>
<span class="hljs-symbol">        i2s0_pins_b:</span> i2s0@<span class="hljs-number">1</span> {
                pins = <span class="hljs-string">"PB4"</span>, <span class="hljs-string">"PB5"</span>, <span class="hljs-string">"PB6"</span>, <span class="hljs-string">"PB7"</span><span class="hljs-comment">;</span>
                function = <span class="hljs-string">"io_disabled"</span><span class="hljs-comment">;</span>
                drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
                <span class="hljs-keyword">bias-disable;
</span>        }<span class="hljs-comment">;</span>
<span class="hljs-symbol">        i2s0_pins_c:</span> i2s0@<span class="hljs-number">2</span> {
                pins = <span class="hljs-string">"PB7"</span><span class="hljs-comment">;</span>
                function = <span class="hljs-string">"i2s0_dout"</span><span class="hljs-comment">;</span>
                drive-strength = &lt;<span class="hljs-number">20</span>&gt;<span class="hljs-comment">;</span>
                <span class="hljs-keyword">bias-disable;
</span>        }<span class="hljs-comment">;</span>
...
&amp;i2s0_plat {
...
        pinctrl-used<span class="hljs-comment">;</span>
        pinctrl-names   = <span class="hljs-string">"default"</span>,<span class="hljs-string">"sleep"</span><span class="hljs-comment">;</span>
        pinctrl-0       = &lt;&amp;i2s0_pins_a &amp;i2s0_pins_c&gt;<span class="hljs-comment">;</span>
        pinctrl-1       = &lt;&amp;i2s0_pins_b&gt;<span class="hljs-comment">;</span>
...
</code></pre><h4 id="-2-">典型问题2：多模块引脚占用导致声卡注册失败</h4>
<p>当两个模块同时引用同一个引脚时，会出现引脚占用而导致模块加载异常问题，参考日志：</p>
<pre><code class="lang-bash"><span class="hljs-selector-attr">[    1.055601]</span> <span class="hljs-selector-tag">sun251iw1-pinctrl</span> 2000000<span class="hljs-selector-class">.pinctrl</span>: <span class="hljs-selector-tag">pin</span> <span class="hljs-selector-tag">PF0</span> <span class="hljs-selector-tag">already</span> <span class="hljs-selector-tag">requested</span> <span class="hljs-selector-tag">by</span> 4020000<span class="hljs-selector-class">.sdmmc</span>; <span class="hljs-selector-tag">cannot</span> <span class="hljs-selector-tag">claim</span> <span class="hljs-selector-tag">for</span> 2034000<span class="hljs-selector-class">.i2s2_plat</span>
</code></pre>
<blockquote>
<p>确认引脚占用模块</p>
</blockquote>
<p>1.被占用引脚可能位于pinctrl属性中，如日志中提示PF0被占用，过滤<strong>PF0</strong>;
2.被占用引脚亦可能位于GPIO属性中，如日志中提示PF0被占用，过滤<strong>PF 0</strong>;</p>
<blockquote>
<p>确认占用模块是否作为关键功能</p>
</blockquote>
<p>1.假设占用模块非必须打开或可在测试阶段关闭，则关闭此模块；
2.假设占用模块必须打开，则需硬件改版解决引脚占用问题。</p>
<h4 id="-3-i2s-codec-pop-">典型问题3：I2S启动或关闭时外挂CODEC产生pop音</h4>
<p>产生此问题的原因可能是I2S关闭状态下引脚处于悬空状态导致，可配置引脚为上拉或下拉解决此问题：</p>
<pre><code class="lang-bash"><span class="hljs-attribute">i2s0_pins_a</span>: i2s0<span class="hljs-variable">@0</span> {
    ...
    <span class="hljs-comment">/* bias-disable:关闭时悬空; bias-pull-up:关闭时上拉; bias-pull-down:关闭时下拉 */</span>
    bias-pull-up;
}
</code></pre>
<h3 id="-">耳机检测配置说明</h3>
<h4 id="codec-">codec内置耳机检测</h4>
<p>::: note
以下属性可用于支持圆孔模拟耳机检测场景。
:::</p>
<p><strong>属性1：jack-det-level</strong></p>
<ul>
<li>调试方法1：</li>
</ul>
<p>1.将 <strong>jack-det-level</strong> 值配置为1；
2.使用示波器或万用表测量 <strong>HP-DET</strong> 引脚，预期结果：</p>
<pre><code>插入耳机前：高电平
插入耳机后：低电平
</code></pre><p>3.若电平变化方向与预期相反，将 <strong>jack-det-level</strong> 设置为0。</p>
<ul>
<li>调试方法2：</li>
</ul>
<p>1.异常现象：</p>
<pre><code>插入耳机时打印：`jack report -&gt; OUT`
拔出耳机时打印：`jack report -&gt; HEADSET`
</code></pre><p>解决方案：将 <code>jack-det-level</code> 的当前值取反。</p>
<p><strong>属性2：jack-det-threshold</strong></p>
<p>1.前提条件：audiocodec驱动中打印出&quot;headset_basedata&quot;变量的值；
2.异常现象1：</p>
<pre><code>插入耳机时：headset_basedata != jack-det-threshold
</code></pre><p>解决方案：将 <code>jack-det-threshold</code> 的当前值设置为headset_basedata插入耳机时的值。</p>
<p>3.异常现象2：</p>
<pre><code><span class="hljs-attr">插入耳机时：headset_basedata =</span>=<span class="hljs-string"> 0</span>
</code></pre><p>解决方案：检查HP-DET引脚有没有连接耳机座子，检查耳机检测外围电路。</p>
<p><strong>属性3：jack-det-debounce</strong></p>
<p>1.设置一个较小的延时值（默认 250ms），测试插入/拔出的响应速度;</p>
<pre><code><span class="hljs-comment">/* 默认值 */</span>
<span class="hljs-keyword">jack-det-debounce </span>= &lt;<span class="hljs-number">250</span>&gt;<span class="hljs-comment">;</span>
</code></pre><p>2.反复插拔耳机，观察是否有误触发现象（如插入后立即检测为拔出）;</p>
<pre><code><span class="hljs-comment">/* 使用以下命令查看日志 */</span>
dmesg | <span class="hljs-keyword">grep</span> <span class="hljs-string">"jack report"</span>
</code></pre><p>3.如果出现误触发，逐步增加延时值（如 300ms、350ms），直到误触发消失。</p>
<h4 id="extcon-">extcon耳机检测</h4>
<p>::: note
以下属性可用于支持TYPE-C模拟耳机检测场景。
:::</p>
<p><strong>属性1：extcon</strong></p>
<p>1.从原理图中查看CC引脚所连接的CC logic模块；
2.确认该模块所属驱动是否已支持extcon检测type-c耳机插拔，若未支持或程序未调用该函数，需要PMU模块负责人协助支持；</p>
<pre><code><span class="hljs-comment">/* 相关代码 */</span>
<span class="hljs-keyword">extcon_set_state_sync(chip-&gt;edev, </span><span class="hljs-keyword">EXTCON_JACK_HEADPHONE, </span>true)<span class="hljs-comment">;</span>
</code></pre><p>3.设置属性值为CC logic模块驱动dts节点：</p>
<pre><code class="lang-dts">extcon = <span class="hljs-params">&lt;&amp;{CC logic模块dts节点}&gt;</span>;
<span class="hljs-comment">/* 示例 */</span>
extcon = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;usb_power_supply</span>&gt;</span>;
<span class="hljs-symbol">
usb_power_supply:</span> <span class="hljs-class">usb_power_supply </span>{
    ...
    pmu_usb_typec_used = <span class="hljs-params">&lt;<span class="hljs-number">1</span>&gt;</span>;
    ...
};
</code></pre>
<p><strong>属性2： jack-swpin-max</strong></p>
<p>从原理图中查看 TYPE-C AUDIO 的相关电路，包括如下两个转换模块，分别为 MIC/GND 转换模块与 USB/AUDIO 转换模块，确认所需配置的引脚数：</p>
<p><img src="figures/extcon耳机检测-pinmax1.png" alt="A523 EVB TYPE-C耳机检测电路"></p>
<ul>
<li>例如：如图中红框圈出，A523 EVB TYPE-C耳机检测电路中控制模式具有3个引脚，故将<strong>jack-swpin-max</strong>设置为3。</li>
</ul>
<p><img src="figures/extcon耳机检测-pinmax2.png" alt="AI985 SCANP TYPE-C耳机检测电路"></p>
<ul>
<li>例如：如图中红框圈出，AI985 SCANP TYPE-C耳机检测电路中控制模式具有2个引脚，故将<strong>jack-swpin-max</strong>设置为2。</li>
</ul>
<p><strong>属性3： jack-swpin-{n}</strong></p>
<p>从原理图中查看各个CC检测电路与USB AUDIO转换电路的引脚序号，并逐一配置。</p>
<pre><code><span class="hljs-keyword">jack-swpin-0 </span>     = &lt;&amp;pio PH <span class="hljs-number">8</span> GPIO_ACTIVE_HIGH&gt;<span class="hljs-comment">;</span>
...
<span class="hljs-comment">/* {n}等于jack-swpin-max的属性值 */</span>
<span class="hljs-keyword">jack-swpin-{n-1} </span>     = ...<span class="hljs-comment">;</span>
</code></pre><p><strong>属性4： jack‑mode‑off</strong></p>
<p>从原理图的真值表中查看TYPE-C识别为拔出状态时各个引脚的电平状态，若无真值表需要查看芯片手册或咨询硬件。</p>
<pre><code>低电平：<span class="hljs-number">0</span>； 高电平：<span class="hljs-number">1</span>；保持默认：<span class="hljs-number">0xf</span>。
示例配置：
<span class="hljs-comment">/* 属性中 value 个数等于jack-swpin-max */</span>
<span class="hljs-comment">/* 第 n 个 value 对应第 n 个 pin 的电平 */</span>
jack-mode-off = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">0xf</span>&gt;;
</code></pre><p><strong>属性5： jack‑mode‑usb</strong></p>
<p>从原理图的真值表中查看TYPE-C识别为USB状态时各个引脚的电平状态，若无真值表需要查看芯片手册或咨询硬件。</p>
<pre><code>低电平：<span class="hljs-number">0</span>； 高电平：<span class="hljs-number">1</span>；保持默认：<span class="hljs-number">0xf</span>。
示例配置：
<span class="hljs-comment">/* 属性中 value 个数等于jack-swpin-max */</span>
<span class="hljs-comment">/* 第 n 个 value 对应第 n 个 pin 的电平 */</span>
jack-mode-usb = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">0</span>&gt;;
</code></pre><p><strong>属性6： jack‑mode‑hp</strong></p>
<p>从原理图的真值表中查看TYPE-C识别为耳机状态时各个引脚的电平状态，若无真值表需要查看芯片手册或咨询硬件。</p>
<pre><code>低电平：<span class="hljs-number">0</span>； 高电平：<span class="hljs-number">1</span>；保持默认：<span class="hljs-number">0xf</span>。
示例配置：
<span class="hljs-comment">/* 属性中 value 个数等于jack-swpin-max */</span>
<span class="hljs-comment">/* 第 n 个 value 对应第 n 个 pin 的电平 */</span>
jack-mode-hp = &lt;<span class="hljs-number">0xf</span> <span class="hljs-number">1</span>&gt;;
</code></pre><p><strong>属性7： jack‑mode‑mici &amp; jack‑mode‑micn</strong></p>
<p>从原理图的真值表中查看TYPE-C识别为正插或反插时各个引脚的电平状态，若无真值表需要查看芯片手册或咨询硬件。</p>
<pre><code>低电平：<span class="hljs-number">0</span>； 高电平：<span class="hljs-number">1</span>；保持默认：<span class="hljs-number">0xf</span>。
示例配置：
<span class="hljs-comment">/* 属性中 value 个数等于jack-swpin-max */</span>
<span class="hljs-comment">/* 第 n 个 value 对应第 n 个 pin 的电平 */</span>
jack-mode-mici = &lt;<span class="hljs-number">0x1</span> <span class="hljs-number">0xf</span>&gt;;
jack-mode-micn = &lt;<span class="hljs-number">0x0</span> <span class="hljs-number">0xf</span>&gt;;
</code></pre><h4 id="gpio-">gpio耳机检测</h4>
<p>用于支持圆孔模拟耳机检测，IC无HP-DET引脚时需使用此方式。</p>
<p><strong>属性1： hp‑det‑gpio</strong></p>
<p>从原理图中查看耳机插入 gpio 检测引脚。</p>
<pre><code>示例配置：
hp-det-gpio = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;pio</span> PF <span class="hljs-number">3</span> GPIO_ACTIVE_HIGH&gt;</span>;
</code></pre><p><strong>属性2： jack‑det‑level</strong></p>
<p>参考内置&quot;codec耳机检测&quot;一章，功能一致。</p>
<h4 id="-">四段耳机属性</h4>
<p><strong>属性： jack‑key‑det‑voltage‑hook &amp; jack‑key‑det‑voltage‑up &amp; jack‑key‑det‑voltage‑down &amp; jack‑key‑det‑voltage‑voice</strong></p>
<p>默认按照出厂经验值即可，若出现按键误识别的情况，需要按如下步骤调试。</p>
<p>1.前提条件：在audiocodec驱动中打印出经过计算的&quot;SUNXI_HMIC_STA&quot;寄存器的值;</p>
<pre><code class="lang-bash">...
regmap_read(regmap, SUNXI_HMIC_STA, &amp;reg_val);
reg_val = (reg_val &amp; <span class="hljs-number">0x1f00</span>) &gt;&gt; <span class="hljs-number">8</span>;
<span class="hljs-comment">/* 增加如下打印 */</span>
SND_LOG_ERR(<span class="hljs-string">"reg_val:%u<span class="hljs-subst">\n</span>"</span>, reg_val);
...
</code></pre>
<p>2.使用多款不同厂商的耳机，逐一按下各个耳机按键，统计各款耳机不同按键下的&quot;reg_val&quot;;
3.逐步调整阈值范围，统计出一套能够兼容所有耳机的按键阈值范围，并确保各个按键阈值互不交错;
4.若出现阈值交错的情况，需确认SWITCH 器件或耳机检测外围电路是否异常。</p>
<h3 id="pa-">PA配置说明</h3>
<blockquote>
<p>电平使能方式配置</p>
</blockquote>
<p><strong>属性1： pa‑pin‑max</strong></p>
<p>1.从原理图中查看外部功放控制引脚数量。
2.部分功放具备电源控制引脚与使能引脚等，注意识别。</p>
<p><strong>属性2： pa‑pin‑{n}</strong></p>
<p>从原理图中查看各个外部功放控制引脚。</p>
<pre><code>示例配置：
pa-pin<span class="hljs-number">-0</span> = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;pio</span> PH <span class="hljs-number">6</span> GPIO_ACTIVE_HIGH&gt;</span>;
...
<span class="hljs-comment">/* {n}等于pa‑pin‑max的属性值 */</span>
pa-pin-{n<span class="hljs-number">-1</span>} = <span class="hljs-params">&lt;<span class="hljs-variable">&amp;pio</span> PH <span class="hljs-number">6</span> GPIO_ACTIVE_HIGH&gt;</span>;
</code></pre><p><strong>属性3： pa-pin-level-{n}</strong></p>
<p>从原理图或功放手册中查看各个外部功放控制引脚使能电平，一般为高电平。</p>
<p><strong>属性4： pa‑pin‑msleep‑{n}</strong></p>
<p>若启动播放时喇叭存在pop音，可逐步调整此配置用于规避pop音。此方法仅可规避IC内部产生的pop音。</p>
<p><strong>属性5： pa‑pin‑msleep1‑{n}</strong></p>
<p>若结束播放时喇叭存在pop音，可逐步调整此配置用于规避pop音。此方法仅可规避IC内部产生的pop音。</p>
<blockquote>
<p>脉冲使能方式附加配置</p>
</blockquote>
<p><strong>属性6： pa‑pin‑duty‑{n}</strong></p>
<p>一个周期内电平为polarity时的时长，从功放手册中确定使能脉冲宽度。</p>
<p><strong>属性7： pa‑pin‑period‑{n}</strong></p>
<p>一个周期的时长，从功放手册中确定使能脉冲周期。</p>
<p><strong>属性8： pa‑pin‑polarity‑{n}</strong></p>
<p>一个周期的起始电平，从功放手册中确定使能脉冲极性。</p>
<p><strong>属性9： pa‑pin‑periodcnt‑{n}</strong></p>
<p>单次使能脉冲的周期个数，从功放手册中确定使能脉冲个数。</p>
<p><img src="figures/pa_enable_pulse.png" alt="脉冲使能方式示例"></p>
<h3 id="i2s-">I2S通道映射</h3>
<h4 id="tx-">TX通道映射</h4>
<p>TX通道映射定义了音频数据流中每个逻辑声道（如左声道、右声道）在物理输出接口（如I2S的DATA线）上的传输顺序和位置关系。具体来说，它规定了每个声道的音频采样值在输出数据流中的排列方式，即txfifo channel序号映射到DOUT的tx slot序号。</p>
<ol>
<li>tx-pin 表示所需使能的 data 线;</li>
<li>tx-pin{n}-chmap[i] 的参数个数为通道数，表示第 n 路 data 的第 i 通道音频数据对应哪个 slot(0~15)；</li>
</ol>
<p>如下示例，假设需要使用 DOUT0 与 DOUT1 输出音频数据，其中 DOUT0 的第一通道数据来源于播放流的 slot0, 第二通道数据来源于播放流的 slot1;
DOUT1 的第一通道数据来源于播放流的 slot1, 第二通道数据来源于播放流的 slot0;</p>
<pre><code>    tx-pin           = &lt;<span class="hljs-number">0</span> <span class="hljs-number">1</span>&gt;;
    tx-pin0-chmap    = &lt;<span class="hljs-number">0</span> <span class="hljs-number">1</span>&gt;;
    tx-pin1-chmap    = &lt;<span class="hljs-number">1</span> <span class="hljs-number">0</span>&gt;;
</code></pre><h4 id="rx-">RX通道映射</h4>
<p>RX通道映射与TX通道映射功能类似，映射方向相反。即该属性用于设置DIN的rx slot序号映射到rxfifo channel的序号。</p>
<ol>
<li>rxfifo-pinmap和rxfifo-chmap的参数个数，即为通道数;</li>
<li>rxfifo-pinmap[i] 表示第 i 个通道的音频数据采集于哪个 data 线；</li>
<li>rxfifo-chmap[j] 表示第 j 个通道的音频数据来源于 rxfifo-pinmap[j] 对应 data 线的哪个 solt(0~15)。</li>
</ol>
<p>如下示例，假设需录制 2ch 音频，rxfifo-pinmap 设置了录音流的第一通道数据采集于 DIN0, 录音流的第二通道数据采集于 DIN1；
rxfifo-chmap 设置了录音流的第一通道数据来源于 DIN0 的 slot0, 录音流的第二通道数据来源于 DIN1 的 slot1:</p>
<pre><code class="lang-bash"><span class="hljs-attribute">    rxfifo-pinmap</span> = &lt;0 1&gt;;
<span class="hljs-attribute">    rxfifo-chmap</span>  = &lt;0 1&gt;;
</code></pre>
<h3 id="dmic-">DMIC通道映射</h3>
<p>DMIC每路 DATA 可采集 2ch 数据，属性值采用十六进制表示，其中 bit0-bit3 代表录音流的第一通道，bit4-bit7 代表录音流的第二通道，后续依此类推；
数字 0 代表 DATA0 的 slot 0，数字 1 代表 DATA0 的 slot1；
数字 2 代表 DATA1 的 slot 0，数字 3 代表 DATA1 的 slot1；
数字 4 代表 DATA2 的 slot 0，数字 5 代表 DATA2 的 slot1；
数字 6 代表 DATA3 的 slot 0，数字 7 代表 DATA3 的 slot1；</p>
<p>如下示例，使用DATA1录制2ch音频，录音流的第一通道映射DATA1的左声道，第二通道映射DATA1的右声道，对应的dts配置为：</p>
<pre><code class="lang-bash"><span class="hljs-attribute">    rx-chmap</span> = &lt;0x32&gt;;
</code></pre>
<h2 id="-">时钟树 {#时钟树}</h2>
<h3 id="sun8iw20">sun8iw20</h3>
<p>sun8iw20 音频模块时钟源来自 PLL_AUDIO0 和 PLL_AUDIO1_DIV5。</p>
<p>PLL_AUDIO0 可输出 22.5792M，PLL_AUDIO1_DIV5 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。</p>
<p><img src="figures/时钟树-sun8iw20.png" alt="时钟树 sun8iw20"></p>
<h3 id="sun8iw21">sun8iw21</h3>
<p>sun8iw21 音频模块时钟源来自 PLL_AUDIO_4X。</p>
<p>PLL_AUDIO 可输出 22.5792M 和 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音，但无法同时输出。</p>
<p><img src="figures/时钟树-sun8iw21.png" alt="时钟树 sun8iw21"></p>
<h3 id="sun8iw11">sun8iw11</h3>
<p>sun8iw11 音频模块时钟源来自 PLL_AUDIO。</p>
<p>PLL_AUDIO 可输出 22.5792M 和 24.576M 频率的时钟，分别支持 44.1K 系列、48K 系列的播放录音，但无法同时输出。</p>
<p><img src="figures/时钟树-sun8iw11.png" alt="时钟树 sun8iw11"></p>
<h3 id="sun8iw22">sun8iw22</h3>
<p>sun8iw22 音频模块时钟源来自 PLL_AUDIO0 和 PERI1_600M。</p>
<p>PLL_AUDIO0 可输出 22.5792M，PERI1_600M 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。</p>
<p><img src="figures/时钟树-sun8iw22.png" alt="时钟树 sun8iw22"></p>
<h3 id="sun50iw9">sun50iw9</h3>
<p>sun50iw9 音频模块时钟源来自 PLL_AUDIO_4X。</p>
<p>PLL_AUDIO_4X 可输出 90.3168M 和 98.304M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音，但无法同时输出。</p>
<p><img src="figures/时钟树-sun50iw9.png" alt="时钟树 sun50iw9"></p>
<h3 id="sun50iw10">sun50iw10</h3>
<p>sun50iw10 音频模块时钟源来自 PLL_COM_AUDIO 和 PLL_AUDIO。</p>
<p>PLL_COM_AUDIO 可输出 90.3168M，PLL_AUDIO 可输出 98.304M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。</p>
<p><img src="figures/时钟树-sun50iw10.png" alt="时钟树 sun50iw10"></p>
<p>::: note</p>
<p>PLL_xxx 到模块时钟，模块时钟将其4分频，最终输出 22.5792M 和 24.576M 的时钟。</p>
<p>:::</p>
<h3 id="sun55iw3">sun55iw3</h3>
<p>sun55iw3 音频模块时钟源来自 PLL_AUDIO0_4X, PLL_AUDIO1_DIV2, PLL_AUDIO1_DIV5, PLL_PERI0_300M。</p>
<p>PLL_AUDIO0、PLL_AUDIO1_DIV2 和 PLL_AUDIO1_DIV5 均可输出 22.5792M 和 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_AUDIO1_DIV2 和 PLL_AUDIO1_DIV5 同时使用时只能输出相同的频率。PLL_PERI0_300M 输出 300M。</p>
<p><img src="figures/时钟树-sun55iw3.png" alt="时钟树 sun55iw3"></p>
<h3 id="sun55iw6">sun55iw6</h3>
<p>sun55iw6 音频模块时钟源来自 PLL_AUDIO0_4X, PLL_AUDIO1_4X。</p>
<p>PLL_AUDIO0 可输出 22.5792M，PLL_AUDIO1_DIV5 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_PERI0_300M 输出 300M。</p>
<p><img src="figures/时钟树-sun55iw6.png" alt="时钟树 sun55iw6"></p>
<h3 id="sun300iw1">sun300iw1</h3>
<p>sun300iw1 音频模块时钟源来自 PLL_AUDIO0_1X。</p>
<p>PLL_AUDIO0_1X 可输出 24.576M，支持48K系列的播放录音。</p>
<p><img src="figures/时钟树-sun300iw1.png" alt="时钟树 sun300iw1"></p>
<h3 id="sun251iw1">sun251iw1</h3>
<p>sun251iw1 音频模块时钟源来自 PLL_AUDIO0_1X, PLL_AUDIO1_DIV5, PLL_AUDIO0_4X, PLL_PERI_1X。</p>
<p>PLL_AUDIO0_1X和PLL_AUDIO0_4X 可输出 22.5792M 频率的时钟，PLL_AUDIO1_DIV5 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_PERI_1X 输出 600M。</p>
<p><img src="figures/时钟树-sun251iw1.png" alt="时钟树 sun251iw1"></p>
<h3 id="sun60iw2">sun60iw2</h3>
<p>sun60iw2 音频模块时钟源来自 PLL_AUDIO0_4X, PLL_AUDIO1_DIV5。</p>
<p>PLL_AUDIO0_4X 可输出 22.5792M 频率的时钟，PLL_AUDIO1_DIV5 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_PERI0_200X 输出 200M。</p>
<p><img src="figures/时钟树-sun60iw2.png" alt="时钟树 sun60iw2"></p>
<h3 id="sun50iw15">sun50iw15</h3>
<p>sun50iw15 音频模块时钟源来自 PLL_AUDIO_4X。</p>
<p>PLL_AUDIO_4X 可输出 22.5792M、24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_PERI0_400X 输出 400M。</p>
<p><img src="figures/时钟树-sun50iw15.png" alt="时钟树 sun50iw15"></p>
<h3 id="sun65iw1">sun65iw1</h3>
<p>sun65iw1 音频模块时钟源来自 PLL_AUDIO0, PLL_AUDIO1_5X。</p>
<p>PLL_AUDIO0 可输出 22.5792M 频率的时钟，PLL_AUDIO1_5X 可输出 24.576M 频率的时钟，分别支持 44.1K 系列、48K系列的播放录音。PLL_PERI0_300X 输出 300M。</p>
<p><img src="figures/时钟树-sun65iw1.png" alt="时钟树 sun65iw1"></p>
<h2 id="audiocodec-audiocodec-">AudioCodec声卡使用 {#AudioCodec声卡使用}</h2>
<h3 id="sun8iw20">sun8iw20</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: 'audiocodec'
Number of controls: 42
ctl     type    num     name                                    value
0       ENUM    1       DAC DRC Mode                            , OffOn
1       ENUM    1       DAC HPF Mode                            , OffOn
2       ENUM    1       ADC DRC0 Mode                           , OffOn
3       ENUM    1       ADC HPF0 Mode                           , OffOn
4       ENUM    1       ADC DRC1 Mode                           , OffOn
5       ENUM    1       ADC HPF1 Mode                           , OffOn
6       ENUM    1       ADC1 ADC2 Swap                          , OffOn
7       ENUM    1       ADC3 ADC4 Swap                          , OffOn
8       ENUM    1       LINEOUTL Output <span class="hljs-keyword">Select</span>                  , singlediffer
<span class="hljs-number">9</span>       ENUM    <span class="hljs-number">1</span>       LINEOUTR <span class="hljs-keyword">Output</span> <span class="hljs-keyword">Select</span>                  , singlediffer
<span class="hljs-number">10</span>      ENUM    <span class="hljs-number">1</span>       MIC1 <span class="hljs-keyword">Input</span> <span class="hljs-keyword">Select</span>                       , singlediffer
<span class="hljs-number">11</span>      ENUM    <span class="hljs-number">1</span>       MIC2 <span class="hljs-keyword">Input</span> <span class="hljs-keyword">Select</span>                       , singlediffer
<span class="hljs-number">12</span>      ENUM    <span class="hljs-number">1</span>       MIC3 <span class="hljs-keyword">Input</span> <span class="hljs-keyword">Select</span>                       single, differ
<span class="hljs-number">13</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DAC Digital Volume                      <span class="hljs-number">63</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">14</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACL Volume                             <span class="hljs-number">160</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">15</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACR Volume                             <span class="hljs-number">160</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">16</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC1 Volume                             <span class="hljs-number">160</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">17</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC2 Volume                             <span class="hljs-number">160</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">18</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC3 Volume                             <span class="hljs-number">160</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">19</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC1 Gain                               <span class="hljs-number">31</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">20</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC2 Gain                               <span class="hljs-number">31</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">21</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC3 Gain                               <span class="hljs-number">31</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">22</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       LINEOUT Volume                          <span class="hljs-number">20</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">23</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       HPOUT Gain                              <span class="hljs-number">7</span> (<span class="hljs-keyword">range</span> <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       FMINL Gain                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">25</span>      BOOL    <span class="hljs-number">1</span>       FMINR Gain                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">26</span>      BOOL    <span class="hljs-number">1</span>       LINEINL Gain                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">27</span>      BOOL    <span class="hljs-number">1</span>       LINEINR Gain                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">28</span>      BOOL    <span class="hljs-number">1</span>       MIC1 <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">29</span>      BOOL    <span class="hljs-number">1</span>       MIC2 <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">30</span>      BOOL    <span class="hljs-number">1</span>       MIC3 <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">On</span>
<span class="hljs-number">31</span>      BOOL    <span class="hljs-number">1</span>       FMINL <span class="hljs-keyword">Switch</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">32</span>      BOOL    <span class="hljs-number">1</span>       FMINR <span class="hljs-keyword">Switch</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">33</span>      BOOL    <span class="hljs-number">1</span>       LINEINL <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">34</span>      BOOL    <span class="hljs-number">1</span>       LINEINR <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">35</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-keyword">Switch</span>                         <span class="hljs-keyword">On</span>
<span class="hljs-number">36</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTR <span class="hljs-keyword">Switch</span>                         <span class="hljs-keyword">On</span>
<span class="hljs-number">37</span>      BOOL    <span class="hljs-number">1</span>       HPOUT <span class="hljs-keyword">Switch</span>                            <span class="hljs-keyword">On</span>
<span class="hljs-number">38</span>      BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">On</span>
<span class="hljs-number">39</span>      ENUM    <span class="hljs-number">1</span>       Input1 Mux                              , MIC1FMINLLINEINL
<span class="hljs-number">40</span>      ENUM    <span class="hljs-number">1</span>       Input2 Mux                              , MIC2FMINRLINEINR
<span class="hljs-number">41</span>      ENUM    <span class="hljs-number">1</span>       Input3 Mux                              , MIC3
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见<strong>“模块使用-&gt;声卡测试工具使用-&gt;tinyalsa 工具”</strong>章节；</li>
<li>假设 audiocodec 声卡序号为 0。声卡序号通过 <strong>cat /proc/asound/cards </strong>查看。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">0</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC2 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">0</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p> MIC双通道输入（从mic1、mic2、mic3中任选2个即可）</p>
<pre><code class="lang-bash"># 录音通路控件（mic1 &amp; mic2）
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">0</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">0</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">3</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>FMIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"FMINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"FMINR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">1</span>
</code></pre>
<p>LINEIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">2</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">2</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUT 双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTR Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>HPOUT 双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>SPK 单/双通道播放</p>
<pre><code class="lang-bash"># 上述播放示例基础上将 <span class="hljs-string">"SPK Switch"</span> 控件打开即可
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
</code></pre>
<p>::: note</p>
<p>MIC和LINEOUT 的单端和差分方式，按需操作相应控件即可。</p>
<p>:::</p>
<h3 id="sun8iw21">sun8iw21</h3>
<h4 id="-">声卡控件</h4>
<pre><code class="lang-bash">Mixer name: <span class="hljs-comment">'audiocodec'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">24</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx sync mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACDRC                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADCDRC                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACHPF                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADCHPF                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC1 ADC2 swap                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">7</span>       INT     <span class="hljs-number">1</span>       digital volume                           <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">8</span>       INT     <span class="hljs-number">1</span>       DAC volume                               <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">9</span>       INT     <span class="hljs-number">1</span>       ADC1 volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       ADC2 volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       MIC1 gain volume                         <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       MIC2 gain volume                         <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">13</span>      BOOL    <span class="hljs-number">1</span>       LINEINL gain volume                      <span class="hljs-keyword">Off</span>
<span class="hljs-number">14</span>      BOOL    <span class="hljs-number">1</span>       LINEINR gain volume                      <span class="hljs-keyword">Off</span>
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       LINEOUT volume                           <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">16</span>      BOOL    <span class="hljs-number">1</span>       MIC1 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">17</span>      BOOL    <span class="hljs-number">1</span>       MIC2 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">18</span>      BOOL    <span class="hljs-number">1</span>       LINEIN Switch                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">19</span>      BOOL    <span class="hljs-number">1</span>       LINEOUT Switch                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">20</span>      BOOL    <span class="hljs-number">1</span>       SPK Switch                               <span class="hljs-keyword">Off</span>
<span class="hljs-number">21</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       LINEOUT Output <span class="hljs-keyword">Select</span>                    <span class="hljs-built_in">single</span> &gt;differ
<span class="hljs-number">22</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       MIC1 Input <span class="hljs-keyword">Select</span>                        &gt;differ <span class="hljs-built_in">single</span>
<span class="hljs-number">23</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       MIC2 Input <span class="hljs-keyword">Select</span>                        &gt;differ <span class="hljs-built_in">single</span>
</code></pre>
<h4 id="-">常见使用说明</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC2 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC1&amp;2 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>LINEIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEIN Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Input Select"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Input Select"</span> <span class="hljs-number">1</span>
tinycap linein.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>::: note</p>
<p>LINEIN和MIC输入属于mux的关系，即只能二选一，不能同时开启LINEIN和MIC的通路开关。</p>
<p>:::</p>
<p><strong>播放</strong></p>
<p>LINEOUT 单/双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
# 或
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>Speaker 单/双通道播放</p>
<pre><code class="lang-bash"># 上述播放示例基础上将 <span class="hljs-string">"SPK Switch"</span> 控件打开即可
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
</code></pre>
<h3 id="sun8iw11">sun8iw11</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code>Mixer name: <span class="hljs-string">'audiocodec'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">58</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       DAC DRC <span class="hljs-keyword">Switch</span>                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       ENUM    <span class="hljs-number">1</span>       DAC HPF <span class="hljs-keyword">Switch</span>                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       ENUM    <span class="hljs-number">1</span>       ADC DRC <span class="hljs-keyword">Switch</span>                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       ENUM    <span class="hljs-number">1</span>       ADC HPF <span class="hljs-keyword">Switch</span>                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       ENUM    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       INT     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">6</span>       INT     <span class="hljs-number">1</span>       ADC Gain                                 <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">7</span>       INT     <span class="hljs-number">1</span>       MIC1 Gain                                <span class="hljs-number">4</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">8</span>       INT     <span class="hljs-number">1</span>       MIC2 Gain                                <span class="hljs-number">4</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">9</span>       INT     <span class="hljs-number">1</span>       MIC1 to OMIX Gain                        <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       MIC2 to OMIX Gain                        <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       FMIN to OMIX Gain                        <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       LINEIN to OMIX Gain                      <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       LINEINL to ROMIX Gain                    <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       LINEINR to LOMIX Gain                    <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       PHONEOUT Gain                            <span class="hljs-number">3</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">16</span>      INT     <span class="hljs-number">1</span>       HPOUT Gain                               <span class="hljs-number">63</span> (range <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">17</span>      BOOL    <span class="hljs-number">1</span>       MIC1 <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">18</span>      BOOL    <span class="hljs-number">1</span>       MIC2 <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">19</span>      BOOL    <span class="hljs-number">1</span>       FMIN <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">20</span>      BOOL    <span class="hljs-number">1</span>       LINEIN <span class="hljs-keyword">Switch</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">21</span>      BOOL    <span class="hljs-number">1</span>       HPOUT <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">22</span>      BOOL    <span class="hljs-number">1</span>       PHONEOUT <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                               <span class="hljs-keyword">Off</span>
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer DACL <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">25</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer DACR <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">26</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer MIC1 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">27</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer MIC2 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">28</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer FMINL <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">29</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer LINEINL <span class="hljs-keyword">Switch</span>         <span class="hljs-keyword">Off</span>
<span class="hljs-number">30</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> Output Mixer LINEINLR <span class="hljs-keyword">Switch</span>        <span class="hljs-keyword">Off</span>
<span class="hljs-number">31</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer DACL <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">32</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer DACR <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">33</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer MIC1 <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">34</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer MIC2 <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">35</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer FMINR <span class="hljs-keyword">Switch</span>          <span class="hljs-keyword">Off</span>
<span class="hljs-number">36</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer LINEINR <span class="hljs-keyword">Switch</span>        <span class="hljs-keyword">Off</span>
<span class="hljs-number">37</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> Output Mixer LINEINLR <span class="hljs-keyword">Switch</span>       <span class="hljs-keyword">Off</span>
<span class="hljs-number">38</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer MIC1 <span class="hljs-keyword">Switch</span>             <span class="hljs-keyword">Off</span>
<span class="hljs-number">39</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer MIC2 <span class="hljs-keyword">Switch</span>             <span class="hljs-keyword">Off</span>
<span class="hljs-number">40</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer FMINL <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">41</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer LINEINL <span class="hljs-keyword">Switch</span>          <span class="hljs-keyword">Off</span>
<span class="hljs-number">42</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer LINEINLR <span class="hljs-keyword">Switch</span>         <span class="hljs-keyword">Off</span>
<span class="hljs-number">43</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer LOMIX <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">44</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Left</span> <span class="hljs-keyword">Input</span> Mixer ROMIX <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">45</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer MIC1 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">46</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer MIC2 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">47</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer FMINR <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">48</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer LINEINR <span class="hljs-keyword">Switch</span>         <span class="hljs-keyword">Off</span>
<span class="hljs-number">49</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer LINEINLR <span class="hljs-keyword">Switch</span>        <span class="hljs-keyword">Off</span>
<span class="hljs-number">50</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer LOMIX <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">51</span>      BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Right</span> <span class="hljs-keyword">Input</span> Mixer ROMIX <span class="hljs-keyword">Switch</span>           <span class="hljs-keyword">Off</span>
<span class="hljs-number">52</span>      BOOL    <span class="hljs-number">1</span>       PHONEOUT Mixer MIC1 <span class="hljs-keyword">Switch</span>               <span class="hljs-keyword">Off</span>
<span class="hljs-number">53</span>      BOOL    <span class="hljs-number">1</span>       PHONEOUT Mixer MIC2 <span class="hljs-keyword">Switch</span>               <span class="hljs-keyword">Off</span>
<span class="hljs-number">54</span>      BOOL    <span class="hljs-number">1</span>       PHONEOUT Mixer LOMIX <span class="hljs-keyword">Switch</span>              <span class="hljs-keyword">Off</span>
<span class="hljs-number">55</span>      BOOL    <span class="hljs-number">1</span>       PHONEOUT Mixer ROMIX <span class="hljs-keyword">Switch</span>              <span class="hljs-keyword">Off</span>
<span class="hljs-number">56</span>      ENUM    <span class="hljs-number">1</span>       HPL Source                               &gt;DACL LOMIX HPR
<span class="hljs-number">57</span>      ENUM    <span class="hljs-number">1</span>       HPR Source                               &gt;DACR ROMIX HPL
</code></pre><h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<pre><code class="lang-bash"># MIC1 单通道录音
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Left Input Mixer MIC1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<pre><code class="lang-bash"># MIC2 单通道录音
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Right Input Mixer MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<pre><code class="lang-bash"># MIC1&amp;<span class="hljs-number">2</span> 双通道输入
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Left Input Mixer MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Right Input Mixer MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<pre><code class="lang-bash"># HPOUT 单/双通道播放
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
# 或
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<pre><code class="lang-bash"># PHONEOUT 单/双通道播放（外接喇叭）
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"PHONEOUT Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Left Output Mixer DACL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Right Output Mixer DACR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"PHONEOUT Mixer LOMIX Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"PHONEOUT Mixer ROMIX Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
# 或
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun8iw22">sun8iw22</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer <span class="hljs-built_in">na</span><span class="hljs-symbol">me:</span> 'audiocodec'
Number of contro<span class="hljs-symbol">ls:</span> <span class="hljs-number">8</span>
ctl     <span class="hljs-built_in">type</span>    num     name                                     <span class="hljs-built_in">value</span>

<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       tx hub <span class="hljs-built_in">mode</span>                              Off
<span class="hljs-number">1</span>       ENUM    <span class="hljs-number">1</span>       DAC DRC <span class="hljs-built_in">Mode</span>                             Off
<span class="hljs-number">2</span>       ENUM    <span class="hljs-number">1</span>       DAC HPF <span class="hljs-built_in">Mode</span>                             Off
<span class="hljs-number">3</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span>
<span class="hljs-number">4</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">5</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">31</span>
<span class="hljs-number">6</span>       BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-built_in">Switch</span>                          Off
<span class="hljs-number">7</span>       BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-built_in">Switch</span>                               Off
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>播放</strong></p>
<p>LINEOUT 单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun50iw9">sun50iw9</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-string">'audiocodec'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">9</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       INT     <span class="hljs-number">1</span>       digital volume                           <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">2</span>       INT     <span class="hljs-number">1</span>       lineout volume                           <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">3</span>       BOOL    <span class="hljs-number">1</span>       LINEOUT <span class="hljs-keyword">Switch</span>                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">4</span>       BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                               <span class="hljs-keyword">Off</span>
<span class="hljs-number">5</span>       BOOL    <span class="hljs-number">1</span>       OutputL Mixer DACL <span class="hljs-keyword">Switch</span>                <span class="hljs-keyword">Off</span>
<span class="hljs-number">6</span>       BOOL    <span class="hljs-number">1</span>       OutputL Mixer DACR <span class="hljs-keyword">Switch</span>                <span class="hljs-keyword">Off</span>
<span class="hljs-number">7</span>       BOOL    <span class="hljs-number">1</span>       OutputR Mixer DACL <span class="hljs-keyword">Switch</span>                <span class="hljs-keyword">Off</span>
<span class="hljs-number">8</span>       BOOL    <span class="hljs-number">1</span>       OutputR Mixer DACR <span class="hljs-keyword">Switch</span>                <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>播放</strong></p>
<p>LINEOUT 左单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputL Mixer DACL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUT 右单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputR Mixer DACR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUT 左右双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputL Mixer DACR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputR Mixer DACL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUT 左右双通道交换播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputL Mixer DACL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"OutputR Mixer DACR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>Speaker 播放</p>
<pre><code class="lang-bash"># 上述播放示例基础上将 <span class="hljs-string">"SPK Switch"</span> 控件打开即可
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
</code></pre>
<h3 id="sun50iw10">sun50iw10</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-comment">'audiocodec'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">24</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx sync mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADCDRC                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADCHPF                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACDRC                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACHPF                                   &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC Swap                                 &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC Swap                                 &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       LINEOUT Output <span class="hljs-keyword">Select</span>                    <span class="hljs-built_in">single</span> &gt;differ
<span class="hljs-number">9</span>       BOOL    <span class="hljs-number">1</span>       <span class="hljs-keyword">Loop</span> ADDA                                <span class="hljs-keyword">Off</span>
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       ADC1 volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       ADC2 volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       DAC digital volume                       <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       DACL volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       DACR volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       MIC1 volume                              <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">16</span>      INT     <span class="hljs-number">1</span>       MIC2 volume                              <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">17</span>      INT     <span class="hljs-number">1</span>       LINEOUT volume                           <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">18</span>      INT     <span class="hljs-number">1</span>       HPOUT volume                             <span class="hljs-number">7</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">19</span>      BOOL    <span class="hljs-number">1</span>       MIC1 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">20</span>      BOOL    <span class="hljs-number">1</span>       MIC2 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">21</span>      BOOL    <span class="hljs-number">1</span>       LINEOUT Switch                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">22</span>      BOOL    <span class="hljs-number">1</span>       HPOUT Switch                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       SPK Switch                               <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC2 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC1&amp;2 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>LINEIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEIN Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Input Select"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Input Select"</span> <span class="hljs-number">1</span>
tinycap linein.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>::: note</p>
<p>LINEIN和MIC输入属于mux的关系，即只能二选一，不能同时开启LINEIN和MIC的通路开关。</p>
<p>:::</p>
<p><strong>播放</strong></p>
<p>LINEOUT 单/双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
# 或
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>Speaker 单/双通道播放</p>
<pre><code class="lang-bash"># 上述播放示例基础上将 <span class="hljs-string">"SPK Switch"</span> 控件打开即可
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
</code></pre>
<h3 id="sun55iw3">sun55iw3</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-string">'audiocodec'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">30</span>
ctl     type    num     name                                     value
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       tx hub mode                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">1</span>       ENUM    <span class="hljs-number">1</span>       rx sync mode                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">2</span>       ENUM    <span class="hljs-number">1</span>       DAC DRC <span class="hljs-keyword">Mode</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">3</span>       ENUM    <span class="hljs-number">1</span>       DAC HPF <span class="hljs-keyword">Mode</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">4</span>       ENUM    <span class="hljs-number">1</span>       ADC DRC0 <span class="hljs-keyword">Mode</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">5</span>       ENUM    <span class="hljs-number">1</span>       ADC HPF0 <span class="hljs-keyword">Mode</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">6</span>       ENUM    <span class="hljs-number">1</span>       ADC DRC1 <span class="hljs-keyword">Mode</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">7</span>       ENUM    <span class="hljs-number">1</span>       ADC HPF1 <span class="hljs-keyword">Mode</span>                            <span class="hljs-keyword">Off</span>
<span class="hljs-number">8</span>       ENUM    <span class="hljs-number">1</span>       ADDA Loop <span class="hljs-keyword">Mode</span>                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">9</span>       ENUM    <span class="hljs-number">1</span>       DACL DACR Swap                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">10</span>      ENUM    <span class="hljs-number">1</span>       ADC1 ADC2 Swap                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">11</span>      ENUM    <span class="hljs-number">1</span>       ADC3 ADC4 Swap                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span>
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       DACR Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       ADC1 Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">16</span>      INT     <span class="hljs-number">1</span>       ADC2 Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">17</span>      INT     <span class="hljs-number">1</span>       ADC3 Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">18</span>      INT     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">31</span>
<span class="hljs-number">19</span>      INT     <span class="hljs-number">1</span>       HPOUT Gain                               <span class="hljs-number">7</span>
<span class="hljs-number">20</span>      INT     <span class="hljs-number">1</span>       ADC1 Gain                                <span class="hljs-number">31</span>
<span class="hljs-number">21</span>      INT     <span class="hljs-number">1</span>       ADC2 Gain                                <span class="hljs-number">31</span>
<span class="hljs-number">22</span>      INT     <span class="hljs-number">1</span>       ADC3 Gain                                <span class="hljs-number">31</span>
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       MIC1 <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       MIC2 <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">25</span>      BOOL    <span class="hljs-number">1</span>       MIC3 <span class="hljs-keyword">Switch</span>                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">26</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">27</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTR <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">28</span>      BOOL    <span class="hljs-number">1</span>       HPOUT <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">29</span>      BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                               <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC2 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC1&amp;2 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUT 单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUT 双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>HPOUT 双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun55iw6">sun55iw6</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer <span class="hljs-built_in">na</span><span class="hljs-symbol">me:</span> 'audiocodec'
Number of contro<span class="hljs-symbol">ls:</span> <span class="hljs-number">7</span>
ctl     <span class="hljs-built_in">type</span>    num     name                                     <span class="hljs-built_in">value</span>

<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       DAC DRC <span class="hljs-built_in">Mode</span>                             Off
<span class="hljs-number">1</span>       ENUM    <span class="hljs-number">1</span>       DAC HPF <span class="hljs-built_in">Mode</span>                             Off
<span class="hljs-number">2</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span>
<span class="hljs-number">3</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">4</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">31</span>
<span class="hljs-number">5</span>       BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-built_in">Switch</span>                          Off
<span class="hljs-number">6</span>       BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-built_in">Switch</span>                               Off
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>播放</strong></p>
<p>LINEOUT 单/双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
# 或
tinyplay test_2ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun300iw1">sun300iw1</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-comment">'audiocodec'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">18</span>
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx sync mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC DRC Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC HPF Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC DRC0 Mode                            &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC HPF0 Mode                            &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC DRC1 Mode                            &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC HPF1 Mode                            &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       LINEOUT Output <span class="hljs-keyword">Select</span>                    DIFFER &gt;<span class="hljs-built_in">SINGLE</span>
<span class="hljs-number">9</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADDA <span class="hljs-keyword">Loop</span> Mode                           &gt;<span class="hljs-keyword">Off</span> DAC-<span class="hljs-keyword">to</span>-ADC
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       ADC Volume                               <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       MIC Gain                                 <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">15</span>      BOOL    <span class="hljs-number">1</span>       MIC Switch                               <span class="hljs-keyword">Off</span>
<span class="hljs-number">16</span>      BOOL    <span class="hljs-number">1</span>       LINEOUT Switch                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">17</span>      BOOL    <span class="hljs-number">1</span>       SPK Switch                               <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUT 单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUT Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun251iw1">sun251iw1</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-string">'audiocodec'</span>
<span class="hljs-built_in">Number</span> of controls: <span class="hljs-number">35</span>
ctl     type    num     name                                     value

<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC DRC Mode                             Off
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC HPF Mode                             Off
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC DRC0 Mode                            Off
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC HPF0 Mode                            Off
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADDA Loop Mode                           Off
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACL DACR Swap                           Off
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC1 ADC2 Swap                           Off
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       MIC1 Input <span class="hljs-keyword">Select</span>                        differ
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       MIC2 Input <span class="hljs-keyword">Select</span>                        differ
<span class="hljs-number">9</span>       <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span>
<span class="hljs-number">10</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">11</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       DACR Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">12</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC1 Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">13</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC2 Volume                              <span class="hljs-number">160</span>
<span class="hljs-number">14</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       FMINL Gain                               <span class="hljs-number">0</span>dB
<span class="hljs-number">15</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       FMINR Gain                               <span class="hljs-number">0</span>dB
<span class="hljs-number">16</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       LINEINL Gain                             <span class="hljs-number">0</span>dB
<span class="hljs-number">17</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       LINEINR Gain                             <span class="hljs-number">0</span>dB
<span class="hljs-number">18</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       LINEOUT Volume                           <span class="hljs-number">7</span>
<span class="hljs-number">19</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       HPOUT Gain                               <span class="hljs-number">7</span>
<span class="hljs-number">20</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC1 Gain                                <span class="hljs-number">0</span>
<span class="hljs-number">21</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">1</span>       ADC2 Gain                                <span class="hljs-number">0</span>
<span class="hljs-number">22</span>      BOOL    <span class="hljs-number">1</span>       MIC1 <span class="hljs-keyword">Switch</span>                              Off
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       MIC2 <span class="hljs-keyword">Switch</span>                              Off
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       FMINL <span class="hljs-keyword">Switch</span>                             Off
<span class="hljs-number">25</span>      BOOL    <span class="hljs-number">1</span>       FMINR <span class="hljs-keyword">Switch</span>                             Off
<span class="hljs-number">26</span>      BOOL    <span class="hljs-number">1</span>       LINEINL <span class="hljs-keyword">Switch</span>                           Off
<span class="hljs-number">27</span>      BOOL    <span class="hljs-number">1</span>       LINEINR <span class="hljs-keyword">Switch</span>                           Off
<span class="hljs-number">28</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-keyword">Switch</span>                          Off
<span class="hljs-number">29</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTR <span class="hljs-keyword">Switch</span>                          Off
<span class="hljs-number">30</span>      BOOL    <span class="hljs-number">1</span>       HPOUT <span class="hljs-keyword">Switch</span>                             Off
<span class="hljs-number">31</span>      BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                               Off
<span class="hljs-number">32</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       Input1 Mux                               NONE
<span class="hljs-number">33</span>      <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       Input2 Mux                               NONE
<span class="hljs-number">34</span>      <span class="hljs-built_in">INT</span>     <span class="hljs-number">2</span>       Soft Volume Master                       <span class="hljs-number">75</span> <span class="hljs-number">75</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC1&amp;2 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>LINEIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">3</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">3</span>
tinycap linein.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>FMIN 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"FMINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"FMINR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input1 Mux"</span> <span class="hljs-number">2</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"Input2 Mux"</span> <span class="hljs-number">2</span>
tinycap linein.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUT 单通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUT 双通道播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>HPOUT 播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun50iw15">sun50iw15</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-string">'audiocodec'</span>
<span class="hljs-keyword">Number</span> of controls: <span class="hljs-number">31</span>
ctl     type    num     name                                     value
        range/values
<span class="hljs-number">0</span>       ENUM    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       ENUM    <span class="hljs-number">1</span>       rx sync mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       ENUM    <span class="hljs-number">1</span>       DAC DRC <span class="hljs-keyword">Mode</span>                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       ENUM    <span class="hljs-number">1</span>       DAC HPF <span class="hljs-keyword">Mode</span>                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       ENUM    <span class="hljs-number">1</span>       ADC DRC <span class="hljs-keyword">Mode</span>                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       ENUM    <span class="hljs-number">1</span>       ADC HPF <span class="hljs-keyword">Mode</span>                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">6</span>       ENUM    <span class="hljs-number">1</span>       DA2AD Loop <span class="hljs-keyword">Mode</span>                          &gt;<span class="hljs-keyword">Off</span> DACLR-to-ADC12
<span class="hljs-number">7</span>       ENUM    <span class="hljs-number">1</span>       DACL DACR Swap                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">8</span>       ENUM    <span class="hljs-number">1</span>       ADC1 ADC2 Swap                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">9</span>       INT     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       DACR Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       ADC1 Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       ADC2 Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">7</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       HPOUT Gain                               <span class="hljs-number">7</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">16</span>      ENUM    <span class="hljs-number">1</span>       LINEINL Gain                             <span class="hljs-number">0</span>dB &gt;<span class="hljs-number">6</span>dB
<span class="hljs-number">17</span>      ENUM    <span class="hljs-number">1</span>       LINEINR Gain                             <span class="hljs-number">0</span>dB &gt;<span class="hljs-number">6</span>dB
<span class="hljs-number">18</span>      ENUM    <span class="hljs-number">1</span>       DAC Src <span class="hljs-keyword">Select</span>                           &gt;APB I2S
<span class="hljs-number">19</span>      ENUM    <span class="hljs-number">1</span>       ADC Dst <span class="hljs-keyword">Select</span>                           &gt;APB I2S
<span class="hljs-number">20</span>      ENUM    <span class="hljs-number">1</span>       I2S DAC OUT SEL                          &gt;<span class="hljs-keyword">Null</span> HPOUT SPK HPOUT-SPK
<span class="hljs-number">21</span>      BOOL    <span class="hljs-number">1</span>       LINEINL <span class="hljs-keyword">Switch</span>                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">22</span>      BOOL    <span class="hljs-number">1</span>       LINEINR <span class="hljs-keyword">Switch</span>                           <span class="hljs-keyword">Off</span>
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTL <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTR <span class="hljs-keyword">Switch</span>                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">25</span>      BOOL    <span class="hljs-number">1</span>       HPOUT <span class="hljs-keyword">Switch</span>                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">26</span>      BOOL    <span class="hljs-number">1</span>       SPK <span class="hljs-keyword">Switch</span>                               <span class="hljs-keyword">Off</span>
<span class="hljs-number">27</span>      BOOL    <span class="hljs-number">1</span>       LINEINL Mixer LINEINL1 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">28</span>      BOOL    <span class="hljs-number">1</span>       LINEINL Mixer LINEINL2 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">29</span>      BOOL    <span class="hljs-number">1</span>       LINEINR Mixer LINEINR1 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
<span class="hljs-number">30</span>      BOOL    <span class="hljs-number">1</span>       LINEINR Mixer LINEINR2 <span class="hljs-keyword">Switch</span>            <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>LINEINL 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Mixer LINEINL1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>LINEINL/R 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINL Mixer LINEINL1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEINR Mixer LINEINR1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUTL 单通道喇叭播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUTL/R 双通道喇叭播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>HPOUT 播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<h3 id="sun65iw1">sun65iw1</h3>
<h4 id="-">声卡控件</h4>
<p><strong>控件列表</strong></p>
<pre><code class="lang-bash">Mixer name: <span class="hljs-comment">'audiocodec'</span>
Number <span class="hljs-keyword">of</span> controls: <span class="hljs-number">25</span>
ctl     type    num     name                                     value
        range/values
<span class="hljs-number">0</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       tx hub mode                              &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">1</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       rx sync mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">2</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC DRC Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">3</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DAC HPF Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">4</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC DRC Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">5</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC HPF Mode                             &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">6</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       AD2DA <span class="hljs-keyword">Loop</span> Mode                          &gt;<span class="hljs-keyword">Off</span> ADC12-<span class="hljs-keyword">to</span>-DACLR
<span class="hljs-number">7</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DA2AD <span class="hljs-keyword">Loop</span> Mode                          &gt;<span class="hljs-keyword">Off</span> DACLR-<span class="hljs-keyword">to</span>-ADC12
<span class="hljs-number">8</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       DACL DACR Swap                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">9</span>       <span class="hljs-keyword">ENUM</span>    <span class="hljs-number">1</span>       ADC1 ADC2 Swap                           &gt;<span class="hljs-keyword">Off</span> <span class="hljs-keyword">On</span>
<span class="hljs-number">10</span>      INT     <span class="hljs-number">1</span>       DAC Volume                               <span class="hljs-number">63</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">63</span>)
<span class="hljs-number">11</span>      INT     <span class="hljs-number">1</span>       DACL Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">12</span>      INT     <span class="hljs-number">1</span>       DACR Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">13</span>      INT     <span class="hljs-number">1</span>       ADC1 Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">14</span>      INT     <span class="hljs-number">1</span>       ADC2 Volume                              <span class="hljs-number">160</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">255</span>)
<span class="hljs-number">15</span>      INT     <span class="hljs-number">1</span>       LINEOUT Gain                             <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">16</span>      INT     <span class="hljs-number">1</span>       HPOUT Gain                               <span class="hljs-number">7</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">7</span>)
<span class="hljs-number">17</span>      INT     <span class="hljs-number">1</span>       ADC1 Gain                                <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">18</span>      INT     <span class="hljs-number">1</span>       ADC2 Gain                                <span class="hljs-number">31</span> (dsrange <span class="hljs-number">0</span>-&gt;<span class="hljs-number">31</span>)
<span class="hljs-number">19</span>      BOOL    <span class="hljs-number">1</span>       MIC1 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">20</span>      BOOL    <span class="hljs-number">1</span>       MIC2 Switch                              <span class="hljs-keyword">Off</span>
<span class="hljs-number">21</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTL Switch                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">22</span>      BOOL    <span class="hljs-number">1</span>       LINEOUTR Switch                          <span class="hljs-keyword">Off</span>
<span class="hljs-number">23</span>      BOOL    <span class="hljs-number">1</span>       HPOUT Switch                             <span class="hljs-keyword">Off</span>
<span class="hljs-number">24</span>      BOOL    <span class="hljs-number">1</span>       SPK Switch                               <span class="hljs-keyword">Off</span>
</code></pre>
<h4 id="-">常用使用方法</h4>
<blockquote>
<ol>
<li>以 tinyalsa 工具举例说明，具体使用方法见 <a href="#tinyalsa工具">tinyalsa工具</a> 章节。</li>
<li>假设 audiocodec 声卡序号为 0。</li>
</ol>
</blockquote>
<p><strong>录音</strong></p>
<p>MIC1 单通道录音</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">1</span> -T <span class="hljs-number">10</span>
</code></pre>
<p>MIC1/2 双通道输入</p>
<pre><code class="lang-bash"># 录音通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC1 Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"MIC2 Switch"</span> <span class="hljs-number">1</span>
tinycap mic.wav -D <span class="hljs-number">0</span> -c <span class="hljs-number">2</span> -T <span class="hljs-number">10</span>
</code></pre>
<p><strong>播放</strong></p>
<p>LINEOUTL 单通道喇叭播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>LINEOUTL/R 双通道喇叭播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTL Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"LINEOUTR Switch"</span> <span class="hljs-number">1</span>
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"SPK Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
<p>HPOUT 播放</p>
<pre><code class="lang-bash"># 播放通路控件
tinymix -D <span class="hljs-number">0</span> <span class="hljs-string">"HPOUT Switch"</span> <span class="hljs-number">1</span>
tinyplay test_1ch.wav -D <span class="hljs-number">0</span>
</code></pre>
