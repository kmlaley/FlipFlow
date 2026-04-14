/**
 * ╔══════════════════════════════════════════════════════════════╗
 * ║          FlipCalc Pro — Launch-Ready v3.0 (2026)            ║
 * ║          Single-file PWA React Component                     ║
 * ╚══════════════════════════════════════════════════════════════╝
 *
 * ── PWA META TAGS (paste into <head> of index.html / layout.tsx) ──
 *
 *  <meta name="theme-color" content="#0a0c12" />
 *  <meta name="apple-mobile-web-app-capable" content="yes" />
 *  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
 *  <meta name="apple-mobile-web-app-title" content="FlipCalc Pro" />
 *  <meta name="mobile-web-app-capable" content="yes" />
 *  <meta name="application-name" content="FlipCalc Pro" />
 *  <meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover" />
 *  <link rel="manifest" href="/manifest.json" />
 *  <link rel="apple-touch-icon" href="/icon-192.png" />
 *
 * ── manifest.json ─────────────────────────────────────────────
 *  {
 *    "name": "FlipCalc Pro",
 *    "short_name": "FlipCalc",
 *    "start_url": "/",
 *    "display": "standalone",
 *    "orientation": "portrait",
 *    "background_color": "#0a0c12",
 *    "theme_color": "#0a0c12",
 *    "icons": [
 *      { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
 *      { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
 *    ]
 *  }
 *
 * ── INSTALL DEPS ──────────────────────────────────────────────
 *  npx create-next-app@latest flipcalc-pro --typescript --tailwind --app
 *  cd flipcalc-pro && npm run dev
 *  (No extra packages needed — pure React)
 */

import { useState, useMemo, useEffect, useCallback, useRef } from "react";

// ════════════════════════════════════════════════════════════════
// §1  2026 FEE ENGINE
// ════════════════════════════════════════════════════════════════

/**
 * Platform registry.
 * isPro:true  → locked behind paywall
 * categories  → array of {id, label} for category picker (Pro only)
 */
const PLATFORM_META = {
  eBay: {
    label: "eBay", isPro: false,
    categories: [
      { id: "general",     label: "General (13.6%)" },
      { id: "sneakers",    label: "Sneakers $150+ (8%)" },
      { id: "electronics", label: "Electronics (9%)" },
    ],
  },
  TikTokShop: {
    label: "TikTok Shop", isPro: true,
    categories: [
      { id: "general",      label: "General (6%)" },
      { id: "collectibles", label: "Collectibles (6% + 3% >$10k)" },
    ],
  },
  Whatnot: {
    label: "Whatnot", isPro: true,
    categories: [
      { id: "general",   label: "General (8% + 2.9% + $0.30)" },
      { id: "highvalue", label: "High-Value Promo (0% >$1,500)" },
    ],
  },
  Depop:    { label: "Depop",    isPro: true,  categories: [] },
  Mercari:  { label: "Mercari",  isPro: false, categories: [] },
  Facebook: { label: "Facebook", isPro: false, categories: [] },
};

/** Master 2026 fee calculator — returns { platformCut, feeBreakdown } */
function computePlatformFees({ platformKey, category, sellPrice, topRated }) {
  const sell = parseFloat(sellPrice) || 0;
  let cut = 0;
  const lines = []; // [{label, amount}] for breakdown tooltip

  switch (platformKey) {

    // ── eBay ────────────────────────────────────────────────────
    case "eBay": {
      const perOrder = sell >= 10 ? 0.40 : 0.30;
      if (category === "sneakers" && sell >= 150) {
        // Sneakers $150+: 8%, no per-order fee
        const pct = sell * 0.08;
        cut = pct;
        lines.push({ label: "Sneakers rate 8%", amount: pct });
      } else {
        const rate = category === "electronics" ? 0.09 : 0.136;
        const pct  = sell * rate;
        const trsDiscount = topRated ? pct * 0.10 : 0;
        const afterTRS = pct - trsDiscount;
        cut = afterTRS + perOrder;
        lines.push({ label: `Category ${(rate*100).toFixed(1)}%`, amount: pct });
        if (topRated) lines.push({ label: "TRS −10%", amount: -trsDiscount });
        lines.push({ label: "Per-order fee", amount: perOrder });
      }
      break;
    }

    // ── TikTok Shop ─────────────────────────────────────────────
    case "TikTokShop": {
      const base = sell * 0.06;
      const processing = 0.30;
      let collectibleExtra = 0;
      if (category === "collectibles" && sell > 10000) {
        collectibleExtra = (sell - 10000) * 0.03;
        lines.push({ label: "Collectibles surcharge 3% (>$10k)", amount: collectibleExtra });
      }
      cut = base + processing + collectibleExtra;
      lines.push({ label: "Base commission 6%", amount: base });
      lines.push({ label: "Processing $0.30",   amount: processing });
      break;
    }

    // ── Whatnot ─────────────────────────────────────────────────
    case "Whatnot": {
      if (category === "highvalue" && sell > 1500) {
        // 0% on portion above $1,500
        const basePortion    = Math.min(sell, 1500);
        const promoPortions  = sell - 1500;
        const commissionBase = basePortion * 0.08;
        const processing     = sell * 0.029 + 0.30;
        cut = commissionBase + processing;
        lines.push({ label: "Commission 8% (≤$1,500)",      amount: commissionBase });
        lines.push({ label: `0% commission (>$1,500: +${fmt(promoPortions)})`, amount: 0 });
        lines.push({ label: "Processing 2.9% + $0.30",      amount: processing });
      } else {
        const commission = sell * 0.08;
        const processing = sell * 0.029 + 0.30;
        cut = commission + processing;
        lines.push({ label: "Commission 8%",          amount: commission });
        lines.push({ label: "Processing 2.9% + $0.30", amount: processing });
      }
      break;
    }

    // ── Depop ───────────────────────────────────────────────────
    case "Depop": {
      // Marketplace fee: 0%. Mandatory processing: 3.3% + $0.45
      const processing = sell * 0.033 + 0.45;
      cut = processing;
      lines.push({ label: "Marketplace fee 0%",         amount: 0 });
      lines.push({ label: "Processing 3.3% + $0.45",    amount: processing });
      break;
    }

    // ── Mercari ─────────────────────────────────────────────────
    case "Mercari": {
      // 10% on (price + shipping) — pass shippingCost via sellPrice proxy
      // Caller passes sell = price only; shipping handled separately
      const fee = sell * 0.10;
      cut = fee;
      lines.push({ label: "Flat 10% (price + shipping)", amount: fee });
      break;
    }

    // ── Facebook ────────────────────────────────────────────────
    case "Facebook": {
      const fee = sell * 0.05;
      cut = fee;
      lines.push({ label: "Marketplace fee 5%", amount: fee });
      break;
    }

    default: cut = 0;
  }

  return { platformCut: Math.max(0, cut), feeBreakdown: lines };
}

