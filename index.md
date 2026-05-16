<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Portfolio Performance Dashboard — May 2026</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f5f5f5;color:#1a1a1a;padding:24px}
  .page{max-width:1100px;margin:0 auto;background:#fff;border-radius:12px;padding:28px 32px;box-shadow:0 1px 6px rgba(0,0,0,.08)}

  /* HEADER */
  .hdr{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:24px;padding-bottom:16px;border-bottom:1px solid #e8e8e8}
  .htitle{font-size:20px;font-weight:600;color:#111;margin-bottom:4px}
  .hsub{font-size:13px;color:#666}
  .bdg{background:#fff3e0;color:#e65100;font-size:11px;font-weight:600;padding:4px 10px;border-radius:4px;display:inline-block}
  .bdg-sub{font-size:12px;color:#999;margin-top:6px;text-align:right}

  /* KPI CARDS */
  .kg{display:grid;grid-template-columns:repeat(5,1fr);gap:10px;margin-bottom:24px}
  .kc{background:#fafafa;border:1px solid #ececec;border-radius:8px;padding:12px 14px}
  .kl{font-size:10px;color:#888;text-transform:uppercase;letter-spacing:.6px;margin-bottom:6px}
  .kv{font-size:22px;font-weight:600;color:#111;margin-bottom:4px;line-height:1}
  .ka{font-size:12px;font-weight:600;color:#2e7d32}
  .kw{font-size:12px;font-weight:600;color:#c62828}
  .ks{font-size:11px;color:#aaa}

  /* SECTION LABEL */
  .sec{font-size:10px;font-weight:600;color:#888;text-transform:uppercase;letter-spacing:.6px;margin-bottom:10px}

  /* CHARTS */
  .cr{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:24px}
  .cb{background:#fff;border:1px solid #ececec;border-radius:10px;padding:16px}

  /* BENCHMARK */
  .bench{background:#fff;border:1px solid #ececec;border-radius:10px;padding:18px;margin-bottom:24px}
  .bench-desc{font-size:12px;color:#666;margin-bottom:14px}
  .bench-inner{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
  .bcard{border-radius:8px;padding:14px}
  .bcard-g{background:#f1f8f2;border:1px solid #c8e6c9}
  .bcard-b{background:#e8f1fb;border:1px solid #b3d1f5}
  .bcard-title{font-size:11px;color:#555;margin-bottom:8px;font-weight:500}
  .bcard-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:3px}
  .blbl{font-size:11px;color:#666}
  .bvp{font-size:17px;font-weight:700;color:#2e7d32}
  .bvn{font-size:17px;font-weight:700;color:#c62828}
  .bvb{font-size:17px;font-weight:700;color:#1565c0}
  .bdiv{display:flex;align-items:center;gap:6px;margin:4px 0}
  .bdiv-line{flex:1;height:1px;background:#ddd}
  .bdiv-txt{font-size:10px;color:#aaa}
  .bfoot{font-size:10px;color:#2e7d32;margin-top:8px;font-weight:600}
  .bfootb{font-size:10px;color:#1565c0;margin-top:8px;font-weight:600}
  .bench-note{font-size:10px;color:#aaa;margin-top:10px}

  /* AI SECTION */
  .ai-wrap{background:#fff;border:1px solid #ececec;border-radius:10px;padding:18px;margin-bottom:24px}
  .ai-desc{font-size:12px;color:#666;margin-bottom:14px}
  .ai-summary{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:16px}
  .ai-sum-card{background:#fafafa;border:1px solid #ececec;border-radius:8px;padding:12px;text-align:center}
  .ai-sum-val{font-size:22px;font-weight:700;color:#e65100;margin-bottom:4px}
  .ai-sum-lbl{font-size:11px;color:#888}
  .ai-tools{display:grid;grid-template-columns:1fr 1fr;gap:10px}
  .ai-tool{background:#fafafa;border:1px solid #ececec;border-radius:8px;padding:12px 14px;display:flex;gap:12px;align-items:flex-start}
  .ai-tool-icon{width:32px;height:32px;border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;margin-top:2px}
  .ic-orange{background:#fff3e0}
  .ic-blue{background:#e3f2fd}
  .ic-green{background:#e8f5e9}
  .ic-purple{background:#f3e5f5}
  .ai-tool-body{flex:1}
  .ai-tool-name{font-size:13px;font-weight:600;color:#111;margin-bottom:3px}
  .ai-tool-desc{font-size:11px;color:#666;margin-bottom:6px;line-height:1.5}
  .ai-tool-tag{display:inline-block;background:#fff3e0;color:#e65100;font-size:10px;font-weight:600;padding:2px 7px;border-radius:3px}
  .ai-tool-tag-b{background:#e3f2fd;color:#1565c0}
  .ai-tool-tag-g{background:#e8f5e9;color:#2e7d32}

  /* GGP TABLE */
  .tb{background:#fff;border:1px solid #ececec;border-radius:10px;overflow:hidden}
  table{width:100%;border-collapse:collapse;font-size:12.5px}
  thead tr{background:#fafafa}
  th{padding:8px 12px;text-align:left;font-weight:600;font-size:10px;color:#888;text-transform:uppercase;letter-spacing:.5px;white-space:nowrap;border-bottom:1px solid #ececec}
  td{padding:7px 12px;border-top:1px solid #f2f2f2;color:#1a1a1a}
  .nr{text-align:right}
  .pos{color:#2e7d32;font-weight:600}
  .neg{color:#c62828;font-weight:600}
  .ct{font-size:10px;padding:2px 7px;border-radius:3px;font-weight:600}
  .tls{background:#e3f2fd;color:#1565c0}
  .tel{background:#fff3e0;color:#e65100}
  .tfm{background:#e8f5e9;color:#2e7d32}
  .tfa{background:#f3e5f5;color:#6a1b9a}
  .foot{font-size:11px;color:#bbb;margin-top:8px;text-align:right}

  @media print{body{background:#fff;padding:0}.page{box-shadow:none;border-radius:0;padding:20px}}
</style>
</head>
<body>
<div class="page">

  <!-- HEADER -->
  <div class="hdr">
    <div>
      <div class="htitle">Portfolio Performance Dashboard</div>
      <div class="hsub">Trang Duong &nbsp;·&nbsp; Lifestyle &amp; ELHA &nbsp;·&nbsp; 13 GGP &nbsp;·&nbsp; 101 shops &nbsp;·&nbsp; Handover: March 2026</div>
    </div>
    <div>
      <span class="bdg">May 2026 MTD</span>
      <div class="bdg-sub">Data as of May 15, 2026</div>
    </div>
  </div>

  <!-- KPI CARDS -->
  <div class="kg">
    <div class="kc">
      <div class="kl">ADO</div>
      <div class="kv">22,716</div>
      <div class="ka">▲ 125.3% vs KPI</div>
      <div class="ks">+25.3% MoM &nbsp;·&nbsp; KPI: 18,130</div>
    </div>
    <div class="kc">
      <div class="kl">ADG</div>
      <div class="kv">$92,626</div>
      <div class="ka">▲ 122.1% vs KPI</div>
      <div class="ks">+22.1% MoM &nbsp;·&nbsp; KPI: $75,885</div>
    </div>
    <div class="kc">
      <div class="kl">Take Rate</div>
      <div class="kv">30.9%</div>
      <div class="ka">+8.6% MoM</div>
      <div class="ks">Seller investment ↑</div>
    </div>
    <div class="kc">
      <div class="kl">CIR</div>
      <div class="kv">25.2%</div>
      <div class="ka">+18.1% MoM</div>
      <div class="ks">Platform co-invest ↑</div>
    </div>
    <div class="kc">
      <div class="kl">PC2</div>
      <div class="kv">5.0%</div>
      <div class="kw">▼ −22.6% MoM</div>
      <div class="ks">Watch point</div>
    </div>
  </div>

  <!-- TREND CHARTS -->
  <div class="cr">
    <div class="cb">
      <div class="sec">ADO trend — Feb to May 2026</div>
      <div style="position:relative;height:190px">
        <canvas id="adoC" role="img" aria-label="ADO grew from 18338 in Feb to 22716 in May 2026. KPI line at 18130.">Feb 18338, Mar 18130, Apr 21408, May 22716</canvas>
      </div>
    </div>
    <div class="cb">
      <div class="sec">ADG trend — Feb to May 2026</div>
      <div style="position:relative;height:190px">
        <canvas id="adgC" role="img" aria-label="ADG grew from $74328 in Feb to $92626 in May 2026. KPI line at $75885.">Feb $74328, Mar $75885, Apr $86469, May $92626</canvas>
      </div>
    </div>
  </div>

  <!-- BENCHMARK -->
  <div class="bench">
    <div class="sec">Portfolio vs. Lifestyle cluster — growth benchmark</div>
    <div class="bench-desc">Trang's portfolio consistently outperformed the Lifestyle cluster since taking over in March 2026, growing above market while the cluster declined.</div>
    <div class="bench-inner">
      <div class="bcard bcard-g">
        <div class="bcard-title">April MoM — ADO growth</div>
        <div class="bcard-row"><span class="blbl">Trang portfolio</span><span class="bvp">+18.1%</span></div>
        <div class="bdiv"><div class="bdiv-line"></div><div class="bdiv-txt">vs</div><div class="bdiv-line"></div></div>
        <div class="bcard-row"><span class="blbl">Lifestyle cluster</span><span class="bvn">−7.8%</span></div>
        <div class="bfoot">+25.9 pp outperformance</div>
      </div>
      <div class="bcard bcard-g">
        <div class="bcard-title">April MoM — ADG growth</div>
        <div class="bcard-row"><span class="blbl">Trang portfolio</span><span class="bvp">+13.9%</span></div>
        <div class="bdiv"><div class="bdiv-line"></div><div class="bdiv-txt">vs</div><div class="bdiv-line"></div></div>
        <div class="bcard-row"><span class="blbl">Lifestyle cluster</span><span class="bvn">−8.2%</span></div>
        <div class="bfoot">+22.1 pp outperformance</div>
      </div>
      <div class="bcard bcard-b">
        <div class="bcard-title">KPI achievement rate — May MTD</div>
        <div class="bcard-row"><span class="blbl">Trang portfolio</span><span class="bvp">125.3%</span></div>
        <div class="bdiv"><div class="bdiv-line"></div><div class="bdiv-txt">vs</div><div class="bdiv-line"></div></div>
        <div class="bcard-row"><span class="blbl">Team overall</span><span class="bvb">115.4%</span></div>
        <div class="bfootb">+9.9 pp above team RR</div>
      </div>
    </div>
    <div class="bench-note">Lifestyle cluster Apr data: ADO 61,088 (Mar: 66,241) &nbsp;·&nbsp; Team KPI RR from cluster tracker &nbsp;·&nbsp; Portfolio managed independently since March 2026</div>
  </div>

  <!-- AI AUTOMATION -->
  <div class="ai-wrap">
    <div class="sec">AI-powered workflow automation — time efficiency</div>
    <div class="ai-desc">6 automated tools independently built and deployed since March 2026, saving ~10–12 hours/week of manual execution (~60% of routine workload). Tools also shared with 2 teammates for broader team efficiency.</div>
    <div class="ai-summary">
      <div class="ai-sum-card">
        <div class="ai-sum-val">6</div>
        <div class="ai-sum-lbl">Automation tools built</div>
      </div>
      <div class="ai-sum-card">
        <div class="ai-sum-val">~11h</div>
        <div class="ai-sum-lbl">Hours saved per week</div>
      </div>
      <div class="ai-sum-card">
        <div class="ai-sum-val">~60%</div>
        <div class="ai-sum-lbl">Routine workload automated</div>
      </div>
    </div>
    <div class="ai-tools">
      <div class="ai-tool">
        <div class="ai-tool-icon ic-orange">📊</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">KOL Livestream Tracker</div>
          <div class="ai-tool-desc">Auto-tracks monthly Top KOL pick results, diffs against previous snapshots, auto-creates Google Calendar events per KOL/date/brand, auto-removes dropped sellers.</div>
          <span class="ai-tool-tag">~3h saved/week</span>
        </div>
      </div>
      <div class="ai-tool">
        <div class="ai-tool-icon ic-blue">⚡</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">SSC Flash Sale Filter</div>
          <div class="ai-tool-desc">Processes CFS ClusterDB files into tiered eligibility sheets (Tier 29k / 9k / 1k) automatically. Outputs 4-sheet workbook ready for submission per campaign cycle.</div>
          <span class="ai-tool-tag ai-tool-tag-b">~2–3h saved/campaign</span>
        </div>
      </div>
      <div class="ai-tool">
        <div class="ai-tool-icon ic-green">📋</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">MCN Final Deal Distributor</div>
          <div class="ai-tool-desc">Distributes final deal data from master source sheet across 5 GGP ELHA accounts via Apps Script automatically. Replaces manual monthly copy-paste distribution.</div>
          <span class="ai-tool-tag ai-tool-tag-g">~2h saved/month</span>
        </div>
      </div>
      <div class="ai-tool">
        <div class="ai-tool-icon ic-orange">🎯</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">Visibility Package Workflow</div>
          <div class="ai-tool-desc">Auto-aggregates seller data across GGP accounts, validates eligibility, and distributes package submissions with built-in error checking. Covers 3-phase workflow end-to-end.</div>
          <span class="ai-tool-tag">~4–5h saved/month</span>
        </div>
      </div>
      <div class="ai-tool">
        <div class="ai-tool-icon ic-purple">📈</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">Livestream PFM Dashboard</div>
          <div class="ai-tool-desc">HTML dashboard visualizing KOL performance analytics by date, cluster, and brand. Includes month tabs, multi-select filters, and calendar events. Used in weekly reviews and seller briefings.</div>
          <span class="ai-tool-tag ai-tool-tag-b">~3h saved/week</span>
        </div>
      </div>
      <div class="ai-tool">
        <div class="ai-tool-icon ic-green">🔄</div>
        <div class="ai-tool-body">
          <div class="ai-tool-name">Daily Task Automations</div>
          <div class="ai-tool-desc">Auto-aggregates GGP sheets, splits data pools (CFS pool, SSC pool, livestream pool), and triggers Google Calendar reminders for deadlines and follow-up actions across all 13 GGPs.</div>
          <span class="ai-tool-tag ai-tool-tag-g">~2h saved/week</span>
        </div>
      </div>
    </div>
  </div>

  <!-- GGP TABLE -->
  <div class="sec">GGP breakdown — May 2026 MTD</div>
  <div class="tb">
    <table>
      <thead>
        <tr>
          <th>GGP</th>
          <th>Cluster</th>
          <th class="nr">ADO</th>
          <th class="nr">MoM</th>
          <th class="nr">ADG ($)</th>
          <th class="nr">MoM</th>
          <th class="nr">Take Rate</th>
          <th class="nr">PC2</th>
        </tr>
      </thead>
      <tbody id="tb"></tbody>
    </table>
  </div>
  <div class="foot">KPI: ADO 18,130 &nbsp;·&nbsp; ADG $75,885 &nbsp;·&nbsp; Source: RM Performance Tracker &nbsp;·&nbsp; Modisolar: P2 &nbsp;·&nbsp; May 15, 2026</div>

</div><!-- end .page -->

<script>
const gc='rgba(0,0,0,0.05)';
const tc='#aaa';
const bo={responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{mode:'index',intersect:false}},scales:{x:{grid:{color:gc},ticks:{color:tc,font:{size:10}}},y:{grid:{color:gc},ticks:{color:tc,font:{size:10}}}}};

new Chart(document.getElementById('adoC'),{
  type:'bar',
  data:{
    labels:['Feb','Mar (Handover)','Apr','May MTD'],
    datasets:[
      {label:'ADO',data:[18338,18130,21408,22716],backgroundColor:['rgba(230,81,0,0.2)','rgba(230,81,0,0.2)','rgba(230,81,0,0.5)','#e65100'],borderRadius:4},
      {label:'KPI',data:[18130,18130,18130,18130],type:'line',borderColor:'#1565c0',borderDash:[5,3],borderWidth:1.5,pointRadius:0,fill:false}
    ]
  },
  options:{...bo,scales:{...bo.scales,y:{...bo.scales.y,min:15000,ticks:{color:tc,font:{size:10},callback:v=>v.toLocaleString()}}}}
});

new Chart(document.getElementById('adgC'),{
  type:'bar',
  data:{
    labels:['Feb','Mar (Handover)','Apr','May MTD'],
    datasets:[
      {label:'ADG',data:[74328,75885,86469,92626],backgroundColor:['rgba(21,101,192,0.18)','rgba(21,101,192,0.18)','rgba(21,101,192,0.45)','#1565c0'],borderRadius:4},
      {label:'KPI',data:[75885,75885,75885,75885],type:'line',borderColor:'#e65100',borderDash:[5,3],borderWidth:1.5,pointRadius:0,fill:false}
    ]
  },
  options:{...bo,scales:{...bo.scales,y:{...bo.scales.y,min:60000,ticks:{color:tc,font:{size:10},callback:v=>'$'+Math.round(v/1000)+'k'}}}}
});

const ggps=[
  {n:'SHAOHUI',cl:'Lifestyle',ado:15687,am:25.8,adg:27797,gm:17.8,tr:41.7,pc2:11.3},
  {n:'ICON-MALL',cl:'Lifestyle',ado:2730,am:34.4,adg:12621,gm:24.9,tr:27.7,pc2:3.1},
  {n:'Smart Link',cl:'ELHA',ado:534,am:46.7,adg:21748,gm:46.5,tr:22.5,pc2:-0.3},
  {n:'HEMEI',cl:'FMCG',ado:1066,am:30.0,adg:2959,gm:29.9,tr:38.2,pc2:4.5},
  {n:'KINGESS',cl:'Lifestyle',ado:649,am:38.6,adg:2860,gm:27.5,tr:36.9,pc2:-8.5},
  {n:'C&M',cl:'Lifestyle',ado:862,am:9.9,adg:3999,gm:1.6,tr:27.2,pc2:4.9},
  {n:'Medoga VN',cl:'Lifestyle',ado:148,am:25.9,adg:5030,gm:24.2,tr:30.9,pc2:4.0},
  {n:'E-Ride Việt',cl:'Fashion',ado:43,am:9.2,adg:5820,gm:-1.9,tr:11.1,pc2:-0.2},
  {n:'We-Tok',cl:'FMCG',ado:168,am:11.4,adg:499,gm:-21.6,tr:38.7,pc2:14.5},
  {n:'凡宸',cl:'Lifestyle',ado:152,am:14.9,adg:1331,gm:4.6,tr:48.0,pc2:24.4},
  {n:'Modisolar (P2)',cl:'Lifestyle',ado:100,am:5.9,adg:2062,gm:22.3,tr:41.9,pc2:14.9},
  {n:'DELIXI',cl:'Lifestyle',ado:286,am:-19.9,adg:1994,gm:-18.0,tr:34.5,pc2:8.5},
  {n:'Foei Chi',cl:'Lifestyle',ado:285,am:-4.5,adg:1995,gm:2.2,tr:28.1,pc2:2.3},
  {n:'MOVA',cl:'ELHA',ado:6,am:121.2,adg:1911,gm:109.1,tr:10.6,pc2:0.6},
];
const cm={Lifestyle:'tls',ELHA:'tel',FMCG:'tfm',Fashion:'tfa'};
const fmt=v=>`<span class="${v>0?'pos':v<0?'neg':''}">${v>0?'+':''}${v.toFixed(1)}%</span>`;
const tb=document.getElementById('tb');
ggps.forEach(g=>{
  const tr=document.createElement('tr');
  tr.innerHTML=`<td style="font-weight:600">${g.n}</td><td><span class="ct ${cm[g.cl]||'tls'}">${g.cl}</span></td><td class="nr">${g.ado.toLocaleString()}</td><td class="nr">${fmt(g.am)}</td><td class="nr">$${g.adg.toLocaleString()}</td><td class="nr">${fmt(g.gm)}</td><td class="nr">${g.tr.toFixed(1)}%</td><td class="nr ${g.pc2<0?'neg':g.pc2>5?'pos':''}">${g.pc2.toFixed(1)}%</td>`;
  tb.appendChild(tr);
});
</script>
</body>
</html>
