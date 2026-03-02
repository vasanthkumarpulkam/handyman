import { useState, useEffect, useRef } from "react";

/* ══════════════════════════════════════════════════════════
   FIXR — DUAL EXPERIENCE MARKETPLACE
   Customer: Warm editorial, consumer-first, trust-forward
   Provider: Dark pro dashboard, data-dense, earnings-focused
══════════════════════════════════════════════════════════ */

// ─── CUSTOMER TOKENS ─────────────────────────────────────
const C = {
  bg:        "#FAFAF7",
  white:     "#FFFFFF",
  surface:   "#FFFFFF",
  ink:       "#1A1A2E",
  body:      "#4A5568",
  muted:     "#9AA5B4",
  line:      "#ECEEF2",
  green:     "#0B7B5E",
  greenLight:"#E6F5F0",
  greenMid:  "#14A07A",
  amber:     "#F59B2B",
  amberLight:"#FEF5E4",
  red:       "#E8453C",
  redLight:  "#FDECEA",
  teal:      "#0D9488",
};

// ─── PROVIDER TOKENS ─────────────────────────────────────
const P = {
  bg:        "#0F1923",
  surface:   "#16222E",
  card:      "#1C2D3E",
  border:    "#243447",
  ink:       "#F0F4F8",
  body:      "#94A3B8",
  muted:     "#4A6278",
  blue:      "#2D8CFF",
  blueLight: "#1A3A5C",
  gold:      "#F5A623",
  goldDark:  "#2A2010",
  green:     "#10B981",
  greenDark: "#0A2018",
  red:       "#EF4444",
  redDark:   "#2A0F0F",
  orange:    "#F97316",
};

// ─── DATA ────────────────────────────────────────────────
const CATS = [
  { icon:"🔧", name:"Plumbing",    n:142 },
  { icon:"⚡", name:"Electrical",  n:98  },
  { icon:"🎨", name:"Painting",    n:203 },
  { icon:"🪚", name:"Carpentry",   n:76  },
  { icon:"❄️", name:"HVAC",        n:89  },
  { icon:"🏠", name:"Roofing",     n:67  },
  { icon:"🧹", name:"Cleaning",    n:311 },
  { icon:"🪟", name:"Windows",     n:54  },
];

const PROS = [
  { id:1, name:"Marcus Thompson",  role:"Master Plumber",       rating:4.9, reviews:312, jobs:847,  price:85,  dist:"0.8", avail:true,  premium:true,  init:"MT", color:"#0B7B5E" },
  { id:2, name:"Sarah Kowalski",   role:"Licensed Electrician", rating:4.8, reviews:198, jobs:523,  price:95,  dist:"1.2", avail:true,  premium:true,  init:"SK", color:"#2563EB" },
  { id:3, name:"David Reyes",      role:"General Handyman",     rating:4.7, reviews:89,  jobs:234,  price:65,  dist:"2.1", avail:false, premium:false, init:"DR", color:"#7C3AED" },
  { id:4, name:"Yolanda Martinez", role:"Finish Carpenter",     rating:4.9, reviews:156, jobs:412,  price:75,  dist:"3.0", avail:true,  premium:false, init:"YM", color:"#DC2626" },
];

const JOBS = [
  { id:1, title:"Leaking Pipe Under Kitchen Sink",     cat:"Plumbing",   budget:"$120–$200", bids:4,  age:"12m",  urgent:true,  zip:"10001", desc:"Kitchen sink has been dripping for 3 days. Water pooling under cabinet." },
  { id:2, title:"Electrical Outlet Not Working",        cat:"Electrical", budget:"$80–$150",  bids:2,  age:"38m",  urgent:false, zip:"10002", desc:"Two outlets in living room stopped working after circuit breaker tripped." },
  { id:3, title:"Fence Panel Replacement (3 panels)",  cat:"Carpentry",  budget:"$300–$500", bids:7,  age:"1hr",  urgent:false, zip:"10001", desc:"Storm damaged 3 wood fence panels on north side of property." },
  { id:4, title:"Interior Repaint – 2BR Apartment",    cat:"Painting",   budget:"$400–$700", bids:9,  age:"2hr",  urgent:false, zip:"10003", desc:"Full interior repaint needed. Walls and ceilings, ~900 sq ft total." },
  { id:5, title:"HVAC Filter & Annual Tune-Up",        cat:"HVAC",       budget:"$150–$250", bids:3,  age:"3hr",  urgent:false, zip:"10001", desc:"Annual maintenance service for 2-ton central AC unit." },
];

const MY_BIDS = [
  { job:"Leaking Pipe Under Kitchen Sink",   amount:155, status:"pending",  ago:"2h",  profit:132 },
  { job:"Fence Panel Replacement",           amount:380, status:"accepted", ago:"1d",  profit:323 },
  { job:"HVAC Filter & Tune-Up",             amount:195, status:"pending",  ago:"4h",  profit:166 },
];

const EARNINGS = [
  { label:"Mon", val:240 },
  { label:"Tue", val:380 },
  { label:"Wed", val:175 },
  { label:"Thu", val:520 },
  { label:"Fri", val:390 },
  { label:"Sat", val:680 },
  { label:"Sun", val:150 },
];

/* ══════════════════════════════════════════════════════════
   ROOT — MODE SWITCHER
══════════════════════════════════════════════════════════ */
export default function Root() {
  const [mode, setMode] = useState(null); // null | "customer" | "provider"

  if (!mode) return <Splash setMode={setMode} />;
  if (mode === "customer") return <CustomerApp switchMode={() => setMode(null)} />;
  return <ProviderApp switchMode={() => setMode(null)} />;
}