/** Main profit calculator */
function calcProfit(item) {
  const {
    buyPrice = 0, sellPrice = 0, shippingCost = 0,
    extraFees = 0, salesTax = false,
    platformKey = "eBay", category = "general",
    topRated = false, shippingSubsidy = 0,
  } = item;

  const buy   = parseFloat(buyPrice)      || 0;
  const sell  = parseFloat(sellPrice)     || 0;
  const ship  = parseFloat(shippingCost)  || 0;
  const extra = parseFloat(extraFees)     || 0;
  const sub   = parseFloat(shippingSubsidy) || 0;

  // Mercari: fee applies to sell + shipping
  const feeBase = platformKey === "Mercari" ? sell + ship : sell;
  const { platformCut, feeBreakdown } = computePlatformFees({
    platformKey, category, sellPrice: feeBase, topRated,
  });

  const taxCost       = salesTax ? buy * 0.07 : 0;
  const netShipping   = Math.max(0, ship - sub);
  const totalCost     = buy + netShipping + extra + platformCut + taxCost;
  const netProfit     = sell - totalCost;
  const roi           = buy > 0 ? (netProfit / buy) * 100 : 0;
  const margin        = sell > 0 ? (netProfit / sell) * 100 : 0;

  let verdict = "PASS", vc = "red";
  if (roi >= 30 && netProfit >= 10) { verdict = "BUY";   vc = "green";  }
  else if (roi >= 15 && netProfit >= 5) { verdict = "MAYBE"; vc = "yellow"; }

  return {
    netProfit, roi, margin, platformCut, feeBreakdown,
    totalCost, taxCost, breakEven: totalCost, verdict, verdictColor: vc,
  };
}

// ════════════════════════════════════════════════════════════════
// §2  CONSTANTS & HELPERS
// ════════════════════════════════════════════════════════════════

const fmt  = (n) => `$${Math.abs(n).toFixed(2)}`;
const fmtP = (n) => `${n < 0 ? "−" : ""}${Math.abs(n).toFixed(1)}%`;

let _uid = 1;
const newItem = () => ({
  id: _uid++, name: "", buyPrice: "", sellPrice: "", shippingCost: "",
  platformKey: "eBay", category: "general", extraFees: "", note: "",
  topRated: false, salesTax: false, shippingSubsidy: "",
});

const FREE_PLATFORMS  = ["eBay", "Mercari", "Facebook"];
const FREE_HISTORY_LIMIT = 3;

// ════════════════════════════════════════════════════════════════
// §3  DESIGN SYSTEM
// ════════════════════════════════════════════════════════════════

const C = {
  page:     "#0a0c12",
  glass:    "rgba(255,255,255,0.04)",
  glassMid: "rgba(255,255,255,0.06)",
  glassHi:  "rgba(255,255,255,0.09)",
  border:   "rgba(255,255,255,0.09)",
  borderHi: "rgba(255,255,255,0.16)",
  t1: "#f0f2f7",   // primary text
  t2: "#8892a4",   // secondary
  t3: "#4e5a6e",   // muted
  green:      "#22c55e",
  greenDim:   "rgba(34,197,94,0.11)",
  greenGlow:  "rgba(34,197,94,0.22)",
  greenBorder:"rgba(34,197,94,0.35)",
  greenText:  "#4ade80",
  red:        "#9b1c4a",
  redDim:     "rgba(155,28,74,0.14)",
  redGlow:    "rgba(155,28,74,0.28)",
  redBorder:  "rgba(155,28,74,0.4)",
  redText:    "#fb7185",
  amber:      "#f59e0b",
  amberDim:   "rgba(245,158,11,0.11)",
  amberGlow:  "rgba(245,158,11,0.2)",
  amberBorder:"rgba(245,158,11,0.35)",
  amberText:  "#fcd34d",
  blue:       "#3b82f6",
  blueDim:    "rgba(59,130,246,0.12)",
  blueText:   "#93c5fd",
  gold:       "#f59e0b",
  goldText:   "#fbbf24",
  goldDim:    "rgba(251,191,36,0.1)",
  goldBorder: "rgba(251,191,36,0.3)",
};

const VT = {
  green:  { bg:C.greenDim,  border:C.greenBorder,  text:C.greenText,  glow:C.greenGlow,  emoji:"🔥", label:"BUY"   },
  yellow: { bg:C.amberDim,  border:C.amberBorder,  text:C.amberText,  glow:C.amberGlow,  emoji:"🤔", label:"MAYBE" },
  red:    { bg:C.redDim,    border:C.redBorder,     text:C.redText,    glow:C.redGlow,    emoji:"❌", label:"PASS"  },
};

// ════════════════════════════════════════════════════════════════
// §4  GLOBAL CSS
// ════════════════════════════════════════════════════════════════

const CSS = `
  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700;0,9..40,800;0,9..40,900;1,9..40,400&family=DM+Mono:wght@400;500&display=swap');

  *, *::before, *::after {
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    -webkit-font-smoothing: antialiased;
  }

  body { margin: 0; background: #0a0c12; }

  /* ── Keyframes ── */
  @keyframes shimmer-move {
    0%   { background-position: -200% center; }
    100% { background-position:  200% center; }
  }
  @keyframes fade-up {
    from { opacity:0; transform:translateY(12px); }
    to   { opacity:1; transform:translateY(0);    }
  }
  @keyframes spin-pop {
    0%   { opacity:0; transform:scale(0.6) rotate(-8deg); }
    60%  { transform:scale(1.12) rotate(3deg); }
    100% { opacity:1; transform:scale(1) rotate(0deg); }
  }
  @keyframes float {
    0%,100% { transform:translateY(0);    }
    50%     { transform:translateY(-5px); }
  }
  @keyframes scan-pulse {
    0%,100% { opacity:0.06; }
    50%     { opacity:0.18; }
  }
  @keyframes modal-in {
    from { opacity:0; transform:scale(0.92) translateY(20px); }
    to   { opacity:1; transform:scale(1)    translateY(0);    }
  }
  @keyframes ring-out {
    0%   { box-shadow:0 0 0 0   rgba(34,197,94,0.5); }
    100% { box-shadow:0 0 0 16px rgba(34,197,94,0);   }
  }

  /* ── Utility classes ── */
  .fade-up  { animation: fade-up  0.28s ease both; }
  .spin-pop { animation: spin-pop 0.38s cubic-bezier(.34,1.56,.64,1) both; }
  .float    { animation: float    3.2s  ease-in-out infinite; }

  .btn-press { transition:transform .12s ease, box-shadow .12s ease; }
  .btn-press:active { transform:scale(0.94); }

  .ring-out { animation: ring-out .5s ease forwards; }

  /* Glass card */
  .gc {
    background: rgba(255,255,255,0.04);
    border:     1px solid rgba(255,255,255,0.09);
    backdrop-filter: blur(18px);
    -webkit-backdrop-filter: blur(18px);
  }

  /* Inputs */
  .gi {
    background: rgba(255,255,255,0.05);
    border:     1.5px solid rgba(255,255,255,0.10);
    color:      #f0f2f7;
    outline:    none;
    font-family:'DM Sans',system-ui,sans-serif;
    transition: border-color .2s, box-shadow .2s, background .2s;
    width:100%; box-sizing:border-box;
  }
  .gi:focus {
    background:   rgba(255,255,255,0.08);
    border-color: rgba(34,197,94,0.55);
    box-shadow:   0 0 0 3px rgba(34,197,94,0.13);
  }
  .gi::placeholder { color:rgba(136,146,164,0.45); }
  .gi-sell {
    border-color: rgba(34,197,94,0.38) !important;
  }
  .gi-sell:focus {
    border-color: rgba(34,197,94,0.7) !important;
    box-shadow:   0 0 0 3px rgba(34,197,94,0.18) !important;
  }

  /* Shimmer text */
  .sh-green {
    background: linear-gradient(90deg,#4ade80,#22c55e,#86efac,#22c55e,#4ade80);
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer-move 2s linear infinite;
  }
  .sh-red {
    background: linear-gradient(90deg,#fb7185,#9b1c4a,#f43f5e,#9b1c4a,#fb7185);
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer-move 2s linear infinite;
  }
  .sh-amber {
    background: linear-gradient(90deg,#fcd34d,#f59e0b,#fde68a,#f59e0b,#fcd34d);
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer-move 2s linear infinite;
  }

  /* Scanning overlay when calculating */
  .scan-overlay::after {
    content:'';
    position:absolute; inset:0; border-radius:inherit; pointer-events:none;
    background: repeating-linear-gradient(
      to bottom,
      rgba(34,197,94,0.04) 0px,
      rgba(34,197,94,0.04) 1px,
      transparent 1px,
      transparent 6px
    );
    animation: scan-pulse 1s ease-in-out infinite;
  }

  /* Scrollbar hide */
  ::-webkit-scrollbar { width:0; height:0; }

  /* Spinner inputs */
  input[type=number]::-webkit-inner-spin-button,
  input[type=number]::-webkit-outer-spin-button { -webkit-appearance:none; }
  input[type=number] { -moz-appearance:textfield; }

  /* Toggle */
  .tog { transition: background .22s; }
  .tog-thumb { transition: left .22s cubic-bezier(.34,1.56,.64,1); }

  /* Select */
  select.gi { appearance:none; cursor:pointer; }

  /* Modal backdrop */
  .modal-bg {
    position:fixed; inset:0; z-index:999;
    background:rgba(0,0,0,0.72);
    backdrop-filter:blur(8px);
    display:flex; align-items:center; justify-content:center;
    padding:24px;
    animation: fade-up .2s ease both;
  }
  .modal-box {
    animation: modal-in .3s cubic-bezier(.34,1.2,.64,1) both;
  }

  /* Pro badge */
  .pro-badge {
    font-size:9px; font-weight:800; padding:1px 5px; border-radius:4px;
    background:rgba(251,191,36,0.18); border:1px solid rgba(251,191,36,0.4);
    color:#fbbf24; vertical-align:middle; margin-left:4px; letter-spacing:.04em;
  }

  /* Blurred history card */
  .blurred { filter:blur(5px); pointer-events:none; user-select:none; }

  /* Nav button */
  .nav-b { transition: color .18s; }
  .nav-b:active { transform:scale(0.88); }
`;

