@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Manrope:wght@400;500;600;700;800&display=swap');

:root {
  color-scheme: light;
  --bg: #f3f6fb; --surface: #ffffff; --surface-alt: #f7f9fc; --text: #17233b;
  --muted: #65728a; --line: #e2e8f1; --primary: #2563eb; --primary-hover: #1d4ed8;
  --accent: #dbeafe; --shadow: 0 12px 35px rgba(27, 48, 83, .07);
  --danger: #b42318; --danger-bg: #fff1f0; --warning-bg: #fff7e6; --warning-text: #8a4b08;
}
body.dark-theme {
  color-scheme: dark; --bg: #0c1424; --surface: #141f33; --surface-alt: #1a2941;
  --text: #edf3ff; --muted: #a5b3ca; --line: #2b3a52; --primary: #6ea2ff;
  --primary-hover: #8ab3ff; --accent: #223b61; --shadow: 0 12px 35px rgba(0,0,0,.2);
  --danger: #ffb4ab; --danger-bg: #442523; --warning-bg: #40351e; --warning-text: #ffd28b;
}
* { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body { margin: 0; background: var(--bg); color: var(--text); font-family: 'DM Sans', sans-serif; line-height: 1.5; transition: background .2s, color .2s; }
button, input { font: inherit; }
button { cursor: pointer; }
button:focus-visible, input:focus-visible { outline: 3px solid #93c5fd; outline-offset: 2px; }
.topbar { background: var(--surface); border-bottom: 1px solid var(--line); }
.topbar-inner { max-width: 1180px; margin: auto; padding: 16px 24px; display: flex; align-items: center; justify-content: space-between; gap: 16px; }
.brand { display: inline-flex; gap: 11px; align-items: center; text-decoration: none; color: var(--text); font-family: Manrope, sans-serif; font-weight: 800; font-size: 1.1rem; }
.brand small { display: block; color: var(--muted); font: 700 .62rem 'DM Sans', sans-serif; letter-spacing: .13em; }
.brand-mark { display: grid; place-items: center; width: 42px; height: 42px; border-radius: 13px; background: var(--primary); color: white; font-size: 1.35rem; }
.container { width: min(1180px, calc(100% - 40px)); margin: 0 auto; }
.intro { display: flex; justify-content: space-between; align-items: end; gap: 20px; padding: 36px 0 24px; }
h1, h2, h3, p { margin-top: 0; }
h1, h2, h3 { font-family: Manrope, sans-serif; letter-spacing: -.025em; }
h1 { font-size: clamp(1.8rem, 4vw, 2.6rem); margin-bottom: 7px; }
h2 { font-size: 1.22rem; margin-bottom: 0; }
h3 { margin-bottom: 6px; }
.muted { color: var(--muted); }
.eyebrow { color: var(--primary); font-size: .7rem; font-weight: 800; letter-spacing: .13em; margin-bottom: 7px; }
.update-pill { display: inline-flex; align-items: center; gap: 8px; color: var(--muted); background: var(--surface); border: 1px solid var(--line); padding: 9px 12px; border-radius: 999px; font-size: .82rem; white-space: nowrap; }
.status-dot { width: 8px; height: 8px; border-radius: 50%; background: #f59e0b; }
.panel { background: var(--surface); border: 1px solid var(--line); border-radius: 20px; padding: 24px; box-shadow: var(--shadow); margin-bottom: 20px; }
.search-panel h2 { margin-bottom: 18px; }
#searchForm { display: flex; align-items: end; gap: 13px; flex-wrap: wrap; }
.field { display: flex; flex-direction: column; gap: 7px; flex: 1 1 230px; }
.field-country { flex: 0 1 150px; }
label { font-weight: 700; font-size: .85rem; }
input { min-width: 0; width: 100%; border: 1px solid var(--line); border-radius: 11px; background: var(--surface-alt); color: var(--text); padding: 12px 13px; }
.button { border: 1px solid transparent; border-radius: 11px; padding: 12px 16px; font-weight: 700; transition: transform .15s, background .15s; }
.button:hover { transform: translateY(-1px); }
.button-primary { background: var(--primary); color: #fff; }
.button-primary:hover { background: var(--primary-hover); }
.button-secondary { color: var(--text); background: var(--surface-alt); border-color: var(--line); }
.button-ghost { background: transparent; border-color: var(--line); color: var(--text); }
.loading { display: flex; align-items: center; gap: 10px; margin-top: 16px; color: var(--primary); font-weight: 600; }
.spinner { width: 17px; height: 17px; border: 2px solid var(--line); border-top-color: var(--primary); border-radius: 50%; animation: spin .8s linear infinite; }
@keyframes spin { to { transform: rotate(360deg); } }
.hidden { display: none !important; }
.error-msg { color: var(--danger); background: var(--danger-bg); padding: 12px 14px; border-radius: 10px; margin: 14px 0 0; }
.section-heading { display: flex; justify-content: space-between; align-items: center; gap: 14px; margin-bottom: 20px; }
.weather-summary { display: flex; justify-content: space-between; align-items: center; gap: 20px; padding: 8px 0 25px; }
.location-block h3 { font-size: 1.6rem; margin-bottom: 5px; }
.location-block .muted { margin-bottom: 4px; }
.weather-description { text-transform: capitalize; color: var(--muted); margin-bottom: 0; }
.main-temperature { min-width: 190px; display: grid; grid-template-columns: auto auto; align-items: center; justify-content: end; column-gap: 12px; }
.main-temperature > span:first-child { font-size: 2.4rem; }
.main-temperature strong { font: 800 2.8rem Manrope, sans-serif; }
.main-temperature .muted { grid-column: 2; font-size: .82rem; }
.cards-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 13px; }
.metric-card { padding: 17px; border: 1px solid var(--line); background: var(--surface-alt); border-radius: 15px; }
.metric-icon { font-size: 1.25rem; }
.metric-label { color: var(--muted); font-size: .83rem; margin: 8px 0 3px; }
.metric-card strong { font: 800 1.35rem Manrope, sans-serif; }
.alert-card { background: var(--warning-bg); color: var(--warning-text); border: 1px solid #e9c46a; border-radius: 13px; padding: 15px 17px; margin-top: 17px; }
.alert-card p { margin: 4px 0 0; }
.content-grid { display: grid; grid-template-columns: minmax(0, 1.5fr) minmax(280px, 1fr); gap: 20px; align-items: start; }
.chart-help { font-size: .87rem; margin: 7px 0 16px; }
.chart-container { position: relative; height: 300px; }
.history-list { list-style: none; padding: 0; margin: 0; }
.history-list li { padding: 12px 0; border-bottom: 1px solid var(--line); }
.history-list li:last-child { border-bottom: 0; }
.history-city { display: block; font-weight: 700; }
.history-detail { color: var(--muted); font-size: .83rem; }
.empty-state { color: var(--muted); }
.text-button { border: 0; background: transparent; color: var(--primary); font-weight: 700; padding: 7px; }
.favorite-panel { display: flex; justify-content: space-between; align-items: center; gap: 20px; }
.favorite-panel h2 { margin-bottom: 5px; }
.favorite-panel p:last-child { margin-bottom: 0; }
.status-badge { border-radius: 999px; padding: 6px 11px; background: var(--warning-bg); color: var(--warning-text); font-size: .78rem; font-weight: 800; }
.status-badge.connected { background: #dcfce7; color: #166534; }
body.dark-theme .status-badge.connected { background: #153b2a; color: #a7f3d0; }
.status-badge.failed { background: var(--danger-bg); color: var(--danger); }
.mqtt-grid { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 14px; }
.mqtt-grid > div { background: var(--surface-alt); border: 1px solid var(--line); border-radius: 12px; padding: 14px; min-width: 0; }
.detail-label { display: block; color: var(--muted); font-size: .77rem; margin-bottom: 5px; }
.mqtt-grid strong { display: block; overflow-wrap: anywhere; }
.payload-label { margin-top: 16px; }
.payload { white-space: pre-wrap; overflow-wrap: anywhere; background: var(--surface-alt); border: 1px solid var(--line); padding: 13px; border-radius: 12px; font-size: .78rem; color: var(--text); max-height: 180px; overflow: auto; }
.mqtt-note { color: var(--muted); font-size: .78rem; margin: 12px 0 0; }
footer { text-align: center; color: var(--muted); font-size: .82rem; padding: 18px 20px 30px; }
footer p { margin: 4px 0; }
@media (max-width: 800px) {
  .content-grid { grid-template-columns: 1fr; }
  .cards-grid { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .intro { align-items: start; flex-direction: column; }
}
@media (max-width: 560px) {
  .container { width: min(100% - 24px, 1180px); }
  .topbar-inner { padding: 13px 12px; }
  .panel { padding: 17px; border-radius: 16px; }
  .intro { padding-top: 26px; }
  #searchForm { align-items: stretch; flex-direction: column; }
  .field, .field-country { flex: auto; }
  .weather-summary { align-items: start; flex-direction: column; }
  .main-temperature { justify-content: start; }
  .cards-grid { gap: 9px; }
  .metric-card { padding: 13px; }
  .metric-card strong { font-size: 1.15rem; }
  .section-heading, .favorite-panel { align-items: flex-start; }
  .favorite-panel { flex-direction: column; }
  .mqtt-grid { grid-template-columns: 1fr; }
  .update-pill { white-space: normal; }
}
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { scroll-behavior: auto !important; animation-duration: .01ms !important; transition-duration: .01ms !important; }
}