/* ── SPLASH ──────────────────────────────────────────────── */
function Splash({ setMode }) {
  return (
    <div style={{ minHeight:"100vh", background:"#0F1923", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", padding:24, fontFamily:"'Nunito', 'Helvetica Neue', sans-serif" }}>
      <div style={{ marginBottom:48, textAlign:"center" }}>
        <div style={{ fontSize:52, marginBottom:8 }}>🔩</div>
        <div style={{ fontSize:40, fontWeight:900, color:"#F0F4F8", letterSpacing:-1 }}>fixr</div>
        <div style={{ fontSize:14, color:"#4A6278", marginTop:6, letterSpacing:1 }}>HYPER-LOCAL HANDYMAN MARKETPLACE</div>
      </div>

      <div style={{ width:"100%", maxWidth:340 }}>
        <div style={{ fontSize:11, fontWeight:700, letterSpacing:2, color:"#4A6278", textAlign:"center", marginBottom:16 }}>I AM A…</div>

        <button onClick={() => setMode("customer")} style={{ width:"100%", background:"linear-gradient(135deg, #0B7B5E, #14A07A)", border:"none", borderRadius:20, padding:"22px 28px", marginBottom:14, cursor:"pointer", textAlign:"left", position:"relative", overflow:"hidden" }}>
          <div style={{ position:"absolute", right:-20, top:-20, fontSize:80, opacity:0.12 }}>🏠</div>
          <div style={{ fontSize:11, fontWeight:800, letterSpacing:2, color:"rgba(255,255,255,0.7)", marginBottom:6 }}>CUSTOMER</div>
          <div style={{ fontSize:20, fontWeight:900, color:"white", marginBottom:4 }}>I need work done</div>
          <div style={{ fontSize:13, color:"rgba(255,255,255,0.75)", lineHeight:1.4 }}>Post jobs, get competitive bids, hire trusted local pros</div>
          <div style={{ marginTop:16, display:"flex", gap:8, flexWrap:"wrap" }}>
            {["Post & Bid","Direct Hire","Emergency Map","Secure Pay"].map(f => (
              <span key={f} style={{ fontSize:10, background:"rgba(255,255,255,0.15)", color:"white", padding:"4px 10px", borderRadius:20, fontWeight:700 }}>{f}</span>
            ))}
          </div>
        </button>

        <button onClick={() => setMode("provider")} style={{ width:"100%", background:"linear-gradient(135deg, #1C2D3E, #243447)", border:"1.5px solid #2D8CFF33", borderRadius:20, padding:"22px 28px", cursor:"pointer", textAlign:"left", position:"relative", overflow:"hidden" }}>
          <div style={{ position:"absolute", right:-20, top:-20, fontSize:80, opacity:0.08 }}>🔧</div>
          <div style={{ fontSize:11, fontWeight:800, letterSpacing:2, color:"#2D8CFF99", marginBottom:6 }}>SERVICE PROVIDER</div>
          <div style={{ fontSize:20, fontWeight:900, color:"#F0F4F8", marginBottom:4 }}>I offer services</div>
          <div style={{ fontSize:13, color:"#4A6278", lineHeight:1.4 }}>Browse leads, submit bids, manage jobs & track earnings</div>
          <div style={{ marginTop:16, display:"flex", gap:8, flexWrap:"wrap" }}>
            {["Lead Board","Bid Tools","Earnings","Analytics"].map(f => (
              <span key={f} style={{ fontSize:10, background:"rgba(45,140,255,0.12)", color:"#2D8CFF", padding:"4px 10px", borderRadius:20, fontWeight:700 }}>{f}</span>
            ))}
          </div>
        </button>
      </div>

      <div style={{ marginTop:32, fontSize:11, color:"#243447", textAlign:"center" }}>
        Already have an account? <span style={{ color:"#2D8CFF", fontWeight:700, cursor:"pointer" }}>Sign in</span>
      </div>
    </div>
  );
}

/* ══════════════════════════════════════════════════════════
   CUSTOMER APP — warm editorial, consumer-trust-forward
══════════════════════════════════════════════════════════ */
function CustomerApp({ switchMode }) {
  const [tab, setTab] = useState("home");
  const [jobSel, setJobSel] = useState(null);
  const [postStep, setPostStep] = useState(1);
  const [chatOpen, setChatOpen] = useState(false);
  const [chatPro, setChatPro] = useState(null);
  const [msgs, setMsgs] = useState([{ from:"pro", text:"Hi! I can be there by 3 PM. Can you confirm the address?" }]);
  const [draft, setDraft] = useState("");
  const [bidSel, setBidSel] = useState(null);
  const chatRef = useRef(null);
  useEffect(() => { chatRef.current?.scrollIntoView({ behavior:"smooth" }); }, [msgs]);

  const openChat = (pro) => { setChatPro(pro); setChatOpen(true); };
  const sendMsg = () => {
    if (!draft.trim()) return;
    setMsgs(m => [...m, { from:"me", text:draft }]);
    setDraft("");
    setTimeout(() => setMsgs(m => [...m, { from:"pro", text:"Perfect — I'm on my way now!" }]), 900);
  };

  const BIDS_FOR_JOB = [
    { name:"Marcus T.", co:"ProFix Solutions", amt:155, eta:"Today 3 PM",    rating:4.9, rev:312, init:"MT", color:"#0B7B5E", badge:"Top Pro", match:96 },
    { name:"Raj Patel", co:"Patel Plumbing",   amt:140, eta:"Today 5:30 PM", rating:4.6, rev:89,  init:"RP", color:"#2563EB", badge:null,     match:88 },
    { name:"Kim Lee",   co:"K&L Services",     amt:175, eta:"Tomorrow 9 AM", rating:4.8, rev:201, init:"KL", color:"#7C3AED", badge:null,     match:92 },
  ];

  const custTabs = [
    { id:"home",      icon:"⌂",  label:"Home"      },
    { id:"search",    icon:"⊙",  label:"Services"  },
    { id:"post",      icon:"+",  label:"Post"      },
    { id:"myjobs",    icon:"≡",  label:"My Jobs"   },
    { id:"profile",   icon:"○",  label:"Account"   },
  ];

  return (
    <div style={{ fontFamily:"'Nunito', 'Helvetica Neue', sans-serif", background:C.bg, minHeight:"100vh", maxWidth:430, margin:"0 auto", color:C.ink }}>

      {/* ── CUSTOMER HEADER ── */}
      <header style={{ background:C.white, padding:"12px 18px", display:"flex", justifyContent:"space-between", alignItems:"center", borderBottom:`1px solid ${C.line}`, position:"sticky", top:0, zIndex:40, boxShadow:"0 1px 8px rgba(0,0,0,0.04)" }}>
        <div style={{ display:"flex", alignItems:"center", gap:8 }}>
          <div style={{ width:34, height:34, background:`linear-gradient(135deg,${C.green},${C.greenMid})`, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", fontSize:17 }}>🔩</div>
          <div>
            <div style={{ fontSize:18, fontWeight:900, color:C.ink, letterSpacing:-0.5, lineHeight:1 }}>fixr</div>
            <div style={{ fontSize:9, color:C.muted, letterSpacing:1, fontWeight:700 }}>FOR CUSTOMERS</div>
          </div>
        </div>
        <div style={{ display:"flex", gap:8, alignItems:"center" }}>
          <div style={{ background:C.greenLight, color:C.green, fontSize:11, fontWeight:700, padding:"5px 10px", borderRadius:20, display:"flex", alignItems:"center", gap:4 }}>
            <span>⚡</span><span>PRO</span>
          </div>
          <div style={{ background:C.amberLight, padding:"6px 10px", borderRadius:20, fontSize:11, fontWeight:700, color:C.amber, display:"flex", alignItems:"center", gap:4 }}>
            <span>📍</span><span>10001</span>
          </div>
        </div>
      </header>

      {/* ── CUSTOMER MAIN ── */}
      <main style={{ paddingBottom:88 }}>
        {tab === "home"    && <CustHome   setTab={setTab} openChat={openChat} />}
        {tab === "search"  && <CustSearch openChat={openChat} />}
        {tab === "post"    && <CustPost   step={postStep} setStep={setPostStep} />}
        {tab === "myjobs"  && <CustMyJobs setJobSel={setJobSel} jobSel={jobSel} bids={BIDS_FOR_JOB} bidSel={bidSel} setBidSel={setBidSel} openChat={openChat} />}
        {tab === "profile" && <CustProfile switchMode={switchMode} />}
      </main>

      {/* ── CUSTOMER NAV ── */}
      <nav style={{ position:"fixed", bottom:0, left:"50%", transform:"translateX(-50%)", width:"100%", maxWidth:430, background:C.white, borderTop:`1px solid ${C.line}`, display:"flex", zIndex:40, paddingBottom:16, boxShadow:"0 -4px 16px rgba(0,0,0,0.06)" }}>
        {custTabs.map(t => (
          <button key={t.id} onClick={() => setTab(t.id)} style={{ flex:1, border:"none", background:"none", cursor:"pointer", padding:"10px 0 2px", display:"flex", flexDirection:"column", alignItems:"center", gap:3 }}>
            {t.id === "post"
              ? <div style={{ width:50, height:50, background:`linear-gradient(135deg,${C.green},${C.greenMid})`, borderRadius:"50%", display:"flex", alignItems:"center", justifyContent:"center", fontSize:24, color:"white", fontWeight:700, boxShadow:`0 4px 16px ${C.green}55`, position:"relative", top:-14, border:`3px solid ${C.bg}` }}>{t.icon}</div>
              : <>
                  <span style={{ fontSize:20, color: tab===t.id ? C.green : C.muted }}>{t.icon}</span>
                  <span style={{ fontSize:9, fontWeight:800, letterSpacing:0.3, color: tab===t.id ? C.green : C.muted, textTransform:"uppercase" }}>{t.label}</span>
                  {tab===t.id && <div style={{ position:"absolute", top:0, width:24, height:2.5, background:C.green, borderRadius:2 }} />}
                </>
            }
          </button>
        ))}
      </nav>

      {/* ── CHAT DRAWER ── */}
      {chatOpen && (
        <div onClick={e=>e.target===e.currentTarget&&setChatOpen(false)} style={{ position:"fixed", inset:0, background:"rgba(26,26,46,0.55)", zIndex:80, display:"flex", alignItems:"flex-end", justifyContent:"center", backdropFilter:"blur(2px)" }}>
          <div style={{ background:C.white, width:"100%", maxWidth:430, borderRadius:"24px 24px 0 0", boxShadow:"0 -8px 40px rgba(0,0,0,0.18)", maxHeight:"72vh", display:"flex", flexDirection:"column" }}>
            <div style={{ display:"flex", justifyContent:"center", padding:"10px 0 0" }}><div style={{ width:36, height:4, background:C.line, borderRadius:2 }} /></div>
            <div style={{ display:"flex", alignItems:"center", gap:12, padding:"10px 20px 14px", borderBottom:`1px solid ${C.line}` }}>
              <div style={{ width:46, height:46, background:chatPro?.color??C.green, borderRadius:14, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:900, fontSize:15 }}>{chatPro?.init??"MT"}</div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:900, fontSize:15, color:C.ink }}>{chatPro?.name??"Marcus Thompson"}</div>
                <div style={{ fontSize:12, color:C.greenMid, fontWeight:700 }}>● Online now · Responding fast</div>
              </div>
              <button onClick={()=>setChatOpen(false)} style={{ background:C.line, border:"none", width:30, height:30, borderRadius:"50%", fontSize:14, cursor:"pointer", color:C.body }}>✕</button>
            </div>
            <div style={{ flex:1, overflowY:"auto", padding:"14px 20px", display:"flex", flexDirection:"column", gap:8 }}>
              {msgs.map((m,i)=>(
                <div key={i} style={{ display:"flex", justifyContent:m.from==="me"?"flex-end":"flex-start" }}>
                  <div style={{ maxWidth:"76%", background:m.from==="me"?C.green:C.line, color:m.from==="me"?"white":C.ink, borderRadius:m.from==="me"?"18px 18px 4px 18px":"18px 18px 18px 4px", padding:"10px 14px", fontSize:14, lineHeight:1.5 }}>{m.text}</div>
                </div>
              ))}
              <div ref={chatRef} />
            </div>
            <div style={{ padding:"10px 16px 8px", borderTop:`1px solid ${C.line}`, display:"flex", gap:8 }}>
              <input value={draft} onChange={e=>setDraft(e.target.value)} onKeyDown={e=>e.key==="Enter"&&sendMsg()} placeholder="Message…" style={{ flex:1, background:C.bg, border:`1.5px solid ${C.line}`, borderRadius:24, padding:"11px 16px", fontSize:14, color:C.ink, outline:"none", fontFamily:"inherit" }} />
              <button onClick={sendMsg} style={{ width:44, height:44, background:C.green, border:"none", borderRadius:"50%", color:"white", fontSize:18, cursor:"pointer", flexShrink:0, display:"flex", alignItems:"center", justifyContent:"center" }}>→</button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

/* ── CUSTOMER HOME ───────────────────────────────────────── */
function CustHome({ setTab, openChat }) {
  const [pulse, setPulse] = useState(true);
  useEffect(() => { const t = setInterval(()=>setPulse(p=>!p), 1600); return ()=>clearInterval(t); }, []);

  return (
    <div>
      {/* ── HERO SEARCH ── */}
      <div style={{ background:`linear-gradient(160deg, ${C.green} 0%, #0a6347 100%)`, padding:"28px 20px 80px", position:"relative", overflow:"hidden" }}>
        <div style={{ position:"absolute", top:-60, right:-60, width:220, height:220, background:"rgba(255,255,255,0.04)", borderRadius:"50%" }} />
        <div style={{ position:"absolute", bottom:-80, left:-40, width:180, height:180, background:"rgba(255,255,255,0.04)", borderRadius:"50%" }} />
        <div style={{ fontSize:11, color:"rgba(255,255,255,0.65)", fontWeight:700, letterSpacing:2, marginBottom:8 }}>GOOD MORNING, JAMES 👋</div>
        <h1 style={{ fontSize:28, fontWeight:900, color:"white", lineHeight:1.2, marginBottom:6, position:"relative" }}>
          Find trusted pros<br/>in your neighborhood
        </h1>
        <p style={{ fontSize:14, color:"rgba(255,255,255,0.75)", marginBottom:20, lineHeight:1.5 }}>Post your job and let the pros compete for your business</p>
        <div style={{ background:"white", borderRadius:16, padding:"12px 16px", display:"flex", alignItems:"center", gap:10, boxShadow:"0 8px 32px rgba(0,0,0,0.2)", position:"relative", zIndex:2 }}>
          <span style={{ fontSize:18 }}>🔍</span>
          <span style={{ fontSize:14, color:C.muted, flex:1 }}>What do you need fixed?</span>
          <div style={{ background:C.green, padding:"7px 14px", borderRadius:10, fontSize:12, fontWeight:800, color:"white" }}>Search</div>
        </div>
      </div>

      {/* ── FEATURE PILLS overlapping hero ── */}
      <div style={{ display:"flex", gap:8, padding:"0 16px", marginTop:-28, position:"relative", zIndex:3, overflowX:"auto", paddingBottom:4 }}>
        {[
          { icon:"📋", label:"Post & Bid", sub:"Pros compete", color:C.amber, bg:C.amberLight },
          { icon:"⚡", label:"Emergency", sub:"Instant connect", color:C.red, bg:C.redLight },
          { icon:"🔍", label:"Browse Pros", sub:"Filter & hire", color:C.teal, bg:"#E0F7F5" },
          { icon:"🔒", label:"Secure Pay", sub:"Escrow held", color:C.green, bg:C.greenLight },
        ].map(f => (
          <div key={f.label} style={{ flexShrink:0, background:C.white, borderRadius:16, padding:"10px 12px", boxShadow:"0 2px 12px rgba(0,0,0,0.08)", display:"flex", alignItems:"center", gap:8, border:`1px solid ${C.line}` }}>
            <div style={{ width:36, height:36, background:f.bg, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", fontSize:18 }}>{f.icon}</div>
            <div>
              <div style={{ fontSize:12, fontWeight:800, color:C.ink }}>{f.label}</div>
              <div style={{ fontSize:10, color:C.muted }}>{f.sub}</div>
            </div>
          </div>
        ))}
      </div>

      {/* ── EMERGENCY BANNER ── */}
      <div style={{ margin:"16px 16px 0", background:`linear-gradient(135deg, #B91C1C, #991B1B)`, borderRadius:18, padding:"16px 18px", display:"flex", justifyContent:"space-between", alignItems:"center", boxShadow:`0 6px 20px rgba(184,28,28,0.3)`, transition:"box-shadow 0.8s", boxShadow: pulse ? "0 6px 28px rgba(184,28,28,0.45)" : "0 4px 14px rgba(184,28,28,0.2)" }}>
        <div>
          <div style={{ fontSize:10, fontWeight:700, letterSpacing:2, color:"rgba(255,255,255,0.65)", marginBottom:4 }}>PREMIUM FEATURE</div>
          <div style={{ fontSize:17, fontWeight:900, color:"white", marginBottom:3 }}>🚨 Emergency Connect</div>
          <div style={{ fontSize:12, color:"rgba(255,255,255,0.8)" }}>8 pros online · Instant direct chat/call</div>
        </div>
        <button onClick={() => {}} style={{ background:"white", color:"#B91C1C", border:"none", padding:"11px 16px", borderRadius:12, fontWeight:800, fontSize:12, cursor:"pointer", whiteSpace:"nowrap" }}>
          Connect Now
        </button>
      </div>

      {/* ── HOW IT WORKS ── */}
      <div style={{ padding:"22px 16px 0" }}>
        <SH title="How FIXR Works" sub="3 simple steps" />
        <div style={{ display:"flex", gap:10 }}>
          {[
            { n:"1", icon:"📝", t:"Post Your Job", d:"Describe it, set budget, add photos" },
            { n:"2", icon:"📊", t:"Compare Bids", d:"Side-by-side quotes from vetted pros" },
            { n:"3", icon:"✅", t:"Hire & Pay", d:"Secure escrow released on completion" },
          ].map(s => (
            <div key={s.n} style={{ flex:1, background:C.white, border:`1px solid ${C.line}`, borderRadius:18, padding:"14px 12px", textAlign:"center" }}>
              <div style={{ width:32, height:32, background:C.greenLight, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", fontSize:16, margin:"0 auto 8px" }}>{s.icon}</div>
              <div style={{ fontWeight:800, fontSize:11, color:C.ink, marginBottom:3 }}>{s.t}</div>
              <div style={{ fontSize:10, color:C.muted, lineHeight:1.4 }}>{s.d}</div>
            </div>
          ))}
        </div>
      </div>

      {/* ── CATEGORIES ── */}
      <div style={{ padding:"20px 16px 0" }}>
        <SH title="Browse Services" action="All →" />
        <div style={{ display:"grid", gridTemplateColumns:"repeat(4,1fr)", gap:8 }}>
          {CATS.map(c => (
            <div key={c.name} style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:16, padding:"14px 8px", textAlign:"center", cursor:"pointer" }}>
              <div style={{ fontSize:26, marginBottom:5 }}>{c.icon}</div>
              <div style={{ fontSize:10, fontWeight:800, color:C.ink }}>{c.name}</div>
              <div style={{ fontSize:9, color:C.muted, marginTop:2 }}>{c.n} pros</div>
            </div>
          ))}
        </div>
      </div>

      {/* ── TOP PROS ── */}
      <div style={{ padding:"20px 16px 0" }}>
        <SH title="Top Pros Near You" sub="Sponsored" />
        {PROS.filter(p=>p.premium).map(p => <CustProCard key={p.id} p={p} openChat={() => openChat(p)} />)}
      </div>

      {/* ── LOCAL REVIEWS ── */}
      <LocalReviews />
    </div>
  );
}

const LOCAL_REVIEWS = [
  { name:"Rachel M.",    init:"RM", color:"#7C3AED", zip:"10001", pro:"Marcus T.",  service:"Plumbing",   rating:5, ago:"2 days ago",  text:"Pipe was fixed in under an hour. Marcus was punctual, clean, and charged exactly what he quoted. Couldn't ask for more." },
  { name:"Tom G.",       init:"TG", color:"#DC2626", zip:"10002", pro:"Sarah K.",   service:"Electrical", rating:5, ago:"4 days ago",  text:"Sarah diagnosed the outlet issue immediately. She explained everything clearly. Will absolutely hire again." },
  { name:"Priya S.",     init:"PS", color:"#0B7B5E", zip:"10001", pro:"David R.",   service:"Carpentry",  rating:4, ago:"1 week ago",  text:"Fence looks great. Took a bit longer than expected but the quality of work was really solid. Good communication throughout." },
  { name:"James W.",     init:"JW", color:"#2563EB", zip:"10003", pro:"Yolanda M.", service:"Painting",   rating:5, ago:"1 week ago",  text:"Best painter I've hired through any app. Yolanda transformed our living room. On time, tidy, and super professional." },
];

function LocalReviews() {
  return (
    <div style={{ padding:"20px 16px 28px" }}>
      <SH title="What Neighbors Are Saying" sub="Verified reviews · ZIP 10001" />

      {/* aggregate trust strip */}
      <div style={{ background:`linear-gradient(135deg, ${C.greenLight}, ${C.amberLight})`, border:`1px solid ${C.green}22`, borderRadius:18, padding:"14px 18px", marginBottom:16, display:"flex", alignItems:"center", gap:14 }}>
        <div style={{ textAlign:"center", minWidth:60 }}>
          <div style={{ fontSize:36, fontWeight:900, color:C.green, lineHeight:1 }}>4.9</div>
          <div style={{ color:C.amber, fontSize:14, marginTop:2 }}>★★★★★</div>
        </div>
        <div style={{ width:1, height:44, background:C.line }} />
        <div style={{ flex:1 }}>
          <div style={{ fontWeight:800, fontSize:13, color:C.ink }}>Trusted by your community</div>
          <div style={{ fontSize:12, color:C.body, marginTop:3, lineHeight:1.4 }}>
            <strong style={{ color:C.green }}>2,147 reviews</strong> from customers in your area in the last 90 days
          </div>
        </div>
      </div>

      {/* review cards — horizontal scroll */}
      <div style={{ display:"flex", gap:12, overflowX:"auto", paddingBottom:8, marginLeft:-16, paddingLeft:16, marginRight:-16, paddingRight:16 }}>
        {LOCAL_REVIEWS.map((r, i) => (
          <div key={i} style={{ flexShrink:0, width:268, background:C.white, border:`1px solid ${C.line}`, borderRadius:20, padding:"16px", boxShadow:"0 2px 12px rgba(0,0,0,0.05)" }}>
            {/* stars + service badge */}
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:10 }}>
              <div style={{ color:C.amber, fontSize:13, letterSpacing:1 }}>{"★".repeat(r.rating)}{"☆".repeat(5 - r.rating)}</div>
              <span style={{ fontSize:9, background:C.greenLight, color:C.green, fontWeight:700, padding:"2px 8px", borderRadius:10 }}>{r.service}</span>
            </div>

            {/* review text */}
            <p style={{ fontSize:13, color:C.body, lineHeight:1.55, margin:"0 0 14px", fontStyle:"italic" }}>
              "{r.text}"
            </p>

            {/* reviewer + pro hired */}
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", paddingTop:12, borderTop:`1px solid ${C.line}` }}>
              <div style={{ display:"flex", alignItems:"center", gap:8 }}>
                <div style={{ width:30, height:30, background:r.color, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:800, fontSize:11 }}>{r.init}</div>
                <div>
                  <div style={{ fontWeight:700, fontSize:12, color:C.ink }}>{r.name}</div>
                  <div style={{ fontSize:10, color:C.muted }}>ZIP {r.zip} · {r.ago}</div>
                </div>
              </div>
              <div style={{ textAlign:"right" }}>
                <div style={{ fontSize:9, color:C.muted, fontWeight:700 }}>HIRED</div>
                <div style={{ fontSize:11, fontWeight:800, color:C.green }}>{r.pro}</div>
              </div>
            </div>
          </div>
        ))}
      </div>

      {/* CTA under reviews */}
      <div style={{ marginTop:16, background:C.white, border:`1px solid ${C.line}`, borderRadius:18, padding:"16px 18px", display:"flex", justifyContent:"space-between", alignItems:"center" }}>
        <div>
          <div style={{ fontWeight:800, fontSize:14, color:C.ink }}>Ready to get started?</div>
          <div style={{ fontSize:12, color:C.muted, marginTop:2 }}>Post a job in under 2 minutes</div>
        </div>
        <button style={{ background:C.green, color:"white", border:"none", padding:"11px 18px", borderRadius:12, fontWeight:800, fontSize:13, cursor:"pointer", whiteSpace:"nowrap" }}>
          Post a Job →
        </button>
      </div>
    </div>
  );
}

function SH({ title, sub, action }) {
  return (
    <div style={{ display:"flex", justifyContent:"space-between", alignItems:"baseline", marginBottom:12 }}>
      <div>
        <span style={{ fontSize:15, fontWeight:900, color:C.ink }}>{title}</span>
        {sub && <span style={{ fontSize:10, color:C.muted, marginLeft:6 }}>{sub}</span>}
      </div>
      {action && <span style={{ fontSize:12, color:C.green, fontWeight:700, cursor:"pointer" }}>{action}</span>}
    </div>
  );
}

function CustProCard({ p, openChat }) {
  return (
    <div style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:20, padding:"16px", marginBottom:12, position:"relative" }}>
      {p.premium && <div style={{ position:"absolute", top:12, right:12, background:C.amberLight, color:C.amber, fontSize:9, fontWeight:800, padding:"3px 8px", borderRadius:20, letterSpacing:0.5 }}>★ SPONSORED</div>}
      <div style={{ display:"flex", gap:12, alignItems:"center" }}>
        <div style={{ width:58, height:58, background:p.color, borderRadius:18, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:900, fontSize:18, flexShrink:0, position:"relative" }}>
          {p.init}
          {p.avail && <div style={{ position:"absolute", bottom:1, right:1, width:13, height:13, background:"#22C55E", borderRadius:"50%", border:"2px solid white" }} />}
        </div>
        <div style={{ flex:1 }}>
          <div style={{ fontWeight:900, fontSize:15, color:C.ink }}>{p.name}</div>
          <div style={{ fontSize:12, color:C.body, marginTop:1 }}>{p.role}</div>
          <div style={{ display:"flex", alignItems:"center", gap:5, marginTop:4 }}>
            <span style={{ color:C.amber, fontSize:12 }}>{"★".repeat(Math.floor(p.rating))}</span>
            <span style={{ fontSize:12, fontWeight:700, color:C.ink }}>{p.rating}</span>
            <span style={{ fontSize:11, color:C.muted }}>({p.reviews})</span>
          </div>
        </div>
      </div>
      <div style={{ display:"flex", gap:8, marginTop:12 }}>
        {[["$"+p.price+"/hr","Rate"],[ p.dist+" mi","Away"],[p.jobs+"+","Jobs"]].map(([v,l])=>(
          <div key={l} style={{ flex:1, background:C.bg, borderRadius:12, padding:"8px", textAlign:"center" }}>
            <div style={{ fontWeight:900, fontSize:13, color:C.green }}>{v}</div>
            <div style={{ fontSize:9, color:C.muted, marginTop:1, fontWeight:700 }}>{l}</div>
          </div>
        ))}
      </div>
      <div style={{ display:"flex", gap:8, marginTop:10 }}>
        <button onClick={openChat} style={{ flex:1, background:C.green, color:"white", border:"none", borderRadius:12, padding:"11px", fontWeight:800, fontSize:13, cursor:"pointer" }}>Request Quote</button>
        <button style={{ width:44, height:44, background:C.greenLight, border:"none", borderRadius:12, fontSize:16, cursor:"pointer", color:C.green }}>💬</button>
      </div>
    </div>
  );
}

/* ── CUSTOMER SEARCH / BROWSE PROS ───────────────────────── */
function CustSearch({ openChat }) {
  const [filter, setFilter]     = useState("All");
  const [isPremium, setIsPremium] = useState(false); // toggle to demo both states
  const [upsellPro, setUpsellPro] = useState(null);  // pro that triggered upsell modal

  const handleHire = (p) => {
    if (isPremium) { openChat(p); }
    else           { setUpsellPro(p); }
  };

  return (
    <div style={{ padding:"16px" }}>

      {/* ── Upsell modal overlay ── */}
      {upsellPro && (
        <div onClick={()=>setUpsellPro(null)} style={{ position:"fixed", inset:0, background:"rgba(26,26,46,0.6)", zIndex:90, display:"flex", alignItems:"flex-end", justifyContent:"center", backdropFilter:"blur(3px)" }}>
          <div onClick={e=>e.stopPropagation()} style={{ background:C.white, width:"100%", maxWidth:430, borderRadius:"28px 28px 0 0", padding:"28px 24px 36px", boxShadow:"0 -8px 48px rgba(0,0,0,0.2)" }}>
            {/* handle */}
            <div style={{ width:36, height:4, background:C.line, borderRadius:2, margin:"0 auto 20px" }} />

            {/* pro preview — blurred/locked feel */}
            <div style={{ background:C.bg, border:`1px solid ${C.line}`, borderRadius:16, padding:"14px 16px", marginBottom:20, display:"flex", gap:12, alignItems:"center" }}>
              <div style={{ width:46, height:46, background:upsellPro.color, borderRadius:14, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:900, fontSize:15 }}>{upsellPro.init}</div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:800, fontSize:14, color:C.ink }}>{upsellPro.name}</div>
                <div style={{ fontSize:12, color:C.body }}>{upsellPro.role} · {upsellPro.dist} mi away</div>
              </div>
              {/* lock icon over hire button */}
              <div style={{ background:C.line, borderRadius:10, padding:"8px 12px", display:"flex", alignItems:"center", gap:5 }}>
                <span style={{ fontSize:14 }}>🔒</span>
                <span style={{ fontSize:11, fontWeight:800, color:C.muted }}>Locked</span>
              </div>
            </div>

            <div style={{ textAlign:"center", marginBottom:20 }}>
              <div style={{ fontSize:28, marginBottom:8 }}>⚡</div>
              <div style={{ fontWeight:900, fontSize:20, color:C.ink, marginBottom:6 }}>Direct Hire is a Premium Feature</div>
              <div style={{ fontSize:14, color:C.body, lineHeight:1.6 }}>
                Upgrade to <strong style={{ color:C.green }}>FIXR Premium</strong> to contact pros directly, skip the bidding queue, and get same-day responses.
              </div>
            </div>

            {/* what you get */}
            <div style={{ background:C.greenLight, border:`1px solid ${C.green}22`, borderRadius:16, padding:"14px 16px", marginBottom:20 }}>
              <div style={{ fontWeight:800, fontSize:12, color:C.green, marginBottom:10, letterSpacing:0.5 }}>PREMIUM INCLUDES</div>
              {[
                ["💬", "Direct hire & instant chat with any pro"],
                ["⚡", "Emergency Map — connect with online pros now"],
                ["🚫", "Zero booking fees on every job"],
                ["🔔", "Priority notifications when pros are available"],
              ].map(([ic, txt]) => (
                <div key={txt} style={{ display:"flex", gap:10, alignItems:"flex-start", marginBottom:8 }}>
                  <span style={{ fontSize:15 }}>{ic}</span>
                  <span style={{ fontSize:13, color:C.body, lineHeight:1.4 }}>{txt}</span>
                </div>
              ))}
            </div>

            {/* pricing */}
            <div style={{ display:"flex", alignItems:"baseline", justifyContent:"center", gap:4, marginBottom:16 }}>
              <span style={{ fontSize:36, fontWeight:900, color:C.ink }}>$9.99</span>
              <span style={{ fontSize:14, color:C.muted, fontWeight:600 }}>/month</span>
            </div>

            <button
              onClick={() => { setIsPremium(true); setUpsellPro(null); }}
              style={{ width:"100%", background:`linear-gradient(135deg, ${C.green}, ${C.greenMid})`, color:"white", border:"none", padding:"15px", borderRadius:16, fontWeight:900, fontSize:15, cursor:"pointer", boxShadow:`0 4px 16px ${C.green}44`, marginBottom:10 }}
            >
              Upgrade to Premium →
            </button>
            <button onClick={()=>setUpsellPro(null)} style={{ width:"100%", background:"none", border:"none", color:C.muted, fontSize:13, fontWeight:600, cursor:"pointer", padding:"6px" }}>
              Not now — post a job instead
            </button>
          </div>
        </div>
      )}

      {/* ── Header ── */}
      <div style={{ fontWeight:900, fontSize:22, color:C.ink, marginBottom:4 }}>Find a Pro</div>
      <div style={{ fontSize:13, color:C.muted, marginBottom:14 }}>Verified · Background-checked · Rated</div>

      {/* ── Premium status bar ── */}
      {isPremium
        ? <div style={{ background:C.greenLight, border:`1px solid ${C.green}33`, borderRadius:14, padding:"10px 14px", marginBottom:14, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
            <div style={{ display:"flex", alignItems:"center", gap:8 }}>
              <span style={{ fontSize:16 }}>⚡</span>
              <span style={{ fontWeight:700, fontSize:13, color:C.green }}>Premium Active — Direct Hire Unlocked</span>
            </div>
            <button onClick={()=>setIsPremium(false)} style={{ fontSize:10, color:C.muted, background:"none", border:"none", cursor:"pointer" }}>demo ↩</button>
          </div>
        : <div style={{ background:"linear-gradient(135deg, #1A1A2E, #2C1F4A)", border:"1px solid #4C3A7A55", borderRadius:14, padding:"12px 16px", marginBottom:14, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
            <div>
              <div style={{ fontSize:12, fontWeight:800, color:"#C084FC" }}>🔒 Direct Hire requires Premium</div>
              <div style={{ fontSize:11, color:"#9CA3AF", marginTop:2 }}>Free members can still post jobs &amp; receive bids</div>
            </div>
            <button onClick={()=>setIsPremium(true)} style={{ background:"#7C3AED", color:"white", border:"none", padding:"8px 12px", borderRadius:10, fontSize:11, fontWeight:800, cursor:"pointer", whiteSpace:"nowrap" }}>
              Upgrade ⚡
            </button>
          </div>
      }

      {/* ── Search bar ── */}
      <div style={{ background:C.white, border:`1.5px solid ${C.line}`, borderRadius:14, padding:"11px 14px", display:"flex", gap:10, marginBottom:14 }}>
        <span>🔍</span>
        <input placeholder="Search by service or name…" style={{ flex:1, border:"none", outline:"none", fontSize:14, background:"transparent", color:C.ink, fontFamily:"inherit" }} />
      </div>

      {/* ── Category filters ── */}
      <div style={{ display:"flex", gap:8, overflowX:"auto", paddingBottom:4, marginBottom:16 }}>
        {["All","Plumbing","Electrical","Painting","HVAC","Carpentry"].map(f => (
          <button key={f} onClick={()=>setFilter(f)} style={{ background:f===filter?C.green:C.white, color:f===filter?"white":C.body, border:`1.5px solid ${f===filter?C.green:C.line}`, padding:"7px 14px", borderRadius:20, fontSize:12, fontWeight:700, cursor:"pointer", whiteSpace:"nowrap" }}>{f}</button>
        ))}
      </div>

      {/* ── Smart Match banner ── */}
      <div style={{ background:`linear-gradient(135deg, ${C.greenLight}, ${C.amberLight})`, border:`1px solid ${C.green}22`, borderRadius:16, padding:"12px 16px", marginBottom:16, display:"flex", gap:10, alignItems:"center" }}>
        <span style={{ fontSize:22 }}>🎯</span>
        <div>
          <div style={{ fontWeight:800, fontSize:13, color:C.ink }}>Smart Match Active</div>
          <div style={{ fontSize:11, color:C.body, marginTop:1 }}>Pros ranked by compatibility with your job history &amp; ZIP</div>
        </div>
      </div>

      {/* ── Pro cards ── */}
      {PROS.map(p => {
        const matchScore = 94 - p.id * 2;
        return (
          <div key={p.id} style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:20, padding:"16px", marginBottom:12, position:"relative", overflow:"hidden" }}>
            {p.premium && (
              <div style={{ fontSize:9, background:C.amberLight, color:C.amber, fontWeight:800, padding:"3px 8px", borderRadius:10, display:"inline-block", marginBottom:8 }}>★ TOP PRO</div>
            )}

            <div style={{ display:"flex", gap:12 }}>
              {/* avatar */}
              <div style={{ width:52, height:52, background:p.color, borderRadius:16, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:800, fontSize:16, flexShrink:0, position:"relative" }}>
                {p.init}
                <div style={{ position:"absolute", bottom:0, right:0, width:12, height:12, background:p.avail?"#22C55E":"#94A3B8", borderRadius:"50%", border:"2px solid white" }} />
              </div>

              {/* info */}
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:800, fontSize:14, color:C.ink }}>{p.name}</div>
                <div style={{ fontSize:12, color:C.body }}>{p.role}</div>
                <div style={{ display:"flex", gap:6, alignItems:"center", marginTop:4, flexWrap:"wrap" }}>
                  <span style={{ color:C.amber, fontSize:11 }}>{"★".repeat(Math.floor(p.rating))}</span>
                  <span style={{ fontSize:11, fontWeight:700, color:C.ink }}>{p.rating}</span>
                  <span style={{ fontSize:10, color:C.muted }}>({p.reviews})</span>
                  <span style={{ fontSize:10, color:C.muted }}>· {p.dist} mi</span>
                </div>
              </div>

              {/* price + action */}
              <div style={{ textAlign:"right", flexShrink:0 }}>
                <div style={{ fontWeight:900, fontSize:15, color:C.green }}>${p.price}/hr</div>
                {isPremium
                  ? <button
                      onClick={() => handleHire(p)}
                      style={{ marginTop:6, background:C.green, color:"white", border:"none", padding:"8px 14px", borderRadius:10, fontSize:11, fontWeight:800, cursor:"pointer", display:"flex", alignItems:"center", gap:4 }}
                    >
                      💬 Hire
                    </button>
                  : <button
                      onClick={() => handleHire(p)}
                      style={{ marginTop:6, background:"#F3F0FF", color:"#7C3AED", border:"1.5px solid #C084FC55", padding:"8px 10px", borderRadius:10, fontSize:11, fontWeight:800, cursor:"pointer", display:"flex", alignItems:"center", gap:4 }}
                    >
                      🔒 Hire
                    </button>
                }
              </div>
            </div>

            {/* match score bar */}
            <div style={{ marginTop:12 }}>
              <div style={{ display:"flex", justifyContent:"space-between", marginBottom:4 }}>
                <span style={{ fontSize:10, color:C.muted, fontWeight:700 }}>MATCH SCORE</span>
                <span style={{ fontSize:10, color:C.green, fontWeight:800 }}>{matchScore}%</span>
              </div>
              <div style={{ height:5, background:C.line, borderRadius:3 }}>
                <div style={{ height:5, width:`${matchScore}%`, background:`linear-gradient(90deg, ${C.green}, ${C.greenMid})`, borderRadius:3, transition:"width 0.5s" }} />
              </div>
            </div>

            {/* free-tier nudge inline — subtle, not aggressive */}
            {!isPremium && (
              <div style={{ marginTop:12, paddingTop:12, borderTop:`1px solid ${C.line}`, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
                <span style={{ fontSize:11, color:C.muted }}>🔒 Direct contact requires Premium</span>
                <span style={{ fontSize:11, color:C.green, fontWeight:700, cursor:"pointer" }}>Post a job instead →</span>
              </div>
            )}
          </div>
        );
      })}
    </div>
  );
}

/* ── CUSTOMER POST JOB ───────────────────────────────────── */
function CustPost({ step, setStep }) {
  const [form, setForm] = useState({ cat:"", title:"", desc:"", urgency:"Standard", min:"", max:"", timeline:"This Week" });
  const up = f => setForm(p=>({...p,...f}));

  return (
    <div style={{ padding:"16px" }}>
      <div style={{ fontWeight:900, fontSize:22, color:C.ink }}>Post a Job</div>
      <div style={{ fontSize:13, color:C.muted, marginBottom:18 }}>Post & Relax — pros bid, you pick the best</div>

      {/* stepper */}
      <div style={{ display:"flex", alignItems:"center", marginBottom:20 }}>
        {["Details","Budget","Preview"].map((s,i)=>(
          <div key={s} style={{ display:"flex", alignItems:"center", flex:1 }}>
            <div style={{ display:"flex", flexDirection:"column", alignItems:"center" }}>
              <div style={{ width:32, height:32, borderRadius:"50%", background:i+1<=step?C.green:C.line, display:"flex", alignItems:"center", justifyContent:"center", fontSize:13, fontWeight:900, color:i+1<=step?"white":C.muted }}>
                {i+1<step?"✓":i+1}
              </div>
              <span style={{ fontSize:9, fontWeight:700, color:i+1===step?C.green:C.muted, marginTop:3, letterSpacing:0.5 }}>{s.toUpperCase()}</span>
            </div>
            {i<2 && <div style={{ flex:1, height:2, background:i+1<step?C.green:C.line, margin:"0 6px", marginBottom:14 }} />}
          </div>
        ))}
      </div>

      {step===1 && (
        <div style={{ background:C.white, borderRadius:20, border:`1px solid ${C.line}`, padding:16 }}>
          <CL>Service Category</CL>
          <div style={{ display:"grid", gridTemplateColumns:"repeat(4,1fr)", gap:8, marginBottom:4 }}>
            {CATS.map(c=>(
              <div key={c.name} onClick={()=>up({cat:c.name})} style={{ background:form.cat===c.name?C.greenLight:C.bg, border:`1.5px solid ${form.cat===c.name?C.green:C.line}`, borderRadius:14, padding:"12px 6px", textAlign:"center", cursor:"pointer" }}>
                <div style={{ fontSize:22 }}>{c.icon}</div>
                <div style={{ fontSize:9, fontWeight:700, color:form.cat===c.name?C.green:C.body, marginTop:3 }}>{c.name}</div>
              </div>
            ))}
          </div>
          <CL>Job Title</CL>
          <CI value={form.title} onChange={e=>up({title:e.target.value})} placeholder="e.g. Leaking pipe under kitchen sink" />
          <CL>Description</CL>
          <textarea value={form.desc} onChange={e=>up({desc:e.target.value})} placeholder="Describe the problem in detail. What's wrong? Where? Any relevant context?" rows={3} style={{ ...cinput, resize:"none" }} />
          <CL>Urgency</CL>
          <div style={{ display:"flex", gap:8, marginBottom:20 }}>
            {[["Standard","📅"],["ASAP","⚡"],["Emergency","🚨"]].map(([u,ic])=>(
              <div key={u} onClick={()=>up({urgency:u})} style={{ flex:1, background:form.urgency===u?(u==="Emergency"?C.redLight:C.greenLight):C.bg, border:`1.5px solid ${form.urgency===u?(u==="Emergency"?C.red:C.green):C.line}`, borderRadius:12, padding:"10px 4px", textAlign:"center", cursor:"pointer" }}>
                <div style={{ fontSize:16 }}>{ic}</div>
                <div style={{ fontSize:10, fontWeight:700, color:form.urgency===u?(u==="Emergency"?C.red:C.green):C.muted, marginTop:2 }}>{u}</div>
              </div>
            ))}
          </div>
          <button onClick={()=>setStep(2)} disabled={!form.title} style={{ width:"100%", background:form.title?C.green:"#ccc", color:"white", border:"none", padding:14, borderRadius:14, fontWeight:800, fontSize:14, cursor:form.title?"pointer":"not-allowed" }}>Continue →</button>
        </div>
      )}

      {step===2 && (
        <div style={{ background:C.white, borderRadius:20, border:`1px solid ${C.line}`, padding:16 }}>
          <CL>Upload Photos</CL>
          <div style={{ border:`2px dashed ${C.line}`, borderRadius:16, padding:"28px", textAlign:"center", marginBottom:4, cursor:"pointer", background:C.bg }}>
            <div style={{ fontSize:36, marginBottom:6 }}>📸</div>
            <div style={{ fontWeight:700, fontSize:13, color:C.ink }}>Add photos of the issue</div>
            <div style={{ fontSize:11, color:C.muted, marginTop:2 }}>Helps pros give accurate quotes · Up to 10 photos</div>
          </div>
          <CL>Budget Range</CL>
          <div style={{ display:"flex", gap:10, alignItems:"center" }}>
            <div style={{ flex:1, position:"relative" }}>
              <span style={{ position:"absolute", left:12, top:"50%", transform:"translateY(-50%)", color:C.muted, fontWeight:700 }}>$</span>
              <input value={form.min} onChange={e=>up({min:e.target.value})} placeholder="Min" style={{ ...cinput, paddingLeft:26 }} />
            </div>
            <span style={{ color:C.muted }}>—</span>
            <div style={{ flex:1, position:"relative" }}>
              <span style={{ position:"absolute", left:12, top:"50%", transform:"translateY(-50%)", color:C.muted, fontWeight:700 }}>$</span>
              <input value={form.max} onChange={e=>up({max:e.target.value})} placeholder="Max" style={{ ...cinput, paddingLeft:26 }} />
            </div>
          </div>
          <CL>Timeline</CL>
          <div style={{ display:"flex", gap:8, marginBottom:20 }}>
            {["Today","This Week","Flexible"].map(t=>(
              <div key={t} onClick={()=>up({timeline:t})} style={{ flex:1, background:form.timeline===t?C.greenLight:C.bg, border:`1.5px solid ${form.timeline===t?C.green:C.line}`, borderRadius:12, padding:10, textAlign:"center", fontSize:12, fontWeight:700, color:form.timeline===t?C.green:C.muted, cursor:"pointer" }}>{t}</div>
            ))}
          </div>
          <div style={{ display:"flex", gap:10 }}>
            <button onClick={()=>setStep(1)} style={{ flex:1, background:C.bg, border:`1.5px solid ${C.line}`, color:C.body, padding:13, borderRadius:14, fontWeight:700, cursor:"pointer", fontSize:13 }}>← Back</button>
            <button onClick={()=>setStep(3)} style={{ flex:2, background:C.green, color:"white", border:"none", padding:13, borderRadius:14, fontWeight:800, fontSize:13, cursor:"pointer" }}>Preview →</button>
          </div>
        </div>
      )}

      {step===3 && (
        <div style={{ background:C.white, borderRadius:20, border:`1px solid ${C.line}`, padding:16 }}>
          <div style={{ background:C.greenLight, border:`1px solid ${C.green}33`, borderRadius:14, padding:"14px 16px", marginBottom:16, display:"flex", gap:10, alignItems:"center" }}>
            <div style={{ width:40, height:40, background:C.green, borderRadius:12, display:"flex", alignItems:"center", justifyContent:"center", fontSize:20 }}>✓</div>
            <div>
              <div style={{ fontWeight:900, fontSize:15, color:C.green }}>Ready to post!</div>
              <div style={{ fontSize:12, color:C.body, marginTop:1 }}>Will reach 47 verified pros in ZIP 10001</div>
            </div>
          </div>
          {[["Category",form.cat||"Plumbing"],["Job",form.title||"Leaking pipe under sink"],["Urgency",form.urgency],["Budget",`$${form.min||100} – $${form.max||200}`],["Timeline",form.timeline],["ZIP","10001"]].map(([k,v])=>(
            <div key={k} style={{ display:"flex", justifyContent:"space-between", padding:"11px 0", borderBottom:`1px solid ${C.line}` }}>
              <span style={{ fontSize:13, color:C.muted, fontWeight:600 }}>{k}</span>
              <span style={{ fontSize:13, fontWeight:800, color:C.ink }}>{v}</span>
            </div>
          ))}
          <div style={{ background:C.amberLight, borderRadius:12, padding:"12px 14px", margin:"14px 0", fontSize:12, color:C.ink, lineHeight:1.6 }}>
            ⚡ <strong>Tip:</strong> Jobs marked ASAP get <strong>3× more bids</strong> within the first hour.
          </div>
          <div style={{ display:"flex", gap:10 }}>
            <button onClick={()=>setStep(2)} style={{ flex:1, background:C.bg, border:`1.5px solid ${C.line}`, color:C.body, padding:13, borderRadius:14, fontWeight:700, cursor:"pointer", fontSize:13 }}>← Edit</button>
            <button onClick={()=>setStep(1)} style={{ flex:2, background:C.green, color:"white", border:"none", padding:13, borderRadius:14, fontWeight:800, fontSize:14, cursor:"pointer" }}>🚀 Post Job</button>
          </div>
        </div>
      )}
    </div>
  );
}

function CL({ children }) { return <div style={{ fontSize:11, fontWeight:800, color:C.muted, letterSpacing:0.8, marginBottom:6, marginTop:14 }}>{children}</div>; }
const cinput = { width:"100%", background:"#F7F9FB", border:`1.5px solid #ECEEF2`, borderRadius:12, padding:"11px 14px", fontSize:14, color:"#1A1A2E", boxSizing:"border-box", outline:"none", fontFamily:"inherit", display:"block" };
function CI({ ...props }) { return <input style={cinput} {...props} />; }

/* ── CUSTOMER MY JOBS ────────────────────────────────────── */
function CustMyJobs({ setJobSel, jobSel, bids, bidSel, setBidSel, openChat }) {
  if (jobSel) return (
    <div style={{ padding:"16px" }}>
      <button onClick={()=>setJobSel(null)} style={{ background:"none", border:"none", color:C.green, fontSize:13, fontWeight:700, cursor:"pointer", padding:"0 0 14px" }}>← My Jobs</button>
      <div style={{ background:C.white, borderRadius:20, border:`1px solid ${C.line}`, padding:18 }}>
        <div style={{ fontSize:18, fontWeight:900, color:C.ink, marginBottom:6, lineHeight:1.3 }}>{jobSel.title}</div>
        <div style={{ display:"flex", gap:6, flexWrap:"wrap", marginBottom:14 }}>
          <span style={{ fontSize:10, background:C.greenLight, color:C.green, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>{jobSel.cat}</span>
          <span style={{ fontSize:10, background:C.amberLight, color:C.amber, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>{jobSel.budget}</span>
          <span style={{ fontSize:10, background:C.bg, color:C.muted, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>📍 {jobSel.zip}</span>
          {jobSel.urgent && <span style={{ fontSize:10, background:C.redLight, color:C.red, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>🔥 Urgent</span>}
        </div>
        <div style={{ background:C.bg, borderRadius:12, padding:"12px 14px", fontSize:13, color:C.body, lineHeight:1.6, marginBottom:18 }}>{jobSel.desc}</div>

        {/* ── UNIQUE: Side-by-side bid comparison ── */}
        <div style={{ fontWeight:900, fontSize:14, color:C.ink, marginBottom:4 }}>Compare Bids ({bids.length})</div>
        <div style={{ fontSize:11, color:C.muted, marginBottom:12 }}>Tap a bid to expand · Green = Best Value</div>
        {bids.map((b,i)=>(
          <div key={i} onClick={()=>setBidSel(bidSel===i?null:i)} style={{ border:`1.5px solid ${bidSel===i?C.green:i===1?C.green+"44":C.line}`, background:bidSel===i?C.greenLight:i===1?`${C.green}08`:C.white, borderRadius:16, padding:"14px 16px", marginBottom:10, cursor:"pointer", transition:"all 0.2s" }}>
            {i===1 && <div style={{ fontSize:9, background:C.green, color:"white", fontWeight:800, padding:"2px 8px", borderRadius:10, display:"inline-block", marginBottom:8 }}>✦ BEST VALUE · ${bids[0].amt - b.amt} less than highest</div>}
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start" }}>
              <div style={{ display:"flex", gap:10, alignItems:"center" }}>
                <div style={{ width:40, height:40, background:b.color, borderRadius:12, display:"flex", alignItems:"center", justifyContent:"center", color:"white", fontWeight:800, fontSize:13 }}>{b.init}</div>
                <div>
                  <div style={{ fontWeight:800, fontSize:14, color:C.ink }}>{b.name}</div>
                  <div style={{ fontSize:11, color:C.muted }}>{b.co}</div>
                  <div style={{ fontSize:11, color:C.amber, marginTop:2 }}>{"★".repeat(Math.floor(b.rating))} <span style={{ color:C.body }}>{b.rating} ({b.rev})</span></div>
                </div>
              </div>
              <div style={{ textAlign:"right" }}>
                <div style={{ fontWeight:900, fontSize:22, color:C.green }}>${b.amt}</div>
                <div style={{ fontSize:10, color:C.muted, marginTop:2 }}>flat rate</div>
              </div>
            </div>
            {bidSel===i && (
              <div style={{ marginTop:14, paddingTop:14, borderTop:`1px solid ${C.line}` }}>
                <div style={{ fontSize:12, color:C.body, marginBottom:10 }}>🕐 Available: <strong>{b.eta}</strong></div>
                <div style={{ fontSize:12, color:C.body, marginBottom:10 }}>🎯 Match score: <strong style={{ color:C.green }}>{b.match}%</strong></div>
                <div style={{ display:"flex", gap:8 }}>
                  <button onClick={()=>openChat(b)} style={{ flex:1, background:C.green, color:"white", border:"none", borderRadius:12, padding:11, fontWeight:800, fontSize:13, cursor:"pointer" }}>✓ Accept & Chat</button>
                  <button style={{ flex:1, background:C.bg, border:`1.5px solid ${C.line}`, color:C.body, borderRadius:12, padding:11, fontWeight:700, fontSize:12, cursor:"pointer" }}>View Profile</button>
                </div>
              </div>
            )}
          </div>
        ))}
      </div>
    </div>
  );

  return (
    <div style={{ padding:"16px" }}>
      <div style={{ fontWeight:900, fontSize:22, color:C.ink, marginBottom:4 }}>My Jobs</div>
      <div style={{ fontSize:13, color:C.muted, marginBottom:16 }}>Track, compare bids & manage active work</div>

      {/* Active jobs */}
      <div style={{ fontWeight:800, fontSize:13, color:C.ink, marginBottom:10, letterSpacing:0.3 }}>ACTIVE JOBS</div>
      {JOBS.slice(0,3).map(j=>(
        <div key={j.id} onClick={()=>setJobSel(j)} style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:18, padding:"14px 16px", marginBottom:10, cursor:"pointer" }}>
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start" }}>
            <div style={{ flex:1 }}>
              <div style={{ display:"flex", gap:5, marginBottom:5 }}>
                {j.urgent && <span style={{ fontSize:9, background:C.redLight, color:C.red, fontWeight:700, padding:"2px 7px", borderRadius:10 }}>🔥 URGENT</span>}
                <span style={{ fontSize:9, background:C.greenLight, color:C.green, fontWeight:700, padding:"2px 7px", borderRadius:10 }}>{j.cat}</span>
                <span style={{ fontSize:9, background:C.amberLight, color:C.amber, fontWeight:700, padding:"2px 7px", borderRadius:10 }}>OPEN</span>
              </div>
              <div style={{ fontWeight:800, fontSize:14, color:C.ink, lineHeight:1.3 }}>{j.title}</div>
              <div style={{ fontSize:11, color:C.muted, marginTop:4 }}>Posted {j.age} ago</div>
            </div>
            <div style={{ textAlign:"right", marginLeft:10, flexShrink:0 }}>
              <div style={{ fontWeight:900, fontSize:14, color:C.green }}>{j.budget}</div>
              <div style={{ marginTop:4, background:C.amberLight, color:C.amber, fontSize:10, fontWeight:800, padding:"3px 8px", borderRadius:10, display:"inline-block" }}>{j.bids} bids →</div>
            </div>
          </div>
        </div>
      ))}

      {/* Completed */}
      <div style={{ fontWeight:800, fontSize:13, color:C.ink, margin:"18px 0 10px", letterSpacing:0.3 }}>COMPLETED</div>
      {[
        { title:"Bathroom faucet replaced", pro:"Marcus T.", date:"Feb 12", paid:"$165", rating:5 },
        { title:"Light fixture installation", pro:"Sarah K.", date:"Jan 28", paid:"$120", rating:5 },
      ].map((j,i)=>(
        <div key={i} style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:16, padding:"12px 16px", marginBottom:8 }}>
          <div style={{ display:"flex", justifyContent:"space-between" }}>
            <div>
              <div style={{ fontWeight:700, fontSize:13, color:C.ink }}>{j.title}</div>
              <div style={{ fontSize:11, color:C.muted, marginTop:2 }}>{j.pro} · {j.date}</div>
              <div style={{ color:C.amber, fontSize:11, marginTop:3 }}>{"★".repeat(j.rating)}</div>
            </div>
            <div>
              <div style={{ fontWeight:800, color:C.green, fontSize:14 }}>{j.paid}</div>
              <div style={{ fontSize:9, background:C.greenLight, color:C.green, fontWeight:700, padding:"2px 8px", borderRadius:10, marginTop:4, textAlign:"center" }}>DONE</div>
            </div>
          </div>
        </div>
      ))}
    </div>
  );
}

/* ── CUSTOMER PROFILE ────────────────────────────────────── */
function CustProfile({ switchMode }) {
  return (
    <div>
      <div style={{ background:`linear-gradient(160deg, ${C.green}, #0a6347)`, padding:"28px 20px 60px", position:"relative", overflow:"hidden" }}>
        <div style={{ position:"absolute", top:-40, right:-40, width:160, height:160, background:"rgba(255,255,255,0.05)", borderRadius:"50%" }} />
        <div style={{ display:"flex", alignItems:"center", gap:14 }}>
          <div style={{ width:64, height:64, background:"rgba(255,255,255,0.2)", borderRadius:20, display:"flex", alignItems:"center", justifyContent:"center", fontSize:24, fontWeight:900, color:"white", border:"2px solid rgba(255,255,255,0.3)" }}>JW</div>
          <div>
            <div style={{ fontWeight:900, fontSize:20, color:"white" }}>James Williams</div>
            <div style={{ fontSize:13, color:"rgba(255,255,255,0.75)", marginTop:2 }}>Member since Jan 2024 · ZIP 10001</div>
            <div style={{ marginTop:6, display:"inline-flex", alignItems:"center", gap:5, background:"rgba(255,255,255,0.15)", padding:"4px 10px", borderRadius:20 }}>
              <span style={{ fontSize:11 }}>⚡</span>
              <span style={{ fontSize:11, fontWeight:700, color:"white" }}>PREMIUM MEMBER</span>
            </div>
          </div>
        </div>
      </div>

      {/* stats card overlapping hero */}
      <div style={{ margin:"0 16px", marginTop:-30, background:C.white, borderRadius:18, border:`1px solid ${C.line}`, padding:"14px 8px", display:"flex", boxShadow:"0 4px 20px rgba(0,0,0,0.08)", position:"relative", zIndex:2 }}>
        {[["6","Jobs Posted"],["4","Completed"],["$825","Total Spent"],["★ 4.9","Avg Rating"]].map(([v,l],i,a)=>(
          <div key={l} style={{ flex:1, textAlign:"center", borderRight:i<a.length-1?`1px solid ${C.line}`:"none" }}>
            <div style={{ fontWeight:900, fontSize:16, color:C.green }}>{v}</div>
            <div style={{ fontSize:9, color:C.muted, marginTop:2, fontWeight:700 }}>{l}</div>
          </div>
        ))}
      </div>

      <div style={{ padding:"16px" }}>
        {/* Premium features */}
        <div style={{ background:C.amberLight, border:`1px solid ${C.amber}33`, borderRadius:16, padding:"14px 16px", marginBottom:16 }}>
          <div style={{ fontWeight:800, fontSize:13, color:C.amber, marginBottom:8 }}>⚡ Your Premium Features</div>
          {["Emergency Connect — instant chat with online pros","Zero booking fees on all hires","Priority placement in provider notifications","Dedicated support line"].map((f,i)=>(
            <div key={i} style={{ display:"flex", gap:8, alignItems:"flex-start", marginBottom:5 }}>
              <span style={{ color:C.green, fontWeight:900, fontSize:13 }}>✓</span>
              <span style={{ fontSize:12, color:C.body }}>{f}</span>
            </div>
          ))}
        </div>

        {/* Settings */}
        <div style={{ fontWeight:800, fontSize:12, color:C.muted, letterSpacing:0.8, marginBottom:10 }}>ACCOUNT SETTINGS</div>
        <div style={{ background:C.white, border:`1px solid ${C.line}`, borderRadius:16, overflow:"hidden" }}>
          {[["👤","Edit Profile",""],["💳","Payment Methods","Visa ···4242"],["🔔","Notifications","On"],["⚡","Subscription","Premium"],["⭐","My Reviews","4 reviews"],["🔒","Privacy",""],["❓","Help & Support",""],["🔄","Switch to Provider", "switchMode"]].map(([ic,lbl,sub],i,a)=>(
            <div key={lbl} onClick={lbl==="Switch to Provider"?switchMode:undefined} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"13px 16px", borderBottom:i<a.length-1?`1px solid ${C.line}`:"none", cursor:"pointer" }}>
              <div style={{ display:"flex", alignItems:"center", gap:10 }}>
                <span style={{ fontSize:16 }}>{ic}</span>
                <span style={{ fontSize:14, fontWeight:600, color:lbl.includes("Switch")?C.green:C.ink }}>{lbl}</span>
              </div>
              <div style={{ display:"flex", alignItems:"center", gap:6 }}>
                {sub && <span style={{ fontSize:11, color:C.muted }}>{sub}</span>}
                <span style={{ color:C.muted, fontSize:18 }}>›</span>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

/* ══════════════════════════════════════════════════════════
   PROVIDER APP — dark pro dashboard, earnings-first, data-dense
══════════════════════════════════════════════════════════ */
// ─── PROVIDER-LEVEL STATE (shared across all screens via props) ───────────────
// isPremiumProv : true  = Premium Provider ($49.99/mo)
//                 false = Regular Provider  (3-bid cap, lead fee on acceptance)
// activeBids    : count of currently open bids (regular cap = 3)
// serviceZIPs   : ZIPs the provider covers (sponsors top placement if premium)
// liveLocation  : simulated GPS ZIP derived from device (or manual override)

function ProviderApp({ switchMode }) {
  const [tab,          setTab]          = useState("dash");
  const [selJob,       setSelJob]       = useState(null);
  const [bidAmt,       setBidAmt]       = useState("");
  const [bidMsg,       setBidMsg]       = useState("");
  const [bidSent,      setBidSent]      = useState(false);
  const [isPremiumProv,setIsPremiumProv]= useState(false);   // DEMO toggle
  const [activeBids,   setActiveBids]   = useState(2);       // regular starts at 2/3 used
  const [serviceZIPs,  setServiceZIPs]  = useState(["10001","10002","10003"]);
  const [liveZIP,      setLiveZIP]      = useState("10001"); // simulated GPS ZIP
  const [leadFeeJob,   setLeadFeeJob]   = useState(null);    // triggers fee modal
  const [feePaid,      setFeePaid]      = useState([]);      // job IDs fee already paid

  const BID_CAP = 3;
  const LEAD_FEE = 7; // $7 per accepted lead for regular providers

  const provTabs = [
    { id:"dash",   icon:"▦", label:"Dashboard" },
    { id:"leads",  icon:"◈", label:"Leads"     },
    { id:"bids",   icon:"◎", label:"My Bids"   },
    { id:"earn",   icon:"$", label:"Earnings"  },
    { id:"profile",icon:"○", label:"Profile"   },
  ];

  const sharedProps = { isPremiumProv, setIsPremiumProv, activeBids, setActiveBids,
    BID_CAP, serviceZIPs, setServiceZIPs, liveZIP, setLiveZIP,
    leadFeeJob, setLeadFeeJob, feePaid, setFeePaid, LEAD_FEE };

  return (
    <div style={{ fontFamily:"'Nunito','Helvetica Neue',sans-serif", background:P.bg, minHeight:"100vh", maxWidth:430, margin:"0 auto", color:P.ink }}>

      {/* ── HEADER ── */}
      <header style={{ background:P.surface, padding:"12px 18px", display:"flex", justifyContent:"space-between", alignItems:"center", borderBottom:`1px solid ${P.border}`, position:"sticky", top:0, zIndex:40 }}>
        <div style={{ display:"flex", alignItems:"center", gap:10 }}>
          <div style={{ width:34, height:34, background:`linear-gradient(135deg,${P.blue},#1a68cc)`, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", fontSize:17 }}>🔧</div>
          <div>
            <div style={{ fontSize:18, fontWeight:900, color:P.ink, letterSpacing:-0.5, lineHeight:1 }}>fixr</div>
            <div style={{ fontSize:9, color:P.blue, letterSpacing:1, fontWeight:700 }}>PRO DASHBOARD</div>
          </div>
        </div>
        <div style={{ display:"flex", gap:8, alignItems:"center" }}>
          {isPremiumProv
            ? <div style={{ background:P.goldDark, border:`1px solid ${P.gold}44`, color:P.gold, fontSize:10, fontWeight:800, padding:"5px 10px", borderRadius:20 }}>★ PREMIUM PRO</div>
            : <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", color:"#C084FC", fontSize:10, fontWeight:800, padding:"5px 10px", borderRadius:20 }}>FREE · {activeBids}/{BID_CAP} bids</div>
          }
          <div style={{ width:10, height:10, background:P.green, borderRadius:"50%", boxShadow:`0 0 6px ${P.green}` }} />
          <span style={{ fontSize:11, color:P.green, fontWeight:700 }}>Online</span>
        </div>
      </header>

      <main style={{ paddingBottom:88 }}>
        {tab==="dash"    && <ProvDash    {...sharedProps} />}
        {tab==="leads"   && <ProvLeads   {...sharedProps} selJob={selJob} setSelJob={setSelJob} bidAmt={bidAmt} setBidAmt={setBidAmt} bidMsg={bidMsg} setBidMsg={setBidMsg} bidSent={bidSent} setBidSent={setBidSent} />}
        {tab==="bids"    && <ProvBids    {...sharedProps} setLeadFeeJob={setLeadFeeJob} />}
        {tab==="earn"    && <ProvEarn    {...sharedProps} />}
        {tab==="profile" && <ProvProfile {...sharedProps} switchMode={switchMode} />}
      </main>

      {/* ── LEAD FEE MODAL — fires when regular provider's bid is accepted ── */}
      {leadFeeJob && (
        <div style={{ position:"fixed", inset:0, background:"rgba(0,0,0,0.75)", zIndex:90, display:"flex", alignItems:"center", justifyContent:"center", padding:20 }}>
          <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:24, padding:"28px 24px", width:"100%", maxWidth:380 }}>
            <div style={{ textAlign:"center", marginBottom:20 }}>
              <div style={{ fontSize:40, marginBottom:8 }}>🎉</div>
              <div style={{ fontWeight:900, fontSize:20, color:P.ink }}>Your bid was accepted!</div>
              <div style={{ fontSize:13, color:P.body, marginTop:6, lineHeight:1.5 }}>
                A small lead fee applies for free-tier providers when a customer accepts your bid.
              </div>
            </div>

            {/* job summary */}
            <div style={{ background:P.bg, border:`1px solid ${P.border}`, borderRadius:14, padding:"14px 16px", marginBottom:18 }}>
              <div style={{ fontWeight:700, fontSize:13, color:P.ink, marginBottom:8 }}>{leadFeeJob.job}</div>
              <div style={{ display:"flex", justifyContent:"space-between" }}>
                <div>
                  <div style={{ fontSize:10, color:P.muted, fontWeight:700 }}>YOUR BID</div>
                  <div style={{ fontWeight:900, fontSize:18, color:P.ink }}>${leadFeeJob.amount}</div>
                </div>
                <div>
                  <div style={{ fontSize:10, color:P.muted, fontWeight:700 }}>LEAD FEE</div>
                  <div style={{ fontWeight:900, fontSize:18, color:P.red }}>−${LEAD_FEE}</div>
                </div>
                <div>
                  <div style={{ fontSize:10, color:P.muted, fontWeight:700 }}>YOU EARN</div>
                  <div style={{ fontWeight:900, fontSize:18, color:P.green }}>${leadFeeJob.amount - LEAD_FEE}</div>
                </div>
              </div>
            </div>

            {/* upgrade nudge */}
            <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:12, padding:"10px 14px", marginBottom:18, display:"flex", gap:10, alignItems:"center" }}>
              <span style={{ fontSize:18 }}>💡</span>
              <div style={{ fontSize:11, color:"#C084FC", lineHeight:1.4 }}>
                <strong>Premium providers pay zero lead fees.</strong> Upgrade for $49.99/mo and keep 100% of your earnings.
              </div>
            </div>

            <button
              onClick={() => { setFeePaid(f=>[...f, leadFeeJob.id]); setLeadFeeJob(null); }}
              style={{ width:"100%", background:P.blue, color:"white", border:"none", padding:"14px", borderRadius:14, fontWeight:800, fontSize:14, cursor:"pointer", marginBottom:10 }}
            >
              Pay ${LEAD_FEE} Lead Fee & Access Job
            </button>
            <button
              onClick={() => { setIsPremiumProv(true); setLeadFeeJob(null); }}
              style={{ width:"100%", background:"transparent", border:"1px solid #4C3A7A55", color:"#C084FC", padding:"13px", borderRadius:14, fontWeight:700, fontSize:13, cursor:"pointer" }}
            >
              Upgrade to Premium — skip all fees
            </button>
          </div>
        </div>
      )}

      {/* ── BOTTOM NAV ── */}
      <nav style={{ position:"fixed", bottom:0, left:"50%", transform:"translateX(-50%)", width:"100%", maxWidth:430, background:P.surface, borderTop:`1px solid ${P.border}`, display:"flex", zIndex:40, paddingBottom:16 }}>
        {provTabs.map(t=>(
          <button key={t.id} onClick={()=>setTab(t.id)} style={{ flex:1, border:"none", background:"none", cursor:"pointer", padding:"10px 0 2px", display:"flex", flexDirection:"column", alignItems:"center", gap:3 }}>
            <span style={{ fontSize:18, color:tab===t.id?P.blue:P.muted }}>{t.icon}</span>
            <span style={{ fontSize:9, fontWeight:800, letterSpacing:0.3, color:tab===t.id?P.blue:P.muted, textTransform:"uppercase" }}>{t.label}</span>
            {tab===t.id && <div style={{ position:"absolute", top:0, width:20, height:2, background:P.blue, borderRadius:1 }} />}
          </button>
        ))}
      </nav>
    </div>
  );
}

/* ── PROVIDER DASHBOARD ──────────────────────────────────── */
function ProvDash({ isPremiumProv, setIsPremiumProv, activeBids, BID_CAP, serviceZIPs, liveZIP }) {
  const maxVal = Math.max(...EARNINGS.map(e=>e.val));
  // leads that fall within this provider's service ZIPs
  const localLeads = JOBS.filter(j => serviceZIPs.includes(j.zip));

  return (
    <div style={{ padding:"16px" }}>

      {/* ── Tier banner ── */}
      {isPremiumProv
        ? <div style={{ background:`linear-gradient(135deg, ${P.goldDark}, #2e1f08)`, border:`1px solid ${P.gold}44`, borderRadius:16, padding:"12px 16px", marginBottom:16, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
            <div>
              <div style={{ fontWeight:800, fontSize:12, color:P.gold }}>★ Premium Provider · Active</div>
              <div style={{ fontSize:11, color:P.body, marginTop:2 }}>Sponsored in ZIPs {serviceZIPs.join(", ")} · Unlimited bids · No lead fees</div>
            </div>
            <button onClick={()=>setIsPremiumProv(false)} style={{ fontSize:9, color:P.muted, background:"none", border:"none", cursor:"pointer" }}>demo ↩</button>
          </div>
        : <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:16, padding:"12px 16px", marginBottom:16 }}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start" }}>
              <div>
                <div style={{ fontWeight:800, fontSize:12, color:"#C084FC" }}>🔒 Free Tier · {activeBids}/{BID_CAP} bids used this month</div>
                <div style={{ fontSize:11, color:P.body, marginTop:2 }}>Upgrade to remove bid cap, lead fees &amp; get sponsored placement</div>
              </div>
              <button onClick={()=>setIsPremiumProv(true)} style={{ background:"#7C3AED", color:"white", border:"none", padding:"7px 12px", borderRadius:10, fontSize:11, fontWeight:800, cursor:"pointer", whiteSpace:"nowrap", marginLeft:10 }}>Upgrade</button>
            </div>
            {/* bid cap progress bar */}
            <div style={{ marginTop:10 }}>
              <div style={{ height:5, background:P.border, borderRadius:3 }}>
                <div style={{ height:5, width:`${(activeBids/BID_CAP)*100}%`, background: activeBids>=BID_CAP ? P.red : "#7C3AED", borderRadius:3, transition:"width 0.4s" }} />
              </div>
              <div style={{ fontSize:9, color: activeBids>=BID_CAP ? P.red : P.muted, marginTop:3, fontWeight:700 }}>
                {activeBids>=BID_CAP ? "⚠ Bid limit reached — upgrade or wait for bids to close" : `${BID_CAP - activeBids} bid slot${BID_CAP-activeBids!==1?"s":""} remaining`}
              </div>
            </div>
          </div>
      }

      {/* ── Location strip ── */}
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:14, padding:"10px 14px", marginBottom:16, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
        <div style={{ display:"flex", alignItems:"center", gap:8 }}>
          <span style={{ fontSize:16 }}>📍</span>
          <div>
            <div style={{ fontSize:11, fontWeight:800, color:P.ink }}>Live Location · ZIP {liveZIP}</div>
            <div style={{ fontSize:10, color:P.body, marginTop:1 }}>Service coverage: {serviceZIPs.join(" · ")}</div>
          </div>
        </div>
        {isPremiumProv && (
          <div style={{ fontSize:9, background:P.goldDark, color:P.gold, fontWeight:800, padding:"3px 8px", borderRadius:10 }}>★ SPONSORED HERE</div>
        )}
      </div>

      {/* ── Greeting ── */}
      <div style={{ marginBottom:16 }}>
        <div style={{ fontSize:12, color:P.muted, fontWeight:700, letterSpacing:1 }}>SUNDAY, MARCH 1</div>
        <div style={{ fontSize:24, fontWeight:900, color:P.ink, marginTop:2 }}>Good morning, Marcus 👋</div>
        <div style={{ fontSize:13, color:P.body, marginTop:2 }}>
          <span style={{ color:P.gold, fontWeight:700 }}>{localLeads.length} new leads</span> in your area ·&nbsp;
          <span style={{ color:P.green, fontWeight:700 }}>2 pending bids</span>
        </div>
      </div>

      {/* ── KPI grid ── */}
      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:10, marginBottom:16 }}>
        {[
          { label:"This Week",   val:"$1,535", sub:"+12% vs last wk",  color:P.green,  bg:P.greenDark, icon:"💰" },
          { label:"Active Jobs", val:"3",      sub:"2 in progress",    color:P.blue,   bg:P.blueLight, icon:"⚙️" },
          { label:"Bids Sent",   val:activeBids, sub:isPremiumProv?"Unlimited plan":`${BID_CAP-activeBids} slots left`, color:P.gold, bg:P.goldDark, icon:"📤" },
          { label:"Avg Rating",  val:"4.9 ★", sub:"from 312 reviews", color:P.orange, bg:"#2A1A0A",   icon:"⭐" },
        ].map(k=>(
          <div key={k.label} style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px 14px" }}>
            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start" }}>
              <div style={{ width:36, height:36, background:k.bg, borderRadius:10, display:"flex", alignItems:"center", justifyContent:"center", fontSize:18 }}>{k.icon}</div>
              <div style={{ fontSize:9, color:k.color, fontWeight:700 }}>▲ UP</div>
            </div>
            <div style={{ marginTop:10, fontWeight:900, fontSize:22, color:k.color, lineHeight:1 }}>{k.val}</div>
            <div style={{ fontSize:10, color:P.muted, marginTop:4, fontWeight:700 }}>{k.label}</div>
            <div style={{ fontSize:10, color:P.body, marginTop:2 }}>{k.sub}</div>
          </div>
        ))}
      </div>

      {/* ── Earnings chart ── */}
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px 14px", marginBottom:16 }}>
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:14 }}>
          <div>
            <div style={{ fontWeight:800, fontSize:14, color:P.ink }}>Earnings This Week</div>
            <div style={{ fontWeight:900, fontSize:22, color:P.green, marginTop:2 }}>$2,535</div>
          </div>
          <div style={{ fontSize:10, background:P.greenDark, color:P.green, padding:"5px 10px", borderRadius:20, fontWeight:700 }}>+18% WoW</div>
        </div>
        <div style={{ display:"flex", alignItems:"flex-end", gap:6, height:70 }}>
          {EARNINGS.map((e,i)=>(
            <div key={e.label} style={{ flex:1, display:"flex", flexDirection:"column", alignItems:"center", gap:4 }}>
              <div style={{ width:"100%", background:i===5?P.blue:`${P.blue}44`, borderRadius:"4px 4px 0 0", height:`${(e.val/maxVal)*60}px`, minHeight:4 }} />
              <span style={{ fontSize:9, color:i===5?P.blue:P.muted, fontWeight:700 }}>{e.label}</span>
            </div>
          ))}
        </div>
      </div>

      {/* ── Priority alerts — jobs in service radius ── */}
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:10 }}>
        <div style={{ fontWeight:800, fontSize:13, color:P.muted, letterSpacing:0.8 }}>
          {isPremiumProv ? "🔔 PRIORITY ALERTS · YOUR COVERAGE AREA" : "📋 LEADS IN YOUR AREA"}
        </div>
        <div style={{ fontSize:10, color:P.body }}>{localLeads.length} jobs</div>
      </div>
      {isPremiumProv && (
        <div style={{ background:P.blueLight, border:`1px solid ${P.blue}33`, borderRadius:12, padding:"8px 12px", marginBottom:10, fontSize:11, color:P.blue }}>
          ⚡ You're notified instantly when jobs post in <strong>{serviceZIPs.join(", ")}</strong>
        </div>
      )}
      {localLeads.slice(0,3).map((j,i)=>(
        <div key={i} style={{ background:P.card, border:`1px solid ${j.urgent?P.red+"44":P.border}`, borderRadius:14, padding:"12px 16px", marginBottom:8, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
          <div style={{ flex:1 }}>
            <div style={{ display:"flex", alignItems:"center", gap:5, marginBottom:3 }}>
              {j.urgent && <span style={{ fontSize:9, background:"#2A0F0F", color:P.red, fontWeight:700, padding:"2px 6px", borderRadius:8 }}>🔥 URGENT</span>}
              <span style={{ fontSize:9, background:P.blueLight, color:P.blue, fontWeight:700, padding:"2px 6px", borderRadius:8 }}>{j.cat}</span>
              <span style={{ fontSize:9, color:P.muted }}>📍 {j.zip}</span>
            </div>
            <div style={{ fontWeight:700, fontSize:13, color:P.ink }}>{j.title}</div>
            <div style={{ fontSize:10, color:P.muted, marginTop:2 }}>{j.age} ago · {j.bids} bids</div>
          </div>
          <div style={{ textAlign:"right", marginLeft:10 }}>
            <div style={{ fontWeight:900, fontSize:14, color:P.green }}>{j.budget}</div>
          </div>
        </div>
      ))}

      {/* ── Today's schedule ── */}
      <div style={{ fontWeight:800, fontSize:13, color:P.muted, letterSpacing:0.8, margin:"18px 0 10px" }}>TODAY'S SCHEDULE</div>
      {[
        { time:"2:00 PM", job:"Window seal replacement", customer:"R. Thompson", status:"confirmed", color:P.green },
        { time:"5:30 PM", job:"Fence panel estimate",    customer:"T. Gordon",   status:"pending",   color:P.gold },
      ].map((s,i)=>(
        <div key={i} style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:14, padding:"12px 16px", marginBottom:8, display:"flex", gap:12, alignItems:"center" }}>
          <div style={{ minWidth:52, textAlign:"center" }}>
            <div style={{ fontSize:12, fontWeight:900, color:s.color }}>{s.time}</div>
          </div>
          <div style={{ width:2, height:34, background:s.color, borderRadius:1, flexShrink:0 }} />
          <div style={{ flex:1 }}>
            <div style={{ fontWeight:700, fontSize:13, color:P.ink }}>{s.job}</div>
            <div style={{ fontSize:11, color:P.muted, marginTop:1 }}>{s.customer}</div>
          </div>
          <div style={{ fontSize:9, background:`${s.color}22`, color:s.color, fontWeight:700, padding:"4px 8px", borderRadius:10, textTransform:"uppercase" }}>{s.status}</div>
        </div>
      ))}
    </div>
  );
}

/* ── PROVIDER LEADS ──────────────────────────────────────── */
function ProvLeads({ isPremiumProv, setIsPremiumProv, activeBids, setActiveBids, BID_CAP, serviceZIPs, liveZIP, selJob, setSelJob, bidAmt, setBidAmt, bidMsg, setBidMsg, bidSent, setBidSent }) {

  // Only show jobs in the provider's service ZIPs
  const visibleJobs = JOBS.filter(j => serviceZIPs.includes(j.zip));
  const atCap = !isPremiumProv && activeBids >= BID_CAP;

  if (selJob) return (
    <div style={{ padding:"16px" }}>
      <button onClick={()=>{setSelJob(null);setBidSent(false);setBidAmt("");}} style={{ background:"none", border:"none", color:P.blue, fontSize:13, fontWeight:700, cursor:"pointer", padding:"0 0 14px" }}>← Lead Board</button>
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:20, padding:18 }}>
        <div style={{ display:"flex", gap:6, flexWrap:"wrap", marginBottom:10 }}>
          <span style={{ fontSize:10, background:P.blueLight, color:P.blue, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>{selJob.cat}</span>
          <span style={{ fontSize:10, background:P.goldDark, color:P.gold, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>{selJob.budget}</span>
          {selJob.urgent && <span style={{ fontSize:10, background:"#2A0F0F", color:P.red, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>🔥 URGENT</span>}
          <span style={{ fontSize:10, background:P.blueLight, color:P.blue, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>📍 ZIP {selJob.zip}</span>
        </div>
        <div style={{ fontWeight:900, fontSize:18, color:P.ink, marginBottom:8, lineHeight:1.3 }}>{selJob.title}</div>
        <div style={{ background:P.bg, borderRadius:12, padding:"12px 14px", fontSize:13, color:P.body, lineHeight:1.6, marginBottom:16, border:`1px solid ${P.border}` }}>{selJob.desc}</div>

        <div style={{ display:"flex", gap:10, marginBottom:16 }}>
          {[["📍",`ZIP ${selJob.zip}`],["⏱",selJob.age+" ago"],["👥",selJob.bids+" bids"]].map(([ic,v])=>(
            <div key={v} style={{ flex:1, background:P.bg, borderRadius:10, padding:"8px", textAlign:"center", border:`1px solid ${P.border}` }}>
              <div style={{ fontSize:14 }}>{ic}</div>
              <div style={{ fontSize:10, color:P.body, marginTop:3, fontWeight:600 }}>{v}</div>
            </div>
          ))}
        </div>

        {/* ── BID CAP WALL for regular providers ── */}
        {atCap && !bidSent ? (
          <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:16, padding:"20px", textAlign:"center" }}>
            <div style={{ fontSize:32, marginBottom:8 }}>🔒</div>
            <div style={{ fontWeight:900, fontSize:16, color:"#C084FC", marginBottom:6 }}>Bid Limit Reached</div>
            <div style={{ fontSize:13, color:P.body, marginBottom:16, lineHeight:1.5 }}>
              Free providers can have <strong style={{ color:"#C084FC" }}>{BID_CAP} active bids</strong> at a time. Wait for existing bids to close, or upgrade for unlimited bidding.
            </div>
            <div style={{ display:"flex", gap:10 }}>
              <button onClick={()=>setSelJob(null)} style={{ flex:1, background:P.card, border:`1px solid ${P.border}`, color:P.body, padding:"12px", borderRadius:12, fontWeight:700, fontSize:13, cursor:"pointer" }}>← Go Back</button>
              <button onClick={()=>setIsPremiumProv(true)} style={{ flex:2, background:"#7C3AED", color:"white", border:"none", padding:"12px", borderRadius:12, fontWeight:800, fontSize:13, cursor:"pointer" }}>⚡ Upgrade — Unlimited Bids</button>
            </div>
          </div>
        ) : bidSent ? (
          <div style={{ background:P.greenDark, border:`1px solid ${P.green}44`, borderRadius:16, padding:"20px", textAlign:"center" }}>
            <div style={{ fontSize:32, marginBottom:8 }}>✅</div>
            <div style={{ fontWeight:900, fontSize:16, color:P.green }}>Bid Sent!</div>
            <div style={{ fontSize:12, color:P.body, marginTop:4 }}>You'll be notified when the customer responds</div>
            <div style={{ fontWeight:900, fontSize:20, color:P.green, marginTop:8 }}>${bidAmt}</div>
            {!isPremiumProv && (
              <div style={{ fontSize:11, color:P.muted, marginTop:8, background:P.bg, borderRadius:10, padding:"8px 12px", border:`1px solid ${P.border}` }}>
                💡 If accepted, a <strong style={{ color:P.gold }}>$7 lead fee</strong> applies before accessing the job chat.
              </div>
            )}
          </div>
        ) : (
          <>
            {/* ── Smart bid tool ── */}
            <div style={{ background:P.bg, border:`1px solid ${P.border}`, borderRadius:16, padding:"14px 16px", marginBottom:14 }}>
              <div style={{ fontWeight:800, fontSize:11, color:P.muted, letterSpacing:0.8, marginBottom:10 }}>SMART BID TOOL</div>
              <div style={{ display:"flex", gap:12, alignItems:"center", marginBottom:10 }}>
                <div style={{ flex:1 }}>
                  <div style={{ fontSize:10, color:P.muted, fontWeight:700, marginBottom:5 }}>YOUR BID</div>
                  <div style={{ display:"flex", alignItems:"center", background:P.card, border:`1.5px solid ${P.blue}`, borderRadius:10, overflow:"hidden" }}>
                    <span style={{ padding:"0 10px", color:P.blue, fontWeight:800, fontSize:16 }}>$</span>
                    <input value={bidAmt} onChange={e=>setBidAmt(e.target.value)} placeholder="0" style={{ flex:1, background:"transparent", border:"none", padding:"11px 8px", fontSize:18, fontWeight:900, color:P.ink, outline:"none", fontFamily:"inherit" }} />
                  </div>
                </div>
                <div style={{ flex:1 }}>
                  <div style={{ fontSize:10, color:P.muted, fontWeight:700, marginBottom:5 }}>YOU TAKE HOME</div>
                  <div style={{ background:P.greenDark, border:`1px solid ${P.green}33`, borderRadius:10, padding:"11px 10px" }}>
                    <div style={{ fontWeight:900, fontSize:18, color:P.green }}>
                      {bidAmt ? `$${isPremiumProv ? Math.round(bidAmt*0.87) : Math.round(bidAmt*0.87) - 7}` : "—"}
                    </div>
                    <div style={{ fontSize:9, color:P.body, marginTop:1 }}>{isPremiumProv ? "After 13% commission" : "After 13% + $7 lead fee"}</div>
                  </div>
                </div>
              </div>
              {bidAmt && (
                <div style={{ background:P.blueLight, borderRadius:10, padding:"8px 10px", fontSize:11, color:P.blue }}>
                  💡 Market avg for {selJob.cat}: <strong>$145</strong> · Your bid is {Number(bidAmt) > 145 ? "above" : "below"} market
                </div>
              )}
              {!isPremiumProv && bidAmt && (
                <div style={{ background:"#1a1a2e", borderRadius:10, padding:"8px 10px", fontSize:11, color:"#C084FC", marginTop:8 }}>
                  🔒 Free tier: $7 lead fee charged on acceptance · <span style={{ textDecoration:"underline", cursor:"pointer" }} onClick={()=>setIsPremiumProv(true)}>Upgrade to waive</span>
                </div>
              )}
            </div>
            <div style={{ marginBottom:14 }}>
              <div style={{ fontSize:10, color:P.muted, fontWeight:700, marginBottom:5 }}>PITCH MESSAGE</div>
              <textarea value={bidMsg} onChange={e=>setBidMsg(e.target.value)} placeholder="Introduce yourself and why you're the best fit…" rows={3} style={{ width:"100%", background:P.bg, border:`1.5px solid ${P.border}`, borderRadius:12, padding:"11px 14px", fontSize:13, color:P.ink, outline:"none", fontFamily:"inherit", resize:"none", boxSizing:"border-box" }} />
            </div>
            <button
              onClick={()=>{ if(bidAmt){ setActiveBids(a=>a+1); setBidSent(true); }}}
              disabled={!bidAmt}
              style={{ width:"100%", background:bidAmt?P.blue:P.border, color:bidAmt?"white":P.muted, border:"none", padding:14, borderRadius:14, fontWeight:800, fontSize:14, cursor:bidAmt?"pointer":"not-allowed" }}
            >
              {bidAmt ? `Submit Bid — $${bidAmt}` : "Enter bid amount to continue"}
            </button>
          </>
        )}
      </div>
    </div>
  );

  return (
    <div style={{ padding:"16px" }}>
      <div style={{ fontWeight:900, fontSize:22, color:P.ink, marginBottom:2 }}>Lead Board</div>
      <div style={{ fontSize:13, color:P.muted, marginBottom:14 }}>
        Jobs in your coverage: {serviceZIPs.join(", ")}
      </div>

      {/* bid cap bar */}
      {!isPremiumProv && (
        <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:14, padding:"12px 14px", marginBottom:14 }}>
          <div style={{ display:"flex", justifyContent:"space-between", marginBottom:6 }}>
            <span style={{ fontSize:11, fontWeight:800, color:"#C084FC" }}>Bid slots used this month</span>
            <span style={{ fontSize:11, fontWeight:800, color: atCap ? P.red : "#C084FC" }}>{activeBids} / {BID_CAP}</span>
          </div>
          <div style={{ height:6, background:P.border, borderRadius:3 }}>
            <div style={{ height:6, width:`${Math.min((activeBids/BID_CAP)*100,100)}%`, background: atCap ? P.red : "#7C3AED", borderRadius:3 }} />
          </div>
          {atCap
            ? <div style={{ fontSize:10, color:P.red, marginTop:5, fontWeight:700 }}>⚠ Limit reached — <span style={{ textDecoration:"underline", cursor:"pointer" }} onClick={()=>setIsPremiumProv(true)}>upgrade for unlimited bids</span></div>
            : <div style={{ fontSize:10, color:P.muted, marginTop:5 }}>{BID_CAP - activeBids} bid slot{BID_CAP-activeBids!==1?"s":""} remaining · Resets monthly</div>
          }
        </div>
      )}

      {/* filter chips */}
      <div style={{ display:"flex", gap:8, overflowX:"auto", paddingBottom:4, marginBottom:14 }}>
        {["All","Urgent","Plumbing","Electrical","Carpentry","Painting"].map((f,i)=>(
          <button key={f} style={{ background:i===0?P.blue:P.card, color:i===0?"white":P.body, border:`1px solid ${i===0?P.blue:P.border}`, padding:"7px 13px", borderRadius:20, fontSize:11, fontWeight:700, cursor:"pointer", whiteSpace:"nowrap" }}>{f}</button>
        ))}
      </div>

      <div style={{ background:P.goldDark, border:`1px solid ${P.gold}44`, borderRadius:14, padding:"10px 14px", marginBottom:14, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
        <div style={{ fontSize:12, color:P.gold, fontWeight:700 }}>🔥 {visibleJobs.length} open leads in your coverage area</div>
        <div style={{ fontSize:10, color:P.body }}>Updated just now</div>
      </div>

      {visibleJobs.map(j=>(
        <div key={j.id} onClick={()=>setSelJob(j)} style={{ background:P.card, border:`1px solid ${j.urgent?P.red+"44":P.border}`, borderRadius:18, padding:"16px", marginBottom:10, cursor:"pointer", position:"relative", overflow:"hidden", opacity: atCap ? 0.6 : 1 }}>
          {j.urgent && <div style={{ position:"absolute", top:0, left:0, right:0, height:2, background:P.red }} />}
          {atCap && <div style={{ position:"absolute", top:10, right:12, fontSize:9, background:"#1a1a2e", color:"#C084FC", fontWeight:700, padding:"3px 7px", borderRadius:8 }}>🔒 UPGRADE TO BID</div>}
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"flex-start", marginBottom:10 }}>
            <div style={{ flex:1 }}>
              <div style={{ display:"flex", gap:6, flexWrap:"wrap", marginBottom:6 }}>
                <span style={{ fontSize:9, background:P.blueLight, color:P.blue, fontWeight:700, padding:"2px 7px", borderRadius:10 }}>{j.cat}</span>
                {j.urgent && <span style={{ fontSize:9, background:"#2A0F0F", color:P.red, fontWeight:700, padding:"2px 7px", borderRadius:10 }}>🔥 URGENT</span>}
              </div>
              <div style={{ fontWeight:800, fontSize:14, color:P.ink, lineHeight:1.3 }}>{j.title}</div>
              <div style={{ fontSize:11, color:P.muted, marginTop:4 }}>📍 ZIP {j.zip} · {j.age} ago</div>
            </div>
            <div style={{ textAlign:"right", marginLeft:10, flexShrink:0 }}>
              <div style={{ fontWeight:900, fontSize:16, color:P.green }}>{j.budget}</div>
              <div style={{ fontSize:10, color:P.gold, fontWeight:700, marginTop:2 }}>{j.bids} bids</div>
            </div>
          </div>
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center" }}>
            <div style={{ fontSize:10, color:P.body }}>
              Est. take-home: <span style={{ color:P.green, fontWeight:800 }}>
                ${Math.round(parseInt(j.budget)*0.87*0.8) - (isPremiumProv ? 0 : 7)}
              </span>
              {!isPremiumProv && <span style={{ color:P.muted }}> (incl. $7 fee)</span>}
            </div>
            <div style={{ background: atCap?"#1a1a2e":P.blue, color: atCap?"#C084FC":"white", fontSize:11, fontWeight:800, padding:"6px 14px", borderRadius:10 }}>
              {atCap ? "🔒 Locked" : "Submit Bid →"}
            </div>
          </div>
        </div>
      ))}
    </div>
  );
}

/* ── PROVIDER MY BIDS ────────────────────────────────────── */
function ProvBids({ isPremiumProv, setIsPremiumProv, feePaid, setLeadFeeJob, LEAD_FEE }) {
  // Demo bids — one "accepted" that triggers fee gate for regular providers
  const bids = [
    { id:"b1", job:"Leaking Pipe Under Kitchen Sink", amount:155, status:"pending",  ago:"2h",  profit:132, zip:"10001" },
    { id:"b2", job:"Fence Panel Replacement",         amount:380, status:"accepted", ago:"1d",  profit:323, zip:"10001" },
    { id:"b3", job:"HVAC Filter & Tune-Up",           amount:195, status:"pending",  ago:"4h",  profit:166, zip:"10002" },
  ];

  const handleAccessJob = (b) => {
    if (isPremiumProv || feePaid.includes(b.id)) {
      alert("Opening job chat…"); // would navigate to chat
    } else {
      setLeadFeeJob({ ...b, id: b.id });
    }
  };

  return (
    <div style={{ padding:"16px" }}>
      <div style={{ fontWeight:900, fontSize:22, color:P.ink, marginBottom:2 }}>My Bids</div>
      <div style={{ fontSize:13, color:P.muted, marginBottom:16 }}>Track all submitted quotes</div>

      {/* summary strip */}
      <div style={{ display:"flex", gap:8, marginBottom:18 }}>
        {[["3","Sent",P.blue,P.blueLight],["1","Accepted",P.green,P.greenDark],["0","Rejected",P.red,"#2A0F0F"],["2","Pending",P.gold,P.goldDark]].map(([v,l,c,bg])=>(
          <div key={l} style={{ flex:1, background:bg, border:`1px solid ${c}33`, borderRadius:14, padding:"10px 6px", textAlign:"center" }}>
            <div style={{ fontWeight:900, fontSize:18, color:c }}>{v}</div>
            <div style={{ fontSize:9, color:c, fontWeight:700, marginTop:2, opacity:0.8 }}>{l}</div>
          </div>
        ))}
      </div>

      {bids.map((b)=>{
        const isAccepted = b.status === "accepted";
        const feeAlreadyPaid = feePaid.includes(b.id);
        const needsFee = isAccepted && !isPremiumProv && !feeAlreadyPaid;

        return (
          <div key={b.id} style={{ background:P.card, border:`1.5px solid ${isAccepted ? (needsFee ? P.gold+"66" : P.green+"55") : P.border}`, borderRadius:18, padding:"16px", marginBottom:12 }}>

            {/* accepted header */}
            {isAccepted && (
              <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:10, paddingBottom:10, borderBottom:`1px solid ${P.border}` }}>
                <div style={{ fontWeight:800, fontSize:11, color: needsFee ? P.gold : P.green }}>
                  {needsFee ? "🎉 ACCEPTED — Lead fee required to access" : "✅ ACCEPTED — Job Active"}
                </div>
                <span style={{ fontSize:9, background: needsFee ? P.goldDark : P.greenDark, color: needsFee ? P.gold : P.green, fontWeight:700, padding:"3px 7px", borderRadius:8 }}>
                  📍 ZIP {b.zip}
                </span>
              </div>
            )}

            <div style={{ fontWeight:800, fontSize:14, color:P.ink, marginBottom:10, lineHeight:1.3 }}>{b.job}</div>

            <div style={{ display:"flex", gap:10, marginBottom:12 }}>
              <div style={{ flex:1, background:P.bg, borderRadius:10, padding:"10px", border:`1px solid ${P.border}` }}>
                <div style={{ fontSize:9, color:P.muted, fontWeight:700, marginBottom:2 }}>YOUR BID</div>
                <div style={{ fontWeight:900, fontSize:17, color:P.ink }}>${b.amount}</div>
              </div>
              <div style={{ flex:1, background:P.greenDark, borderRadius:10, padding:"10px", border:`1px solid ${P.green}33` }}>
                <div style={{ fontSize:9, color:P.muted, fontWeight:700, marginBottom:2 }}>YOU EARN</div>
                <div style={{ fontWeight:900, fontSize:17, color:P.green }}>
                  ${isPremiumProv ? b.profit : b.profit - LEAD_FEE}
                </div>
              </div>
              <div style={{ flex:1, background:P.bg, borderRadius:10, padding:"10px", border:`1px solid ${P.border}` }}>
                <div style={{ fontSize:9, color:P.muted, fontWeight:700, marginBottom:2 }}>SENT</div>
                <div style={{ fontWeight:700, fontSize:12, color:P.body }}>{b.ago} ago</div>
              </div>
            </div>

            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center" }}>
              <div style={{
                fontSize:11,
                background: isAccepted ? (needsFee ? P.goldDark : P.greenDark) : b.status==="pending" ? P.goldDark : "#2A0F0F",
                color: isAccepted ? (needsFee ? P.gold : P.green) : b.status==="pending" ? P.gold : P.red,
                fontWeight:700, padding:"4px 10px", borderRadius:10, textTransform:"uppercase", letterSpacing:0.5
              }}>
                {isAccepted ? (needsFee ? `⏳ Pay $${LEAD_FEE} to Access` : "✓ Accepted") : b.status==="pending" ? "⏳ Awaiting" : "✗ Declined"}
              </div>

              {isAccepted && (
                <button
                  onClick={() => handleAccessJob(b)}
                  style={{ background: needsFee ? P.gold : P.blue, color: needsFee ? "#0F1923" : "white", border:"none", padding:"8px 14px", borderRadius:10, fontWeight:800, fontSize:12, cursor:"pointer" }}
                >
                  {needsFee ? `💳 Pay $${LEAD_FEE} & Access` : "💬 Open Job Chat"}
                </button>
              )}
            </div>

            {/* fee explanation for free providers */}
            {needsFee && (
              <div style={{ marginTop:10, paddingTop:10, borderTop:`1px solid ${P.border}`, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
                <div style={{ fontSize:11, color:P.body, flex:1 }}>
                  💡 Premium providers skip this fee entirely.
                </div>
                <button onClick={()=>setIsPremiumProv(true)} style={{ fontSize:10, color:"#C084FC", background:"none", border:"none", cursor:"pointer", fontWeight:700, whiteSpace:"nowrap" }}>Upgrade →</button>
              </div>
            )}
          </div>
        );
      })}
    </div>
  );
}

/* ── PROVIDER EARNINGS + ANALYTICS ──────────────────────── */
function ProvEarn({ isPremiumProv, setIsPremiumProv }) {
  const maxVal = Math.max(...EARNINGS.map(e=>e.val));
  const [activeTab, setActiveTab] = useState("earnings");

  return (
    <div style={{ padding:"16px" }}>
      <div style={{ fontWeight:900, fontSize:22, color:P.ink, marginBottom:2 }}>Earnings & Analytics</div>
      <div style={{ fontSize:13, color:P.muted, marginBottom:16 }}>
        {isPremiumProv ? "Full analytics dashboard" : "Basic earnings · Upgrade for full analytics"}
      </div>

      {/* tab switcher */}
      <div style={{ display:"flex", background:P.card, borderRadius:12, padding:3, marginBottom:16, border:`1px solid ${P.border}` }}>
        {["earnings","analytics"].map(t=>(
          <button key={t} onClick={()=>setActiveTab(t)} style={{ flex:1, background:activeTab===t?P.blue:"transparent", color:activeTab===t?"white":P.muted, border:"none", padding:"9px", borderRadius:10, fontWeight:700, fontSize:12, cursor:"pointer", textTransform:"capitalize", display:"flex", alignItems:"center", justifyContent:"center", gap:5 }}>
            {t==="analytics" && !isPremiumProv && <span style={{ fontSize:10 }}>🔒</span>}
            {t.charAt(0).toUpperCase()+t.slice(1)}
          </button>
        ))}
      </div>

      {activeTab === "earnings" && (
        <>
          {/* big number */}
          <div style={{ background:`linear-gradient(135deg, ${P.blueLight}, #0d2035)`, border:`1px solid ${P.blue}33`, borderRadius:22, padding:"22px 20px", marginBottom:16, position:"relative", overflow:"hidden" }}>
            <div style={{ position:"absolute", right:-30, top:-30, width:120, height:120, background:"rgba(45,140,255,0.06)", borderRadius:"50%" }} />
            <div style={{ fontSize:11, color:P.blue, fontWeight:700, letterSpacing:1, marginBottom:6 }}>TOTAL EARNINGS · THIS WEEK</div>
            <div style={{ fontSize:44, fontWeight:900, color:P.ink, lineHeight:1 }}>$2,535</div>
            <div style={{ fontSize:13, color:P.green, marginTop:6, fontWeight:700 }}>▲ +$272 (+12%) vs last week</div>
            <div style={{ display:"flex", gap:16, marginTop:16 }}>
              {[["$680","PENDING",P.gold],["$1,855","PAID OUT",P.green],["$330","FEES",P.body]].map(([v,l,c])=>(
                <div key={l}>
                  <div style={{ fontSize:9, color:P.muted, fontWeight:700, marginBottom:2 }}>{l}</div>
                  <div style={{ fontWeight:800, fontSize:15, color:c }}>{v}</div>
                </div>
              ))}
            </div>
          </div>

          {/* bar chart */}
          <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px 14px", marginBottom:16 }}>
            <div style={{ fontWeight:800, fontSize:13, color:P.ink, marginBottom:14 }}>Daily Earnings</div>
            <div style={{ display:"flex", alignItems:"flex-end", gap:8, height:80 }}>
              {EARNINGS.map((e,i)=>(
                <div key={e.label} style={{ flex:1, display:"flex", flexDirection:"column", alignItems:"center", gap:5 }}>
                  <div style={{ fontSize:9, color:P.body, fontWeight:700 }}>${e.val}</div>
                  <div style={{ width:"100%", background:i===5?P.blue:`${P.blue}44`, borderRadius:"4px 4px 0 0", height:`${(e.val/maxVal)*58}px`, minHeight:4 }} />
                  <span style={{ fontSize:9, color:i===5?P.blue:P.muted, fontWeight:700 }}>{e.label}</span>
                </div>
              ))}
            </div>
          </div>

          {/* transactions */}
          <div style={{ fontWeight:800, fontSize:12, color:P.muted, letterSpacing:0.8, marginBottom:10 }}>RECENT TRANSACTIONS</div>
          {[
            { job:"Fence panel replacement", amount:330, fee:isPremiumProv?0:47, date:"Feb 28", status:"paid" },
            { job:"Window seal job",         amount:195, fee:isPremiumProv?0:25, date:"Feb 27", status:"paid" },
            { job:"Leaking pipe estimate",   amount:155, fee:0,                  date:"Mar 1",  status:"pending" },
          ].map((t,i)=>(
            <div key={i} style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:14, padding:"12px 16px", marginBottom:8, display:"flex", justifyContent:"space-between", alignItems:"center" }}>
              <div>
                <div style={{ fontWeight:700, fontSize:13, color:P.ink }}>{t.job}</div>
                <div style={{ fontSize:11, color:P.muted, marginTop:2 }}>
                  {t.date}
                  {t.fee > 0 && <span style={{ color:P.red }}> · Lead fee: ${t.fee}</span>}
                  {t.fee === 0 && isPremiumProv && t.status==="paid" && <span style={{ color:P.green }}> · No fee (Premium)</span>}
                </div>
              </div>
              <div style={{ textAlign:"right" }}>
                <div style={{ fontWeight:900, fontSize:15, color:t.status==="paid"?P.green:P.gold }}>${t.amount - t.fee}</div>
                <div style={{ fontSize:9, fontWeight:700, color:t.status==="paid"?P.green:P.gold, marginTop:2 }}>{t.status.toUpperCase()}</div>
              </div>
            </div>
          ))}
        </>
      )}

      {activeTab === "analytics" && !isPremiumProv && (
        <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:20, padding:"32px 24px", textAlign:"center" }}>
          <div style={{ fontSize:40, marginBottom:12 }}>📊</div>
          <div style={{ fontWeight:900, fontSize:18, color:"#C084FC", marginBottom:8 }}>Analytics — Premium Feature</div>
          <div style={{ fontSize:13, color:P.body, lineHeight:1.6, marginBottom:24 }}>
            Unlock deep insights: profile view trends, bid win rate, earnings forecasts, top-performing ZIP codes, and competitor pricing benchmarks.
          </div>
          <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:8, marginBottom:20, textAlign:"left" }}>
            {[["📈","Bid win rate by category"],["👁","Profile views & impressions"],["🗺️","Top performing ZIP codes"],["💵","Avg earnings per job type"],["⏱","Response time vs competitors"],["🔮","Weekly earnings forecast"]].map(([ic,txt])=>(
              <div key={txt} style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:10, padding:"10px 10px", display:"flex", gap:6, alignItems:"flex-start", opacity:0.7 }}>
                <span style={{ fontSize:14 }}>{ic}</span>
                <span style={{ fontSize:10, color:P.body, lineHeight:1.3 }}>{txt}</span>
              </div>
            ))}
          </div>
          <button onClick={()=>setIsPremiumProv(true)} style={{ width:"100%", background:"#7C3AED", color:"white", border:"none", padding:"14px", borderRadius:14, fontWeight:800, fontSize:14, cursor:"pointer" }}>
            ⚡ Upgrade to Premium — $49.99/mo
          </button>
        </div>
      )}

      {activeTab === "analytics" && isPremiumProv && (
        <div>
          {/* bid win rate */}
          <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px", marginBottom:12 }}>
            <div style={{ fontWeight:800, fontSize:13, color:P.ink, marginBottom:12 }}>📈 Bid Win Rate by Category</div>
            {[["Plumbing",72,P.green],["Electrical",58,P.blue],["Carpentry",64,P.gold],["HVAC",45,P.orange]].map(([cat,rate,c])=>(
              <div key={cat} style={{ marginBottom:10 }}>
                <div style={{ display:"flex", justifyContent:"space-between", marginBottom:3 }}>
                  <span style={{ fontSize:12, color:P.body, fontWeight:600 }}>{cat}</span>
                  <span style={{ fontSize:12, color:c, fontWeight:800 }}>{rate}%</span>
                </div>
                <div style={{ height:5, background:P.border, borderRadius:3 }}>
                  <div style={{ height:5, width:`${rate}%`, background:c, borderRadius:3 }} />
                </div>
              </div>
            ))}
          </div>

          {/* ZIP performance */}
          <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px", marginBottom:12 }}>
            <div style={{ fontWeight:800, fontSize:13, color:P.ink, marginBottom:12 }}>🗺️ Revenue by ZIP Code</div>
            {[["10001","$1,240","68% of total",P.blue],["10002","$485","26% of total",P.green],["10003","$110","6% of total",P.muted]].map(([zip,rev,pct,c])=>(
              <div key={zip} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"8px 0", borderBottom:`1px solid ${P.border}` }}>
                <div style={{ display:"flex", alignItems:"center", gap:8 }}>
                  <span style={{ fontSize:11, background:`${c}22`, color:c, fontWeight:700, padding:"2px 8px", borderRadius:8 }}>📍{zip}</span>
                </div>
                <div style={{ textAlign:"right" }}>
                  <div style={{ fontWeight:800, fontSize:14, color:P.ink }}>{rev}</div>
                  <div style={{ fontSize:10, color:P.muted }}>{pct}</div>
                </div>
              </div>
            ))}
          </div>

          {/* profile views */}
          <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px" }}>
            <div style={{ fontWeight:800, fontSize:13, color:P.ink, marginBottom:6 }}>👁️ Profile Impressions This Week</div>
            <div style={{ fontSize:36, fontWeight:900, color:P.blue }}>1,247</div>
            <div style={{ fontSize:12, color:P.green, fontWeight:700 }}>▲ +18% from sponsored placement in ZIP 10001</div>
            <div style={{ fontSize:11, color:P.muted, marginTop:4 }}>342 profile visits · 28 quote requests · 8 bookings</div>
          </div>
        </div>
      )}
    </div>
  );
}