// ════════════════════════════════════════════════════════════════
// §5  SMALL COMPONENTS
// ════════════════════════════════════════════════════════════════

function Toggle({ on, onChange, label, sub, locked, onLockClick }) {
  return (
    <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between",
      padding:"11px 0", borderBottom:`1px solid ${C.border}` }}>
      <div>
        <span style={{ fontSize:14, fontWeight:600, color: locked ? C.t3 : C.t1 }}>{label}</span>
        {locked && <span className="pro-badge">PRO</span>}
        {sub && <div style={{ fontSize:12, color:C.t3, marginTop:1 }}>{sub}</div>}
      </div>
      <div className="tog btn-press"
        onClick={locked ? onLockClick : () => onChange(!on)}
        style={{ width:44, height:24, borderRadius:12, position:"relative", cursor:"pointer",
          background: locked ? "rgba(255,255,255,0.06)" : on ? C.green : "rgba(255,255,255,0.08)",
          border:`1.5px solid ${locked ? C.border : on ? C.greenBorder : C.border}`,
          flexShrink:0 }}>
        <div className="tog-thumb"
          style={{ width:18, height:18, borderRadius:"50%", background: locked ? C.t3 : "#fff",
            position:"absolute", top:1, left: on && !locked ? 22 : 2,
            boxShadow:"0 1px 4px rgba(0,0,0,0.5)" }} />
      </div>
    </div>
  );
}

/** Pro upgrade popup */
function ProModal({ onClose }) {
  return (
    <div className="modal-bg" onClick={onClose}>
      <div className="gc modal-box"
        style={{ borderRadius:20, padding:28, maxWidth:340, width:"100%",
          border:`1px solid ${C.goldBorder}`,
          background:"linear-gradient(135deg,rgba(251,191,36,0.07),rgba(10,12,18,0.95))" }}
        onClick={e => e.stopPropagation()}>
        <div style={{ fontSize:48, textAlign:"center", marginBottom:12 }}>⭐</div>
        <h2 style={{ margin:"0 0 8px", fontSize:22, fontWeight:900, textAlign:"center", color:C.t1 }}>
          Upgrade to Pro
        </h2>
        <p style={{ margin:"0 0 20px", fontSize:14, color:C.t2, textAlign:"center", lineHeight:1.6 }}>
          Unlock TikTok Shop, Whatnot, Depop, category math, TRS discounts, Sales Tax, and full history.
        </p>
        {[
          "⚡ TikTok Shop, Whatnot & Depop fees",
          "📂 Sneakers & Electronics category math",
          "🏅 eBay Top Rated Seller discount",
          "🧾 Sales Tax toggle (auto 7%)",
          "📊 Full flip history + Stats dashboard",
          "📤 CSV export & Tax report generation",
        ].map(f => (
          <div key={f} style={{ display:"flex", alignItems:"center", gap:10,
            fontSize:13, color:C.t2, marginBottom:9 }}>
            <span style={{ color:C.greenText, fontSize:16 }}>✓</span> {f}
          </div>
        ))}
        <button className="btn-press"
          style={{ width:"100%", marginTop:18, padding:"14px 0", borderRadius:12,
            background:`linear-gradient(135deg,${C.gold},#b45309)`,
            color:"#000", fontWeight:900, fontSize:16, border:"none",
            cursor:"pointer", fontFamily:"inherit",
            boxShadow:"0 4px 24px rgba(245,158,11,0.35)" }}
          onClick={() => alert("Stripe checkout coming soon! 🚀")}>
          ✨ Start Pro — $4.99/mo
        </button>
        <div style={{ fontSize:12, color:C.t3, textAlign:"center", marginTop:10 }}>
          Cancel anytime · Instant access · 7-day free trial
        </div>
      </div>
    </div>
  );
}

/** Fee Breakdown tooltip card */
function FeeBreakdown({ lines }) {
  if (!lines?.length) return null;
  return (
    <div className="gc fade-up"
      style={{ borderRadius:10, padding:"10px 14px", marginTop:10 }}>
      <div style={{ fontSize:11, fontWeight:700, color:C.t3, textTransform:"uppercase",
        letterSpacing:".08em", marginBottom:7 }}>Fee Breakdown</div>
      {lines.map((l, i) => (
        <div key={i} style={{ display:"flex", justifyContent:"space-between",
          fontSize:12, color:l.amount < 0 ? C.greenText : C.t2,
          paddingBottom:4, marginBottom:4,
          borderBottom: i < lines.length-1 ? `1px solid ${C.border}` : "none" }}>
          <span>{l.label}</span>
          <span style={{ fontFamily:"'DM Mono',monospace", fontWeight:600 }}>
            {l.amount === 0 ? "—" : l.amount < 0 ? `−${fmt(-l.amount)}` : fmt(l.amount)}
          </span>
        </div>
      ))}
    </div>
  );
}

// ════════════════════════════════════════════════════════════════
// §6  HISTORY CARD
// ════════════════════════════════════════════════════════════════

