# Lead Hub — Mini Design System

A drop-in design system extracted from the Lead Hub prototype. Copy this file into any project. It gives Claude (or any engineer) the tokens + component recipes to **restyle existing components into the Lead Hub look**.

Stack assumptions: **React + inline styles** (no CSS framework required). Icons use [lucide](https://lucide.dev) (`lucide` global, or `lucide-react`). Everything below is framework-agnostic — the tokens translate 1:1 to CSS variables or Tailwind theme.

---

## 0. How to use

1. Drop `LH` tokens (§1) into a shared module (`theme.js`).
2. Paste the component recipes (§4) you need.
3. To **convert an existing component** to this system, give Claude this file + the component and say: _"Restyle this to the Lead Hub design system — use the `LH` tokens and the matching recipe. Keep behavior, change only styling."_

Two palettes ship: **Zuper** (neutral/slate, default) and **Legacy** (warm/stone). Pick one as `LH`.

---

## 1. Design tokens

```js
// theme.js — pick ONE palette as LH
export const ZUPER_LH = {
  primary: '#111827', primaryDark: '#000000', tint: '#f1f2f4',
  ink: '#1E293B', body: '#475569', sub: '#64748B', muted: '#94A3B8',
  border: '#E2E8F0', borderSoft: '#EEF2F6', card: '#ffffff', bg: '#FDFDFC',
  green: '#16a34a', greenBg: '#dcfce7', greenBd: '#bbf7d0',
  amber: '#b45309', amberBg: '#fef3c7', amberBd: '#fde68a',
  red:   '#dc2626', redBg:   '#fee2e2', redBd:   '#fecaca',
  blue:  '#2563eb', blueBg:  '#eff6ff', blueBd:  '#dbeafe',
};

export const LEGACY_LH = {
  primary: '#1f1f1d', primaryDark: '#000000', tint: '#f0eeea',
  ink: '#252A31', body: '#475569', sub: '#64748B', muted: '#94A3B8',
  border: '#e7e2d8', borderSoft: '#efeae0', card: '#ffffff', bg: '#fbf9f5',
  green: '#15803d', greenBg: '#e7f6ec', greenBd: '#c3e8ce',
  amber: '#8a6410', amberBg: '#fff5e2', amberBd: '#f6e2ad',
  red:   '#dc2626', redBg:   '#fdecec', redBd:   '#f6cccc',
  blue:  '#2563eb', blueBg:  '#eff4ff', blueBd:  '#cfe0fb',
};

export const LH = ZUPER_LH; // default
```

**Role of each token**

| Token | Use |
|---|---|
| `primary` | brand/ink — primary buttons, selected states, active borders |
| `ink` | headings, strong text |
| `body` | default body text |
| `sub` | secondary text, icons |
| `muted` | tertiary text, placeholders, timestamps |
| `border` / `borderSoft` | card/input borders / hairline dividers |
| `tint` | subtle fill (avatars, selected pills, hover) |
| `bg` | page / canvas background |
| `green/amber/red/blue` + `Bg`/`Bd` | semantic fill/border trios for pills & banners |

### Typography
System font stack: `'Inter', ui-sans-serif, system-ui`. No custom weights beyond 400/500/600/700.

| Role | size / weight / color |
|---|---|
| Page title | 20 / 700 / `ink` |
| Section head | 14–15 / 700 / `ink` |
| Card title | 13.5 / 700 / `ink` |
| Body | 13 / 400–500 / `body` |
| Label | 12 / 600 / `body` |
| Caption | 11–11.5 / 500–600 / `muted` |
| Micro-label (UPPER) | 10.5–11 / 700 / `muted`, `letterSpacing:.06em`, `textTransform:uppercase` |

### Radii / spacing / shadows
```
radius: chip/pill 999 · input 8–9 · button 9 · card 10–14 · modal 14–16
control heights: 30 (sm) · 32 (filter) · 34 (btn) · 36–40 (input)
gaps: 6 (tight) · 8–10 (default) · 14–16 (section)
page padding: 20 · card padding: 12–20
shadows:
  card       0 1px 2px rgba(16,24,40,.05)
  hover lift 0 6px 16px rgba(16,24,40,.09)
  dropdown   0 12px 32px rgba(0,0,0,.14)
  modal      0 24px 60px rgba(0,0,0,.28)
  side sheet -8px 0 28px rgba(0,0,0,.10)
```

---

## 2. Status / tone map

Pill colors are driven by a status→trio lookup. Use for any status/stage label.

```js
export const TONE = {
  Live:{bg:LH.greenBg,fg:LH.green,bd:LH.greenBd}, Active:{bg:LH.greenBg,fg:LH.green,bd:LH.greenBd},
  Qualified:{bg:LH.greenBg,fg:LH.green,bd:LH.greenBd}, Contacted:{bg:LH.greenBg,fg:LH.green,bd:LH.greenBd},
  Pending:{bg:LH.amberBg,fg:LH.amber,bd:LH.amberBd}, Paused:{bg:LH.amberBg,fg:LH.amber,bd:LH.amberBd},
  'Awaiting human':{bg:LH.amberBg,fg:LH.amber,bd:LH.amberBd}, Rejected:{bg:LH.amberBg,fg:LH.amber,bd:LH.amberBd},
  New:{bg:LH.blueBg,fg:LH.blue,bd:LH.blueBd}, Converted:{bg:LH.blueBg,fg:LH.blue,bd:LH.blueBd}, Completed:{bg:LH.blueBg,fg:LH.blue,bd:LH.blueBd},
  Engaged:{bg:'#f3eefe',fg:'#6d28d9',bd:'#ddd0f7'}, Qualifying:{bg:'#f3eefe',fg:'#6d28d9',bd:'#ddd0f7'},
  Disqualified:{bg:'#fdecec',fg:LH.red,bd:'#f2d0d0'},
  Draft:{bg:'#f1f0ec',fg:'#8b8578',bd:'#e2ddd2'}, Lost:{bg:'#f1f0ec',fg:'#8b8578',bd:'#e2ddd2'}, Duplicate:{bg:'#f1f0ec',fg:'#8b8578',bd:'#e2ddd2'},
};
```

Stage dot colors (kanban/lifecycle):
```js
export const COL_DOT = { New:'#3b82f6', Contacted:'#22c55e', Engaged:'#6d28d9', Qualifying:'#8b5cf6',
  Qualified:'#16a34a', Converted:'#0ea5e9', Disqualified:'#dc2626', Lost:'#6b7280', Rejected:'#b45309', Duplicate:'#64748b' };
```

---

## 3. Icon wrapper

Thin lucide wrapper. Icons are ~1.7–2 stroke, sized 12–18.

```jsx
function Ic({ name, size = 16, color = 'currentColor', sw = 2, style }) {
  const ref = React.useRef(null);
  React.useEffect(() => {
    const el = ref.current; if (!el || !window.lucide) return;
    el.innerHTML = ''; const i = document.createElement('i'); i.setAttribute('data-lucide', name); el.appendChild(i);
    try { window.lucide.createIcons({ nodes:[i], attrs:{ width:size, height:size, 'stroke-width':sw, stroke:color, fill:'none' } }); } catch(e){}
  });
  return <span ref={ref} style={{ display:'inline-flex', alignItems:'center', justifyContent:'center', width:size, height:size, flexShrink:0, ...style }} />;
}
```
(Using `lucide-react`? Replace with `const I = icons[name]; return <I size={size} color={color} />`.)

---

## 4. Component recipes

### Card
```jsx
function Card({ children, style, onClick, hover }) {
  const [h, setH] = React.useState(false);
  return <div onClick={onClick} onMouseEnter={()=>setH(true)} onMouseLeave={()=>setH(false)}
    style={{ background:LH.card, border:'1px solid '+(hover&&h?'#d8cdb8':LH.border), borderRadius:10, transition:'border-color .12s', ...(onClick?{cursor:'pointer'}:{}), ...style }}>
    {children}</div>;
}
```

### Button
```jsx
function Btn({ children, kind='secondary', icon, onClick, style }) {
  const base = { display:'inline-flex', alignItems:'center', gap:6, height:34, padding:'0 13px', borderRadius:9, fontSize:13, fontWeight:600, cursor:'pointer', transition:'all .12s', whiteSpace:'nowrap' };
  const skin = kind==='primary' ? { background:LH.primary, color:'#fff', border:'1px solid '+LH.primary, boxShadow:'0 1px 2px rgba(0,0,0,.05)' }
    : kind==='ghost' ? { background:'transparent', color:LH.primary, border:'1px solid transparent' }
    : { background:'#fff', color:LH.body, border:'1px solid '+LH.border, boxShadow:'0 1px 2px rgba(0,0,0,.04)' };
  return <button onClick={onClick} style={{ ...base, ...skin, ...style }}>
    {icon && <Ic name={icon} size={15} color={kind==='primary'?'#fff':LH.sub} />}{children}</button>;
}
```
- `primary` = dark ink fill (CTA). `secondary` = white + border. `ghost` = text only.

### Pill (status / stage)
```jsx
function Pill({ label, tone }) {
  const t = TONE[tone || label] || TONE.Draft;
  return <span style={{ display:'inline-flex', alignItems:'center', gap:5, padding:'1px 7px', borderRadius:6, fontSize:11, fontWeight:600, background:t.bg, color:t.fg, border:'1px solid '+t.bd, lineHeight:'15px', whiteSpace:'nowrap' }}>{label}</span>;
}
```

### Avatar (photo + initials fallback)
```jsx
const AV_IMG = name => { let h=0,s=String(name||''); for(let i=0;i<s.length;i++) h=(h*31+s.charCodeAt(i))>>>0; return 'https://i.pravatar.cc/80?img='+(1+h%70); };
function LeadAvatar({ name, size=24, style }) {
  const init = String(name||'?').split(' ').map(w=>w[0]).join('').slice(0,2).toUpperCase();
  return <span style={{ position:'relative', width:size, height:size, borderRadius:999, overflow:'hidden', flexShrink:0, display:'inline-flex', alignItems:'center', justifyContent:'center', background:LH.tint, color:LH.primary, fontSize:Math.round(size*0.42), fontWeight:700, ...style }}>{init}
    <img src={AV_IMG(name)} alt="" style={{ position:'absolute', inset:0, width:'100%', height:'100%', objectFit:'cover' }} onError={e=>{e.currentTarget.style.display='none';}} /></span>;
}
```

### Toggle (switch)
```jsx
function Toggle({ on, onChange }) {
  return <button onClick={()=>onChange(!on)} style={{ width:38, height:22, borderRadius:999, border:'none', background:on?LH.primary:'#d9d5cc', position:'relative', cursor:'pointer', flexShrink:0, transition:'background .15s' }}>
    <span style={{ position:'absolute', top:2, left:on?18:2, width:18, height:18, borderRadius:999, background:'#fff', transition:'left .15s' }} /></button>;
}
```

### Input / Textarea
```js
const inp = { width:'100%', height:38, borderRadius:9, border:'1px solid '+LH.border, padding:'0 11px', fontSize:13, color:LH.ink, outline:'none', fontFamily:'inherit', boxSizing:'border-box' };
const lbl = { fontSize:12, fontWeight:600, color:LH.body, marginBottom:6, display:'block' };
// textarea: { ...inp, height:70, padding:10, resize:'vertical', lineHeight:1.5 }
// error state: borderColor LH.red + a 11px LH.red message row below
```

### Dropdown (custom select)
Button-trigger + absolute menu. Signature: `<Dropdown value options={[{value,label}]} placeholder onChange height />`. Trigger = white pill (`height 36, radius 8, border`), menu = white card `radius 9, shadow dropdown, padding 5`, rows `padding 7px 10px, radius 6, hover #f6f4ef`, selected row shows a `check` icon.

### Section micro-label
```jsx
const secLbl = t => <div style={{ fontSize:11, fontWeight:700, letterSpacing:'.06em', color:LH.muted, margin:'0 0 12px', textTransform:'uppercase' }}>{t}</div>;
```

### Removable chip + "add" dropdown (card customize pattern)
```jsx
// selected chip
<span style={{ display:'inline-flex', alignItems:'center', gap:6, height:30, padding:'0 6px 0 11px', borderRadius:999, border:'1px solid '+LH.primary, background:LH.tint, fontSize:12.5, fontWeight:600, color:LH.ink }}>
  <Ic name="tag" size={13} color={LH.primary} />{label}
  <button onClick={remove} style={{ width:18, height:18, borderRadius:999, border:'none', background:'none', cursor:'pointer' }}><Ic name="x" size={12} color={LH.sub} /></button>
</span>
// "+ Add" trigger: dashed pill (border:'1px dashed '+LH.border) that opens a searchable menu.
```

---

## 5. Layout patterns

### Right side sheet
```jsx
<div style={{ position:'fixed', top:0, right:0, bottom:0, width:'min(640px,96vw)', background:'#fff',
  borderLeft:'1px solid '+LH.border, boxShadow:'-8px 0 28px rgba(0,0,0,.16)', display:'flex', flexDirection:'column',
  transform: open?'translateX(0)':'translateX(101%)', transition:'transform .28s cubic-bezier(0.32,0.72,0,1)' }}>
  <header flexShrink:0 /* title + close */ />
  <div style={{ flex:1, overflowY:'auto', padding:20 }}>…</div>
  <footer flexShrink:0 /* Cancel + primary */ />
</div>
```
- Header/footer are `flexShrink:0`; only the middle scrolls.
- Tabs row: underline-style — active tab `color:ink, borderBottom:'2px solid '+ink`.

### Tabs (underline)
```jsx
{TABS.map(([k,l]) => <button key={k} onClick={()=>setTab(k)} style={{ padding:'11px 8px', fontSize:12.5, fontWeight:600,
  color: tab===k?LH.ink:LH.sub, background:'none', border:'none', borderBottom:'2px solid '+(tab===k?LH.ink:'transparent'), marginBottom:-1, cursor:'pointer' }}>{l}</button>)}
```

### Key-value detail row
```jsx
const kv = (k, v) => <div style={{ display:'flex', gap:12, padding:'6px 0' }}>
  <span style={{ fontSize:11, color:LH.muted, width:104, flexShrink:0 }}>{k}</span>
  <span style={{ fontSize:12, color:LH.body, fontWeight:500, flex:1, minWidth:0 }}>{v}</span></div>;
```

### Lifecycle chevron bar (continuous breadcrumb)
Rounded pill (`borderRadius:999, overflow:hidden`), each segment is a right-pointing chevron that **overlaps** the next (hairline `>` dividers). Reached = `green` fill, current = `primary` (ink), upcoming = `tint`.
```jsx
const P = 14; // point depth
// per segment:
const clip = last ? 'polygon(0 0,100% 0,100% 100%,0 100%)'
  : 'polygon(0 0, calc(100% - '+P+'px) 0, 100% 50%, calc(100% - '+P+'px) 100%, 0 100%)';
<div style={{ flex:1, height:38, background:bg, clipPath:clip, marginLeft:first?0:-P+1.5, zIndex:total-i,
  display:'flex', alignItems:'center', justifyContent:'center', color:on?'#fff':LH.sub }}>{label}</div>
```

### Communication timeline row
Icon tile + vertical connector + card. Channel tiles: SMS `blueBg/blue`, Call `greenBg/green`, Email `amberBg/amber`.
```jsx
<div style={{ display:'flex', gap:12 }}>
  <div style={{ display:'flex', flexDirection:'column', alignItems:'center', width:30 }}>
    <div style={{ width:30, height:30, borderRadius:8, background:m.bg, display:'flex', alignItems:'center', justifyContent:'center' }}><Ic name={m.ic} size={14} color={m.fg} /></div>
    {!last && <div style={{ flex:1, width:1, background:LH.borderSoft, minHeight:14, marginTop:4 }} />}
  </div>
  <div>{/* label + In/Out tag + time, then title + preview */}</div>
</div>
```

### Chat bubble (conversation)
Outbound = right-aligned `blueBg` bubble; inbound = left `ink` bubble, white text; call events = bordered card; day dividers = centered pill `"Today"/"Mon d, yyyy"`.

### Bulk-action bar (floating)
Dark pill docked bottom-center: `background:LH.ink, color:#fff, borderRadius:12, boxShadow:'0 10px 30px rgba(0,0,0,.28)'`, with `N selected` + action buttons (`rgba(255,255,255,.08)` fill) + dropdowns.

### Floating dropdown menu (anchored)
`position:fixed/absolute, background:#fff, border:'1px solid '+LH.border, borderRadius:10–12, boxShadow dropdown, padding:5`; rows hover `#f6f4ef`.

---

## 6. Interaction rules

- **Hover reveal**: row actions (call/mail/edit icons) start `opacity:0`, fade to `1` on row hover via `onMouseEnter` toggling a `[data-*]` child's opacity.
- **Transitions**: `.12s` for color/opacity, `.28s cubic-bezier(0.32,0.72,0,1)` for sheets/slides.
- **Selected**: border → `primary`, soft ring `0 0 0 3px rgba(17,24,39,.07)`.
- **Portals bubble through the React tree** — stop `onClick` propagation on modal roots nested inside another overlay.
- **Empty/placeholder** text → `muted`; destructive → `red`.

---

## 7. Restyle prompt (paste to Claude)

> Using `LEAD-HUB-DESIGN-SYSTEM.md`, restyle the attached component to the Lead Hub system:
> - swap colors to the `LH` tokens (pick Zuper palette),
> - use the Pill/Btn/Card/Dropdown recipes where they fit,
> - match radii (card 10–14, pill 999, input 9), control heights (btn 34, input 38, filter 32),
> - apply the typography scale and the hover/selected/transition rules.
> Keep all behavior and props; change styling only. Output the updated component.