/* ── PROVIDER PROFILE ────────────────────────────────────── */
function ProvProfile({ isPremiumProv, setIsPremiumProv, serviceZIPs, setServiceZIPs, liveZIP, setLiveZIP, BID_CAP, activeBids, switchMode }) {
  const [addingZIP, setAddingZIP] = useState(false);
  const [newZIP,    setNewZIP]    = useState("");
  const [useGPS,    setUseGPS]    = useState(true);

  const addZIP = () => {
    const z = newZIP.trim();
    if (z.length === 5 && !serviceZIPs.includes(z)) {
      setServiceZIPs(s=>[...s, z]);
    }
    setNewZIP(""); setAddingZIP(false);
  };
  const removeZIP = (z) => setServiceZIPs(s=>s.filter(x=>x!==z));

  return (
    <div style={{ padding:"16px" }}>

      {/* ── Profile card ── */}
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:22, padding:"20px", marginBottom:14, position:"relative", overflow:"hidden" }}>
        <div style={{ position:"absolute", top:0, right:0, width:100, height:100, background:`${P.blue}08`, borderRadius:"0 22px 0 100px" }} />
        <div style={{ display:"flex", gap:14, alignItems:"flex-start" }}>
          <div style={{ width:64, height:64, background:`linear-gradient(135deg,${P.blue},#1a68cc)`, borderRadius:20, display:"flex", alignItems:"center", justifyContent:"center", fontSize:22, fontWeight:900, color:"white", position:"relative" }}>
            MT
            <div style={{ position:"absolute", bottom:2, right:2, width:14, height:14, background:P.green, borderRadius:"50%", border:"2px solid #1C2D3E" }} />
          </div>
          <div style={{ flex:1 }}>
            <div style={{ fontWeight:900, fontSize:18, color:P.ink }}>Marcus Thompson</div>
            <div style={{ fontSize:12, color:P.body, marginTop:1 }}>ProFix Solutions · Master Plumber</div>
            <div style={{ display:"flex", gap:6, marginTop:6, flexWrap:"wrap" }}>
              {isPremiumProv
                ? <span style={{ fontSize:10, background:P.goldDark, color:P.gold, fontWeight:800, padding:"3px 8px", borderRadius:10 }}>★ PREMIUM PRO</span>
                : <span style={{ fontSize:10, background:"#1a1a2e", color:"#C084FC", fontWeight:800, padding:"3px 8px", borderRadius:10 }}>FREE TIER</span>
              }
              <span style={{ fontSize:10, background:P.blueLight, color:P.blue, fontWeight:700, padding:"3px 8px", borderRadius:10 }}>✓ VERIFIED</span>
            </div>
          </div>
        </div>
        <div style={{ display:"grid", gridTemplateColumns:"repeat(3,1fr)", gap:8, marginTop:16 }}>
          {[["$85/hr","Rate"],["847","Jobs Done"],["4.9★","Rating"]].map(([v,l])=>(
            <div key={l} style={{ background:P.bg, borderRadius:12, padding:"8px", textAlign:"center", border:`1px solid ${P.border}` }}>
              <div style={{ fontWeight:900, fontSize:14, color:P.blue }}>{v}</div>
              <div style={{ fontSize:9, color:P.muted, marginTop:2, fontWeight:700 }}>{l}</div>
            </div>
          ))}
        </div>
      </div>

      {/* ── Tier card — full real feature descriptions ── */}
      {isPremiumProv ? (
        <div style={{ background:P.goldDark, border:`1px solid ${P.gold}44`, borderRadius:18, padding:"16px 18px", marginBottom:14 }}>
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:12 }}>
            <div style={{ fontWeight:900, fontSize:14, color:P.gold }}>★ Premium Provider · $49.99/mo</div>
            <button onClick={()=>setIsPremiumProv(false)} style={{ fontSize:9, color:P.muted, background:"none", border:"none", cursor:"pointer" }}>demo ↩</button>
          </div>
          {[
            ["📍","Sponsored placement","Your profile appears first in your covered ZIPs — boosted by live location"],
            ["∞", "Unlimited bids",     "No monthly cap — bid on as many jobs as you want"],
            ["📊","Advanced analytics", "Bid win rates, ZIP revenue, profile impressions, earnings forecast"],
            ["🔔","Priority alerts",     "Instant push when jobs post inside your service radius before anyone else sees them"],
            ["💸","Zero lead fees",      "No charge when a customer accepts your bid — keep 100% of your earnings (minus 13% commission)"],
          ].map(([ic,title,desc])=>(
            <div key={title} style={{ display:"flex", gap:10, alignItems:"flex-start", marginBottom:10 }}>
              <div style={{ width:28, height:28, background:`${P.gold}22`, borderRadius:8, display:"flex", alignItems:"center", justifyContent:"center", fontSize:14, flexShrink:0 }}>{ic}</div>
              <div>
                <div style={{ fontSize:12, fontWeight:800, color:P.gold }}>{title}</div>
                <div style={{ fontSize:11, color:P.body, marginTop:1, lineHeight:1.4 }}>{desc}</div>
              </div>
            </div>
          ))}
        </div>
      ) : (
        <div style={{ background:"#1a1a2e", border:"1px solid #4C3A7A55", borderRadius:18, padding:"16px 18px", marginBottom:14 }}>
          <div style={{ fontWeight:900, fontSize:14, color:"#C084FC", marginBottom:12 }}>Free Tier · Current Limitations</div>
          {[
            ["🔒",`Max ${BID_CAP} active bids/month`, `You've used ${activeBids} of ${BID_CAP} this month`,"#C084FC"],
            ["🔒","Lead fee on acceptance",    "Pay $7 each time a customer accepts your bid","#EF4444"],
            ["🔒","No analytics access",       "Bid win rates and ZIP data locked","#C084FC"],
            ["🔒","Standard listing only",     "No sponsored placement — sorted by rating","#C084FC"],
            ["🔒","Standard job alerts",       "Notified at same time as all providers in area","#C084FC"],
          ].map(([ic,title,desc,c])=>(
            <div key={title} style={{ display:"flex", gap:10, alignItems:"flex-start", marginBottom:9 }}>
              <div style={{ width:28, height:28, background:"#2a1a3e", borderRadius:8, display:"flex", alignItems:"center", justifyContent:"center", fontSize:12, flexShrink:0, color:c }}>{ic}</div>
              <div>
                <div style={{ fontSize:12, fontWeight:800, color:P.body }}>{title}</div>
                <div style={{ fontSize:11, color:P.muted, marginTop:1, lineHeight:1.4 }}>{desc}</div>
              </div>
            </div>
          ))}
          <button onClick={()=>setIsPremiumProv(true)} style={{ width:"100%", background:"#7C3AED", color:"white", border:"none", padding:"13px", borderRadius:12, fontWeight:800, fontSize:14, cursor:"pointer", marginTop:6 }}>
            ⚡ Upgrade to Premium — $49.99/mo
          </button>
        </div>
      )}

      {/* ── Location & Service Coverage ── */}
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:18, padding:"16px 18px", marginBottom:14 }}>
        <div style={{ fontWeight:800, fontSize:13, color:P.ink, marginBottom:14 }}>📍 Location & Service Coverage</div>

        {/* live GPS toggle */}
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", background:P.bg, borderRadius:12, padding:"10px 14px", marginBottom:12, border:`1px solid ${P.border}` }}>
          <div>
            <div style={{ fontSize:12, fontWeight:700, color:P.ink }}>Use Live GPS Location</div>
            <div style={{ fontSize:10, color:P.body, marginTop:1 }}>Currently detected: ZIP <strong style={{ color:P.blue }}>{liveZIP}</strong></div>
          </div>
          <div onClick={()=>setUseGPS(g=>!g)} style={{ width:44, height:24, background:useGPS?P.green:P.border, borderRadius:12, cursor:"pointer", position:"relative", transition:"background 0.2s" }}>
            <div style={{ position:"absolute", top:3, left: useGPS ? 22 : 3, width:18, height:18, background:"white", borderRadius:"50%", transition:"left 0.2s" }} />
          </div>
        </div>

        {/* manual ZIP override */}
        {!useGPS && (
          <div style={{ marginBottom:12 }}>
            <div style={{ fontSize:10, color:P.muted, fontWeight:700, marginBottom:5 }}>MANUAL LOCATION ZIP</div>
            <div style={{ display:"flex", gap:8 }}>
              <input value={liveZIP} onChange={e=>setLiveZIP(e.target.value)} maxLength={5} placeholder="Enter ZIP" style={{ flex:1, background:P.bg, border:`1.5px solid ${P.blue}`, borderRadius:10, padding:"9px 12px", fontSize:13, color:P.ink, outline:"none", fontFamily:"inherit" }} />
              <button style={{ background:P.blue, color:"white", border:"none", padding:"9px 16px", borderRadius:10, fontWeight:700, fontSize:12, cursor:"pointer" }}>Set</button>
            </div>
          </div>
        )}

        {/* service ZIPs */}
        <div style={{ fontSize:10, color:P.muted, fontWeight:700, marginBottom:8 }}>
          SERVICE COVERAGE ZIPS {isPremiumProv && <span style={{ color:P.gold }}>· SPONSORED IN ALL</span>}
        </div>
        <div style={{ display:"flex", gap:8, flexWrap:"wrap", marginBottom:10 }}>
          {serviceZIPs.map(z=>(
            <div key={z} style={{ display:"flex", alignItems:"center", gap:4, background: z===liveZIP ? `${P.blue}22` : P.blueLight, border:`1px solid ${z===liveZIP?P.blue:P.border}`, borderRadius:20, padding:"4px 10px" }}>
              <span style={{ fontSize:11, color:P.blue, fontWeight:700 }}>📍{z}</span>
              {z===liveZIP && <span style={{ fontSize:9, color:P.blue, fontWeight:700 }}>·live</span>}
              <span onClick={()=>removeZIP(z)} style={{ fontSize:11, color:P.muted, cursor:"pointer", marginLeft:2 }}>✕</span>
            </div>
          ))}
          {addingZIP
            ? <div style={{ display:"flex", gap:6 }}>
                <input value={newZIP} onChange={e=>setNewZIP(e.target.value)} onKeyDown={e=>e.key==="Enter"&&addZIP()} maxLength={5} placeholder="ZIP" style={{ width:70, background:P.bg, border:`1.5px solid ${P.blue}`, borderRadius:20, padding:"4px 10px", fontSize:11, color:P.ink, outline:"none", fontFamily:"inherit" }} />
                <button onClick={addZIP} style={{ background:P.blue, color:"white", border:"none", padding:"4px 10px", borderRadius:20, fontSize:11, fontWeight:700, cursor:"pointer" }}>Add</button>
                <button onClick={()=>{setAddingZIP(false);setNewZIP("");}} style={{ background:P.card, color:P.muted, border:`1px solid ${P.border}`, padding:"4px 8px", borderRadius:20, fontSize:11, cursor:"pointer" }}>✕</button>
              </div>
            : <button onClick={()=>setAddingZIP(true)} style={{ fontSize:11, background:P.bg, color:P.muted, fontWeight:700, padding:"4px 12px", borderRadius:20, border:`1px solid ${P.border}`, cursor:"pointer" }}>+ Add ZIP</button>
          }
        </div>
        <div style={{ fontSize:11, color:P.body, lineHeight:1.4 }}>
          {isPremiumProv
            ? `Your profile is sponsored at the top of search results in all ${serviceZIPs.length} ZIP codes above.`
            : "Add ZIPs where you work. Upgrade to Premium for sponsored top placement in all your ZIPs."
          }
        </div>
      </div>

      {/* Settings */}
      <div style={{ fontWeight:800, fontSize:12, color:P.muted, letterSpacing:0.8, marginBottom:10 }}>SETTINGS</div>
      <div style={{ background:P.card, border:`1px solid ${P.border}`, borderRadius:16, overflow:"hidden" }}>
        {[["🔧","Edit Services & Bio",""],["💳","Payout Method","Bank ···4242"],["🔔","Lead Notifications","On"],["📋","Verification Docs",""],["🔄","Switch to Customer","switch"]].map(([ic,lbl,sub,],i,a)=>(
          <div key={lbl} onClick={lbl==="Switch to Customer"?switchMode:undefined} style={{ display:"flex", justifyContent:"space-between", alignItems:"center", padding:"13px 16px", borderBottom:i<a.length-1?`1px solid ${P.border}`:"none", cursor:"pointer" }}>
            <div style={{ display:"flex", alignItems:"center", gap:10 }}>
              <span style={{ fontSize:16 }}>{ic}</span>
              <span style={{ fontSize:14, fontWeight:600, color:lbl.includes("Switch")?P.blue:P.ink }}>{lbl}</span>
            </div>
            <div style={{ display:"flex", alignItems:"center", gap:6 }}>
              {sub && <span style={{ fontSize:11, color:P.muted }}>{sub}</span>}
              <span style={{ color:P.muted, fontSize:18 }}>›</span>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
}.   can you build all features and all frotend and backend and complete a production ready for real world usage website (webapp) like thumstack and taskrabbit and angilist combined i have provided a complete setup in code but i want a website or webapp