function HistoryCard({ h, blurred }) {
  const vt = VT[h.verdictColor] || VT.red;
  return (
    <div className={`gc fade-up${blurred ? " blurred" : ""}`}
      style={{ borderRadius:14, padding:16, marginBottom:10 }}>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:12 }}>
        <div style={{ flex:1, paddingRight:12 }}>
          <div style={{ fontWeight:700, fontSize:15, color:C.t1 }}>{h.name||"Unnamed Item"}</div>
          <div style={{ fontSize:12, color:C.t3, marginTop:2, fontFamily:"'DM Mono',monospace" }}>
            {h.savedAt} · {PLATFORM_META[h.platformKey]?.label || h.platformKey}
            {h.category && h.category !== "general" &&
              <span style={{ color:C.t3 }}> · {h.category}</span>}
          </div>
        </div>
        <span style={{ fontSize:12, fontWeight:700, padding:"3px 11px", borderRadius:999,
          background:vt.bg, border:`1px solid ${vt.border}`, color:vt.text, flexShrink:0 }}>
          {vt.emoji} {vt.label}
        </span>
      </div>
      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr 1fr", gap:7 }}>
        {[
          ["Buy",    fmt(parseFloat(h.buyPrice)||0),  C.t1],
          ["Sell",   fmt(parseFloat(h.sellPrice)||0), C.t1],
          ["Profit", `${h.netProfit>=0?"+":"−"}${fmt(h.netProfit)}`,
                     h.netProfit>=0 ? C.greenText : C.redText],
        ].map(([lbl, val, color]) => (
          <div key={lbl} style={{ background:"rgba(0,0,0,0.28)", borderRadius:8,
            padding:"9px 4px", textAlign:"center" }}>
            <div style={{ fontSize:10, fontWeight:700, color:C.t3, textTransform:"uppercase",
              letterSpacing:".07em", marginBottom:4 }}>{lbl}</div>
            <div style={{ fontSize:15, fontWeight:800, color,
              fontFamily:"'DM Mono',monospace" }}>{val}</div>
          </div>
        ))}
      </div>
      {h.note && <div style={{ fontSize:12, color:C.t3, marginTop:9, fontStyle:"italic" }}>"{h.note}"</div>}
    </div>
  );
}

// ════════════════════════════════════════════════════════════════
// §7  CALCULATOR VIEW
// ════════════════════════════════════════════════════════════════

function CalculatorView({ items, activeId, setActiveId, addItem, removeItem,
  update, saveToHistory, active, result, isCalc, isPro, onProClick }) {

  const vt       = VT[result.verdictColor];
  const pmeta    = PLATFORM_META[active?.platformKey] || PLATFORM_META.eBay;
  const hasCats  = isPro && pmeta.categories?.length > 0;
  const [showBreakdown, setShowBreakdown] = useState(false);

  const IS = { borderRadius:10, padding:"12px 14px", fontSize:15 };

  const LBL = ({ children, color, pro }) => (
    <div style={{ fontSize:12, fontWeight:700, color: color || C.t2, marginBottom:6,
      textTransform:"uppercase", letterSpacing:".06em" }}>
      {children}{pro && <span className="pro-badge">PRO</span>}
    </div>
  );

  return (
    <div style={{ padding:"14px 14px 0" }}>

      {/* ── Item Tabs ── */}
      <div style={{ display:"flex", gap:7, overflowX:"auto", paddingBottom:6, marginBottom:14 }}>
        {items.map(it => (
          <button key={it.id} onClick={() => setActiveId(it.id)} className="btn-press"
            style={{ flexShrink:0, padding:"6px 13px", borderRadius:8, fontSize:13, fontWeight:600,
              border:`1.5px solid ${it.id===activeId ? C.greenBorder : C.border}`,
              background: it.id===activeId ? C.greenDim : C.glass,
              color: it.id===activeId ? C.greenText : C.t2,
              cursor:"pointer", fontFamily:"inherit" }}>
            {it.name ? it.name.slice(0,14) : `Item ${it.id}`}
          </button>
        ))}
        <button onClick={addItem} className="btn-press"
          style={{ flexShrink:0, padding:"6px 13px", borderRadius:8, fontSize:13,
            border:`1.5px dashed ${C.border}`, background:"transparent",
            color:C.t3, cursor:"pointer", fontFamily:"inherit" }}>
          + New
        </button>
      </div>

      {/* ── Item Name ── */}
      <div style={{ marginBottom:12 }}>
        <LBL>Item Name</LBL>
        <input className="gi" style={IS} placeholder="e.g. Jordan 1 Retro High OG"
          value={active?.name||""} onChange={e=>update("name",e.target.value)} />
      </div>

      {/* ── Price Grid ── */}
      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:9, marginBottom:12 }}>
        {[
          { lbl:"Buy Price",  field:"buyPrice",    sell:false },
          { lbl:"Sell Price", field:"sellPrice",   sell:true  },
          { lbl:"Shipping",   field:"shippingCost",sell:false },
          { lbl:"Extra Fees", field:"extraFees",   sell:false },
        ].map(({ lbl, field, sell }) => (
          <div key={field}>
            <LBL color={sell ? C.greenText : undefined}>{lbl}</LBL>
            <div style={{ position:"relative" }}>
              <span style={{ position:"absolute", left:11, top:"50%", transform:"translateY(-50%)",
                fontSize:15, fontWeight:700, color: sell ? C.green : C.t3 }}>$</span>
              <input type="number" inputMode="decimal"
                className={`gi${sell?" gi-sell":""}`}
                style={{ ...IS, paddingLeft:27 }}
                placeholder="0.00" value={active?.[field]||""}
                onChange={e=>update(field,e.target.value)} />
            </div>
          </div>
        ))}
      </div>

      {/* ── Platform Grid ── */}
      <div style={{ marginBottom:12 }}>
        <LBL>Platform</LBL>
        <div style={{ display:"grid", gridTemplateColumns:"repeat(3,1fr)", gap:7 }}>
          {Object.entries(PLATFORM_META).map(([key, meta]) => {
            const isActive = active?.platformKey === key;
            const locked   = meta.isPro && !isPro;
            return (
              <button key={key} className="btn-press"
                onClick={() => {
                  if (locked) { onProClick(); return; }
                  update("platformKey", key);
                  update("category", "general");
                  update("topRated", false);
                }}
                style={{ padding:"10px 4px", borderRadius:9, fontSize:13, fontWeight:700,
                  border:`1.5px solid ${isActive ? C.blueDim+"aa" : C.border}`,
                  background: isActive ? C.blueDim : C.glass,
                  color: isActive ? C.blueText : locked ? C.t3 : C.t2,
                  cursor:"pointer", fontFamily:"inherit",
                  opacity: locked ? 0.6 : 1, position:"relative" }}>
                {locked && <span style={{ position:"absolute", top:3, right:4, fontSize:10 }}>⭐</span>}
                {meta.label}
              </button>
            );
          })}
        </div>
      </div>

      {/* ── Category Dropdown (Pro + platforms with categories) ── */}
      {hasCats && (
        <div style={{ marginBottom:12 }}>
          <LBL pro>Category</LBL>
          <select className="gi" style={{ ...IS, paddingRight:36,
            backgroundImage:`url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%238892a4' stroke-width='2'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E")`,
            backgroundRepeat:"no-repeat", backgroundPosition:"right 12px center" }}
            value={active?.category||"general"}
            onChange={e=>update("category",e.target.value)}>
            {pmeta.categories.map(c => (
              <option key={c.id} value={c.id}>{c.label}</option>
            ))}
          </select>
        </div>
      )}

      {/* ── Smart Toggles ── */}
      <div className="gc" style={{ borderRadius:12, padding:"2px 14px", marginBottom:12 }}>
        <Toggle
          on={active?.topRated||false}
          onChange={v=>update("topRated",v)}
          label="eBay Top Rated Seller"
          sub={active?.platformKey==="eBay" ? "−10% on category fee" : "eBay only"}
          locked={!isPro || active?.platformKey!=="eBay"}
          onLockClick={!isPro ? onProClick : undefined} />
        <Toggle
          on={active?.salesTax||false}
          onChange={v=>update("salesTax",v)}
          label="Sales Tax"
          sub="Adds 7% to buy cost"
          locked={!isPro}
          onLockClick={onProClick} />
      </div>

      {/* ── Result Card ── */}
      <div className={isCalc ? "scan-overlay" : ""}
        style={{ borderRadius:18, padding:20, marginBottom:12, position:"relative",
          overflow:"hidden",
          background: vt.bg,
          border:`1.5px solid ${vt.border}`,
          boxShadow:`0 0 55px ${vt.glow}, inset 0 1px 0 rgba(255,255,255,0.07)`,
          transition:"box-shadow .4s, border-color .4s" }}>

        {/* Glow orb */}
        <div style={{ position:"absolute", top:-50, right:-50, width:140, height:140,
          borderRadius:"50%", background:vt.glow, filter:"blur(50px)", pointerEvents:"none" }} />

        {/* Verdict + profit hero */}
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:18 }}>
          <div>
            <div style={{ fontSize:11, fontWeight:800, color:vt.text, letterSpacing:".14em",
              textTransform:"uppercase", marginBottom:7 }}>Net Profit</div>
            <div key={result.netProfit}
              className={`spin-pop ${result.netProfit>=0?"sh-green":result.verdictColor==="yellow"?"sh-amber":"sh-red"}`}
              style={{ fontSize:46, fontWeight:900, lineHeight:1,
                fontFamily:"'DM Mono','Courier New',monospace" }}>
              {result.netProfit<0?"−":"+"}{fmt(result.netProfit)}
            </div>
          </div>
          <div key={result.verdict} className="spin-pop float"
            style={{ fontSize:46, lineHeight:1 }}>{vt.emoji}</div>
        </div>

        {/* Stats row */}
        <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr 1fr", gap:8, marginBottom:14 }}>
          {[
            { lbl:"ROI",         val:fmtP(result.roi),         pos:result.roi>=15    },
            { lbl:"Margin",      val:fmtP(result.margin),      pos:result.margin>=15 },
            { lbl:"Platform Cut",val:fmt(result.platformCut),  neutral:true          },
          ].map(({ lbl, val, pos, neutral }) => (
            <div key={lbl} style={{ background:"rgba(0,0,0,0.3)", borderRadius:9,
              padding:"10px 4px", textAlign:"center", backdropFilter:"blur(8px)" }}>
              <div style={{ fontSize:10, fontWeight:700, color:C.t3, textTransform:"uppercase",
                letterSpacing:".08em", marginBottom:4 }}>{lbl}</div>
              <div style={{ fontSize:16, fontWeight:800,
                color: neutral ? C.t2 : pos ? C.greenText : C.redText,
                fontFamily:"'DM Mono',monospace" }}>{val}</div>
            </div>
          ))}
        </div>

        {/* Breakeven + tax */}
        <div style={{ display:"flex", justifyContent:"space-between", fontSize:13, color:C.t2,
          borderTop:"1px solid rgba(255,255,255,0.07)", paddingTop:12 }}>
          <span>Break-even: <strong style={{ color:C.t1, fontFamily:"'DM Mono',monospace" }}>{fmt(result.breakEven)}</strong></span>
          {result.taxCost>0 && <span style={{ color:C.amberText }}>Tax: <strong style={{ fontFamily:"'DM Mono',monospace" }}>{fmt(result.taxCost)}</strong></span>}
        </div>

        {/* Toggle fee breakdown */}
        <button onClick={() => setShowBreakdown(p=>!p)} className="btn-press"
          style={{ marginTop:10, fontSize:12, color:C.t3, background:"none", border:"none",
            cursor:"pointer", fontFamily:"inherit", padding:0, textDecoration:"underline dotted" }}>
          {showBreakdown ? "Hide" : "Show"} fee breakdown
        </button>
        {showBreakdown && <FeeBreakdown lines={result.feeBreakdown} />}
      </div>

      {/* Verdict message */}
      <div className="gc fade-up"
        style={{ borderRadius:12, padding:"13px 15px", marginBottom:12,
          display:"flex", alignItems:"flex-start", gap:11 }}>
        <span style={{ fontSize:22, flexShrink:0 }}>
          {result.verdictColor==="green"?"💡":result.verdictColor==="yellow"?"🤔":"🚫"}
        </span>
        <p style={{ margin:0, fontSize:14, color:C.t2, lineHeight:1.65 }}>
          {result.verdictColor==="green"
            ? `${result.roi.toFixed(0)}% ROI — solid deal. Platform takes ${fmt(result.platformCut)}.`
            : result.verdictColor==="yellow"
            ? `Slim margin. Try sourcing below ${fmt(result.breakEven * 0.85)} to hit 30% ROI.`
            : `After ${fmt(result.platformCut)} in fees, this deal doesn't pencil. Pass.`}
        </p>
      </div>

      {/* Notes */}
      <div style={{ marginBottom:12 }}>
        <LBL>Notes</LBL>
        <textarea className="gi" style={{ ...IS, resize:"none", lineHeight:1.55, display:"block" }}
          rows={2} placeholder="Source, condition, lot details..."
          value={active?.note||""} onChange={e=>update("note",e.target.value)} />
      </div>

      {/* Actions */}
      <div style={{ display:"flex", gap:9, paddingBottom:18 }}>
        <button onClick={saveToHistory} className="btn-press"
          style={{ flex:1, background:`linear-gradient(135deg,${C.green},#16a34a)`,
            color:"#000", fontWeight:800, fontSize:16, padding:"15px 0", borderRadius:13,
            border:"none", cursor:"pointer", fontFamily:"inherit",
            boxShadow:`0 4px 22px ${C.greenGlow}` }}>
          💾  Save Flip
        </button>
        <button onClick={() => removeItem(activeId)} className="btn-press"
          style={{ padding:"15px 18px", borderRadius:13, background:C.glass,
            border:`1px solid ${C.border}`, color:C.t2, cursor:"pointer", fontSize:20 }}>
          🗑️
        </button>
      </div>
    </div>
  );

  function removeItem(id) {/* handled by parent via prop */ }
}

// ════════════════════════════════════════════════════════════════
// §8  HISTORY VIEW
// ════════════════════════════════════════════════════════════════

function HistoryView({ history, isPro, search, setSearch, onProClick }) {
  const IS = { borderRadius:10, padding:"12px 14px", fontSize:15 };
  const filtered = history.filter(h =>
    (h.name||"").toLowerCase().includes(search.toLowerCase()) ||
    (h.note||"").toLowerCase().includes(search.toLowerCase())
  );
  const visible = isPro ? filtered : filtered.slice(0, FREE_HISTORY_LIMIT);
  const locked  = !isPro ? filtered.slice(FREE_HISTORY_LIMIT) : [];

  return (
    <div style={{ padding:"14px 14px 0" }}>
      <input className="gi" style={{ ...IS, marginBottom:14 }}
        placeholder="🔍  Search saved flips..."
        value={search} onChange={e=>setSearch(e.target.value)} />

      {filtered.length === 0 ? (
        <div style={{ textAlign:"center", paddingTop:70, color:C.t3 }}>
          <div className="float" style={{ fontSize:52, marginBottom:14 }}>📦</div>
          <p style={{ fontSize:16, margin:0, color:C.t2 }}>No flips saved yet.</p>
          <p style={{ fontSize:13, marginTop:5 }}>Save deals in the Calculator to see them here.</p>
        </div>
      ) : (
        <>
          {visible.map((h, i) => <HistoryCard key={i} h={h} blurred={false} />)}

          {locked.length > 0 && (
            <>
              {locked.slice(0,2).map((h, i) => (
                <HistoryCard key={`lk${i}`} h={h} blurred />
              ))}
              {/* Paywall CTA */}
              <div className="gc fade-up"
                style={{ borderRadius:18, padding:24, textAlign:"center", marginBottom:12,
                  border:`1px solid ${C.goldBorder}`,
                  background:`linear-gradient(135deg,${C.goldDim},rgba(10,12,18,0.9))`,
                  boxShadow:`0 0 40px ${C.goldDim}` }}>
                <div style={{ fontSize:42, marginBottom:10 }}>⭐</div>
                <div style={{ fontSize:18, fontWeight:900, color:C.goldText, marginBottom:8 }}>
                  Unlock Full History
                </div>
                <p style={{ fontSize:13, color:C.t2, margin:"0 0 18px", lineHeight:1.6 }}>
                  {locked.length} more flip{locked.length!==1?"s":""} locked.
                  Pro gives you unlimited history, CSV export, and tax reports.
                </p>
                <button className="btn-press" onClick={onProClick}
                  style={{ width:"100%", padding:"13px 0", borderRadius:11,
                    background:`linear-gradient(135deg,${C.gold},#b45309)`,
                    color:"#000", fontWeight:900, fontSize:15, border:"none",
                    cursor:"pointer", fontFamily:"inherit",
                    boxShadow:"0 4px 20px rgba(245,158,11,0.32)" }}>
                  ✨ Upgrade to Pro — $4.99/mo
                </button>
                <div style={{ fontSize:11, color:C.t3, marginTop:10 }}>
                  Cancel anytime · 7-day free trial
                </div>
              </div>
            </>
          )}
        </>
      )}
      <div style={{ height:20 }} />
    </div>
  );
}

// ════════════════════════════════════════════════════════════════
// §9  STATS VIEW
// ════════════════════════════════════════════════════════════════

function StatsView({ history, isPro, onProClick }) {
  if (!isPro) {
    return (
      <div style={{ padding:"40px 20px", textAlign:"center" }}>
        <div className="float" style={{ fontSize:64, marginBottom:16 }}>📊</div>
        <h2 style={{ margin:"0 0 10px", fontSize:22, fontWeight:900, color:C.t1 }}>Stats Dashboard</h2>
        <p style={{ fontSize:14, color:C.t2, margin:"0 0 24px", lineHeight:1.7 }}>
          Track your ROI trends, best flips, win rate, and total profit over time.
          Pro-exclusive.
        </p>
        <button className="btn-press" onClick={onProClick}
          style={{ padding:"14px 32px", borderRadius:13, background:`linear-gradient(135deg,${C.gold},#b45309)`,
            color:"#000", fontWeight:900, fontSize:16, border:"none", cursor:"pointer",
            fontFamily:"inherit", boxShadow:"0 4px 20px rgba(245,158,11,0.32)" }}>
          ⭐ Unlock Stats — $4.99/mo
        </button>
      </div>
    );
  }

  const totalProfit = history.reduce((s,h) => s + (h.netProfit||0), 0);
  const wins        = history.filter(h => h.netProfit > 0);
  const winRate     = history.length ? Math.round(wins.length / history.length * 100) : 0;
  const avgROI      = history.length ? history.reduce((s,h) => s + (h.roi||0), 0) / history.length : 0;
  const avgProfit   = history.length ? totalProfit / history.length : 0;

  return (
    <div style={{ padding:"14px 14px 0" }}>
      {/* Big numbers */}
      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:9, marginBottom:9 }}>
        {[
          { lbl:"Total Flips", val:history.length,       color:C.t1           },
          { lbl:"Win Rate",    val:`${winRate}%`,         color:winRate>=50?C.greenText:C.redText },
          { lbl:"Avg ROI",     val:fmtP(avgROI),          color:avgROI>=20?C.greenText:C.redText  },
          { lbl:"Avg Profit",  val:`+${fmt(avgProfit)}`,  color:avgProfit>0?C.greenText:C.redText },
        ].map(({ lbl, val, color }) => (
          <div key={lbl} className="gc fade-up" style={{ borderRadius:14, padding:16 }}>
            <div style={{ fontSize:11, fontWeight:700, color:C.t3, textTransform:"uppercase",
              letterSpacing:".07em", marginBottom:7 }}>{lbl}</div>
            <div style={{ fontSize:32, fontWeight:900, color, fontFamily:"'DM Mono',monospace" }}>{val}</div>
          </div>
        ))}
      </div>

      {/* Total profit hero */}
      <div className="gc fade-up" style={{ borderRadius:14, padding:20, marginBottom:9,
        background: totalProfit>=0 ? C.greenDim : C.redDim,
        border:`1px solid ${totalProfit>=0 ? C.greenBorder : C.redBorder}`,
        boxShadow:`0 0 45px ${totalProfit>=0 ? C.greenGlow : C.redGlow}` }}>
        <div style={{ fontSize:11, fontWeight:700, color:C.t3, textTransform:"uppercase",
          letterSpacing:".07em", marginBottom:7 }}>Total Profit</div>
        <div className={totalProfit>=0?"sh-green":"sh-red"}
          style={{ fontSize:44, fontWeight:900, fontFamily:"'DM Mono',monospace" }}>
          {totalProfit>=0?"+":"−"}{fmt(totalProfit)}
        </div>
      </div>

      {/* Best flips */}
      {history.length > 0 && (
        <div className="gc fade-up" style={{ borderRadius:14, padding:18, marginBottom:9 }}>
          <div style={{ fontSize:14, fontWeight:700, color:C.t1, marginBottom:13 }}>🏆 Best Flips</div>
          {[...history].sort((a,b)=>(b.netProfit||0)-(a.netProfit||0)).slice(0,5).map((h,i) => (
            <div key={i} style={{ display:"flex", justifyContent:"space-between", alignItems:"center",
              paddingBottom:10, marginBottom:10, borderBottom:i<4?`1px solid ${C.border}`:"none" }}>
              <div style={{ display:"flex", alignItems:"center", gap:9 }}>
                <span style={{ fontSize:18 }}>{["🥇","🥈","🥉","4️⃣","5️⃣"][i]}</span>
                <div>
                  <div style={{ fontSize:14, color:C.t1, maxWidth:170, overflow:"hidden",
                    textOverflow:"ellipsis", whiteSpace:"nowrap" }}>{h.name||"Unnamed"}</div>
                  <div style={{ fontSize:11, color:C.t3 }}>{h.platformKey}</div>
                </div>
              </div>
              <span style={{ fontSize:15, fontWeight:800,
                color:(h.netProfit||0)>=0?C.greenText:C.redText,
                fontFamily:"'DM Mono',monospace", flexShrink:0 }}>
                +{fmt(h.netProfit||0)}
              </span>
            </div>
          ))}
        </div>
      )}

      {/* Platform breakdown */}
      {history.length > 0 && (() => {
        const byPlatform = {};
        history.forEach(h => {
          if (!byPlatform[h.platformKey]) byPlatform[h.platformKey] = { count:0, profit:0 };
          byPlatform[h.platformKey].count++;
          byPlatform[h.platformKey].profit += h.netProfit||0;
        });
        return (
          <div className="gc fade-up" style={{ borderRadius:14, padding:18, marginBottom:9 }}>
            <div style={{ fontSize:14, fontWeight:700, color:C.t1, marginBottom:13 }}>📦 By Platform</div>
            {Object.entries(byPlatform).map(([plat, { count, profit }]) => (
              <div key={plat} style={{ display:"flex", justifyContent:"space-between", alignItems:"center",
                paddingBottom:10, marginBottom:10, borderBottom:`1px solid ${C.border}` }}>
                <div>
                  <div style={{ fontSize:14, color:C.t1, fontWeight:600 }}>
                    {PLATFORM_META[plat]?.label || plat}
                  </div>
                  <div style={{ fontSize:11, color:C.t3 }}>{count} flip{count!==1?"s":""}</div>
                </div>
                <span style={{ fontSize:15, fontWeight:800,
                  color:profit>=0?C.greenText:C.redText,
                  fontFamily:"'DM Mono',monospace" }}>
                  {profit>=0?"+":"−"}{fmt(profit)}
                </span>
              </div>
            ))}
          </div>
        );
      })()}

      {/* Pro Features */}
      <div className="gc fade-up" style={{ borderRadius:14, padding:18, marginBottom:9,
        border:`1px solid ${C.goldBorder}`, background:C.goldDim }}>
        <div style={{ display:"flex", alignItems:"center", gap:8, marginBottom:13 }}>
          <span style={{ fontSize:18 }}>⭐</span>
          <span style={{ fontSize:15, fontWeight:700, color:C.goldText }}>Pro Tools</span>
        </div>
        {[
          { icon:"📤", lbl:"Export to CSV",           desc:"Download all flips as spreadsheet" },
          { icon:"🧾", lbl:"Tax Report (Schedule C)", desc:"Auto-generate seller tax summary"   },
          { icon:"📈", lbl:"Profit Trend Charts",     desc:"30/60/90-day performance graphs"    },
          { icon:"🔔", lbl:"Price Drop Alerts",       desc:"Notify when sourcing prices fall"   },
        ].map(({ icon, lbl, desc }) => (
          <div key={lbl} onClick={() => alert(`${lbl} — coming in the next update! 🚀`)}
            style={{ display:"flex", alignItems:"center", gap:11, padding:"10px 0",
              borderBottom:`1px solid ${C.border}`, cursor:"pointer" }}>
            <span style={{ fontSize:22, flexShrink:0 }}>{icon}</span>
            <div style={{ flex:1 }}>
              <div style={{ fontSize:14, fontWeight:600, color:C.t1 }}>{lbl}</div>
              <div style={{ fontSize:12, color:C.t3 }}>{desc}</div>
            </div>
            <span style={{ fontSize:13, color:C.t3 }}>→</span>
          </div>
        ))}
      </div>

      <div style={{ height:20 }} />
    </div>
  );
}

// ════════════════════════════════════════════════════════════════
// §10  ROOT APP
// ════════════════════════════════════════════════════════════════

export default function App() {
  const [tab,      setTab]      = useState("calc");
  const [isPro,    setIsPro]    = useState(false);   // ← wire to Stripe / RevenueCat
  const [showPro,  setShowPro]  = useState(false);   // paywall modal
  const [items,    setItems]    = useState([newItem()]);
  const [activeId, setActiveId] = useState(1);
  const [search,   setSearch]   = useState("");
  const [isCalc,   setIsCalc]   = useState(false);
  const calcTimer = useRef(null);

  const [history, setHistory] = useState([
    { id:0, name:"Nike Dunk Low Panda", buyPrice:"80", sellPrice:"160",
      shippingCost:"12", platformFee:13.25, extraFees:"0", note:"DS, size 10",
      platformKey:"eBay", category:"sneakers", topRated:false, salesTax:false,
      shippingSubsidy:"",
      netProfit:48.8, roi:61.0, margin:30.5, platformCut:12.8, feeBreakdown:[],
      totalCost:111.2, taxCost:0, verdict:"BUY", verdictColor:"green", savedAt:"10:23 AM" },
    { id:0, name:"Pokemon Booster Box", buyPrice:"110", sellPrice:"145",
      shippingCost:"8", extraFees:"0", note:"Whatnot auction",
      platformKey:"Whatnot", category:"general", topRated:false, salesTax:false,
      shippingSubsidy:"",
      netProfit:12.94, roi:11.76, margin:8.93, platformCut:15.81, feeBreakdown:[],
      totalCost:132.06, taxCost:0, verdict:"PASS", verdictColor:"red", savedAt:"9:05 AM" },
    { id:0, name:"Vintage Levi's 501 (lot)", buyPrice:"40", sellPrice:"95",
      shippingCost:"9", extraFees:"2", note:"Depop find",
      platformKey:"Depop", category:"general", topRated:false, salesTax:true,
      shippingSubsidy:"",
      netProfit:34.0, roi:85.0, margin:35.8, platformCut:3.59, feeBreakdown:[],
      totalCost:57.39, taxCost:2.8, verdict:"BUY", verdictColor:"green", savedAt:"8:47 AM" },
    { id:0, name:"Funko Pop Chase", buyPrice:"65", sellPrice:"85",
      shippingCost:"7", extraFees:"0", note:"eBay BIN",
      platformKey:"eBay", category:"general", topRated:false, salesTax:false,
      shippingSubsidy:"",
      netProfit:0.64, roi:0.98, margin:0.75, platformCut:11.96, feeBreakdown:[],
      totalCost:83.96, taxCost:0, verdict:"PASS", verdictColor:"red", savedAt:"Yesterday" },
    { id:0, name:"TikTok Water Bottle Trend", buyPrice:"12", sellPrice:"34",
      shippingCost:"5", extraFees:"0", note:"TikTok Shop deal",
      platformKey:"TikTokShop", category:"general", topRated:false, salesTax:false,
      shippingSubsidy:"3",
      netProfit:14.66, roi:122.2, margin:43.1, platformCut:2.34, feeBreakdown:[],
      totalCost:17.34, taxCost:0, verdict:"BUY", verdictColor:"green", savedAt:"Yesterday" },
  ]);

  const active = items.find(i => i.id === activeId) || items[0];
  const result = useMemo(() => calcProfit(active || {}), [active]);
  const vt     = VT[result.verdictColor];

  // Shimmer trigger on any calc-relevant field change
  useEffect(() => {
    setIsCalc(true);
    if (calcTimer.current) clearTimeout(calcTimer.current);
    calcTimer.current = setTimeout(() => setIsCalc(false), 700);
    return () => clearTimeout(calcTimer.current);
  }, [
    active?.buyPrice, active?.sellPrice, active?.shippingCost,
    active?.extraFees, active?.platformKey, active?.category,
    active?.topRated, active?.salesTax, active?.shippingSubsidy,
  ]);

  const update = useCallback((f, v) =>
    setItems(p => p.map(i => i.id === activeId ? { ...i, [f]: v } : i)), [activeId]);

  const addItem = () => { const n = newItem(); setItems(p=>[...p,n]); setActiveId(n.id); };

  const removeItem = useCallback((id) => {
    const next = items.filter(i => i.id !== id);
    if (!next.length) { const n = newItem(); setItems([n]); setActiveId(n.id); return; }
    setItems(next);
    if (activeId === id) setActiveId(next[next.length-1].id);
  }, [items, activeId]);

  const saveToHistory = () => {
    if (!active?.name && !active?.sellPrice) return;
    setHistory(p => [{ ...active, ...result, savedAt: new Date().toLocaleTimeString() }, ...p.slice(0,99)]);
  };

  const openPro = () => setShowPro(true);

  return (
    <>
      <style>{CSS}</style>
      {showPro && <ProModal onClose={() => setShowPro(false)} />}

      <div style={{ fontFamily:"'DM Sans',system-ui,sans-serif",
        background:C.page, minHeight:"100vh", color:C.t1,
        paddingBottom:88, maxWidth:480, margin:"0 auto",
        backgroundImage:`
          radial-gradient(ellipse at 15% 0%,  rgba(34,197,94,0.07)  0%, transparent 55%),
          radial-gradient(ellipse at 85% 100%, rgba(59,130,246,0.06) 0%, transparent 55%)` }}>

        {/* ── HEADER ── */}
        <div style={{ position:"sticky", top:0, zIndex:40,
          background:"rgba(10,12,18,0.88)", backdropFilter:"blur(22px)",
          borderBottom:`1px solid ${C.border}`,
          padding:"12px 14px", display:"flex", alignItems:"center", justifyContent:"space-between" }}>

          <div style={{ display:"flex", alignItems:"center", gap:9 }}>
            <span className="float" style={{ fontSize:22 }}>⚡</span>
            <div style={{ lineHeight:1 }}>
              <span style={{ fontWeight:900, fontSize:19, letterSpacing:"-.03em" }}>FlipCalc</span>
              <span style={{ fontWeight:900, fontSize:19, letterSpacing:"-.03em",
                background:"linear-gradient(90deg,#f59e0b,#fbbf24)",
                WebkitBackgroundClip:"text", WebkitTextFillColor:"transparent" }}> Pro</span>
            </div>
          </div>

          <div style={{ display:"flex", alignItems:"center", gap:8 }}>
            {/* Demo toggle */}
            <button className="btn-press" onClick={()=>setIsPro(p=>!p)}
              style={{ fontSize:11, fontWeight:700, padding:"4px 10px", borderRadius:999,
                border:`1px solid ${isPro?C.goldBorder:C.border}`,
                background: isPro?C.goldDim:C.glass,
                color: isPro?C.goldText:C.t3,
                cursor:"pointer", fontFamily:"inherit" }}>
              {isPro?"⭐ PRO":"Free"}
            </button>

            {/* Live verdict badge */}
            <div key={result.verdict} className="spin-pop"
              style={{ fontSize:13, fontWeight:700, padding:"5px 13px", borderRadius:999,
                background:vt.bg, border:`1px solid ${vt.border}`, color:vt.text,
                boxShadow:`0 0 14px ${vt.glow}` }}>
              {vt.emoji} {vt.label}
            </div>
          </div>
        </div>

        {/* ── TAB BAR ── */}
        <div style={{ display:"flex", borderBottom:`1px solid ${C.border}`,
          background:"rgba(10,12,18,0.65)", backdropFilter:"blur(12px)",
          position:"sticky", top:56, zIndex:39 }}>
          {[
            { id:"calc",    label:"Calculator" },
            { id:"history", label:"History", badge:history.length },
            { id:"stats",   label:"Stats", locked:!isPro },
          ].map(({ id, label, badge, locked }) => (
            <button key={id} className="btn-press"
              onClick={() => { if (locked) { openPro(); return; } setTab(id); }}
              style={{ flex:1, padding:"12px 4px", border:"none", background:"transparent",
                cursor:"pointer", fontFamily:"inherit",
                color: tab===id ? C.t1 : C.t3,
                fontWeight: tab===id ? 700 : 500, fontSize:13,
                borderBottom:`2px solid ${tab===id ? C.green : "transparent"}`,
                transition:"all .2s" }}>
              {label}
              {locked && <span className="pro-badge">PRO</span>}
              {badge>0 && !locked && (
                <span style={{ marginLeft:5, fontSize:10, fontWeight:800,
                  padding:"1px 6px", borderRadius:999,
                  background: tab===id ? C.greenDim : "rgba(255,255,255,0.07)",
                  color: tab===id ? C.greenText : C.t3 }}>{badge}</span>
              )}
            </button>
          ))}
        </div>

        {/* ── VIEWS ── */}
        {tab === "calc" && (
          <CalculatorView
            items={items} activeId={activeId} setActiveId={setActiveId}
            addItem={addItem} removeItem={removeItem}
            update={update} saveToHistory={saveToHistory}
            active={active} result={result} isCalc={isCalc}
            isPro={isPro} onProClick={openPro} />
        )}
        {tab === "history" && (
          <HistoryView
            history={history} isPro={isPro}
            search={search} setSearch={setSearch}
            onProClick={openPro} />
        )}
        {tab === "stats" && (
          <StatsView history={history} isPro={isPro} onProClick={openPro} />
        )}

        {/* ── BOTTOM NAV ── */}
        <div style={{ position:"fixed", bottom:0, left:"50%", transform:"translateX(-50%)",
          width:"100%", maxWidth:480,
          background:"rgba(12,14,20,0.94)", backdropFilter:"blur(22px)",
          borderTop:`1px solid ${C.border}`,
          display:"flex", justifyContent:"space-around", alignItems:"center",
          padding:"9px 0 18px", zIndex:50 }}>
          {[
            { id:"calc",    icon:"⚡", label:"Calc"     },
            { id:"history", icon:"📦", label:"History",  badge:history.length },
            { id:"stats",   icon:"📊", label:"Stats",    locked:!isPro },
          ].map(({ id, icon, label, badge, locked }) => {
            const on = tab === id;
            return (
              <button key={id} className="nav-b btn-press"
                onClick={() => { if (locked) { openPro(); return; } setTab(id); }}
                style={{ display:"flex", flexDirection:"column", alignItems:"center", gap:3,
                  padding:"4px 22px", border:"none", background:"transparent",
                  cursor:"pointer", fontFamily:"inherit",
                  color: on ? C.green : C.t3, position:"relative" }}>
                <span style={{ fontSize:24 }}>{icon}</span>
                <span style={{ fontSize:11, fontWeight: on?700:500, letterSpacing:".02em" }}>
                  {label}{locked && " ⭐"}
                </span>
                {badge>0 && (
                  <span style={{ position:"absolute", top:0, right:8,
                    width:17, height:17, background:C.green, color:"#000",
                    fontSize:9, fontWeight:900, borderRadius:"50%",
                    display:"flex", alignItems:"center", justifyContent:"center",
                    boxShadow:`0 0 8px ${C.greenGlow}` }}>
                    {badge>99?"99+":badge}
                  </span>
                )}
              </button>
            );
          })}
        </div>

      </div>
    </>
  );
}
