import { useState, useEffect, useRef } from "react";

/* ── PALETTE ─────────────────────────────────────────
   bg:       #0d1117  (deep ink navy)
   surface:  #161c27  (raised card)
   border:   #2a3347  (subtle slate border)
   gold:     #c9a84c  (warm antique gold — accent)
   goldlt:   #e2c97e  (light gold for headings)
   cream:    #f0ead8  (ivory text)
   muted:    #7a8799  (secondary text)
   dim:      #3d4f66  (very muted)
────────────────────────────────────────────────────── */

const C = {
  bg:     "#0d1117",
  surf:   "#161c27",
  surf2:  "#1c2333",
  border: "#2a3347",
  gold:   "#c9a84c",
  goldlt: "#e2c97e",
  cream:  "#f0ead8",
  muted:  "#7a8799",
  dim:    "#3d4f66",
  red:    "#c0705a",
  blue:   "#5a8fc0",
  sage:   "#6a9e82",
};

const TYPING_LINES = [
  "Machine Learning & Deep Learning Enthusiast",
  "Sign Language → Text & Audio Converter · Team Lead",
  "B.Tech AI & Data Science — SMVEC, Pondicherry",
  "Google Developer Groups · Content Creation Lead",
  "Building real-world AI solutions that matter",
];

const EDUCATION = [
  {
    degree: "B.Tech — Artificial Intelligence and Data Science",
    school: "Sri Manakula Vinayagar Engineering College",
    place: "Pondicherry, India",
    result: "CGPA: 8.26",
    icon: "◈",
    color: C.gold,
    year: "Present",
  },
  {
    degree: "Higher Secondary",
    school: "TAS English Higher Secondary School",
    place: "Pondicherry, India",
    result: "80.83%",
    icon: "◇",
    color: C.blue,
    year: "2023",
  },
  {
    degree: "Secondary Education",
    school: "TAS English Higher Secondary School",
    place: "Pondicherry, India",
    result: "All Pass",
    icon: "◇",
    color: C.sage,
    year: "2021",
  },
];

const SKILLS = [
  {
    cat: "Programming Languages",
    icon: "{ }",
    color: C.gold,
    items: ["Python", "SQL", "C", "HTML5", "CSS3", "JavaScript"],
  },
  {
    cat: "Libraries & Frameworks",
    icon: "⊞",
    color: C.blue,
    items: ["TensorFlow", "scikit-learn", "OpenCV", "MediaPipe", "Pandas", "LightGBM", "XGBoost", "CatBoost"],
  },
  {
    cat: "AI & Data Science Techniques",
    icon: "◉",
    color: C.sage,
    items: ["Machine Learning", "Deep Learning", "LLMs", "NLP", "Data Preprocessing", "CNN"],
  },
  {
    cat: "Tools",
    icon: "⊙",
    color: C.red,
    items: ["PowerBI", "MS Excel", "Canva", "Figma"],
  },
];

const PROJECTS = [
  {
    title: "Sign Language to Text and Audio Converter",
    role: "Team Lead",
    icon: "✦",
    color: C.gold,
    desc: "Developed an assistive AI system that recognises sign language gestures and converts them into readable text and audible speech in real time. Bridges the communication gap for the hearing-impaired community using cutting-edge computer vision and deep learning.",
    tech: ["Python", "CNN", "MediaPipe", "OpenCV", "TensorFlow", "Text-to-Speech"],
  },
  {
    title: "Intercity Demand Forecasting Using ML",
    role: "Team Lead",
    icon: "◆",
    color: C.blue,
    desc: "Built an ML-based demand forecasting solution leveraging ensemble learning techniques to optimise intercity transportation services. Applied advanced gradient boosting models to predict passenger demand with high accuracy.",
    tech: ["Python", "Pandas", "LightGBM", "XGBoost", "CatBoost"],
  },
];

const ACHIEVEMENTS = [
  { label: "Top 4 — Technoforge ML Hackathon",  org: "SMVEC", icon: "I",   color: C.gold  },
  { label: "Top 10 — HackMaster 2.0 Hackathon", org: "SMVEC", icon: "II",  color: C.blue  },
  { label: "Finalist — HackMaster 1.0 Hackathon",org: "SMVEC", icon: "III", color: C.sage  },
];

const CERTS = [
  { name: "Introduction to Machine Learning", issuer: "NPTEL",               date: "Apr 2025", color: C.gold },
  { name: "Python for Data Science",          issuer: "NPTEL",               date: "Aug 2025", color: C.blue },
  { name: "NLP for Developers",               issuer: "Infosys Springboard", date: "Jul 2025", color: C.sage },
  { name: "Natural Language Processing",      issuer: "NPTEL",               date: "Apr 2026", color: C.red  },
];

const LEADERSHIP = [
  {
    role: "Content Creation Lead",
    org: "Google Developer Groups",
    status: "Present",
    desc: "Turning ideas into impactful content for the developer community.",
    color: C.gold,
  },
  {
    role: "Team Lead",
    org: "School Badminton Team",
    status: "Completed",
    desc: "Led the school badminton team with strong team spirit and discipline.",
    color: C.sage,
  },
];

const EXTRACURRICULAR = [
  { title: "Badminton Player",          desc: "Actively participated in college-level and local badminton tournaments; strong team spirit and discipline.", color: C.gold },
  { title: "Content Creator",           desc: "Create engaging digital content for social media platforms; experienced in concept planning and audience engagement.", color: C.blue },
  { title: "Videographer & Photographer", desc: "Skilled in capturing and editing photos and videos for events, social media, and creative projects.", color: C.sage },
];

const LANGUAGES = [
  { lang: "English", level: "Fluent",  pct: 90,  color: C.gold },
  { lang: "Tamil",   level: "Native",  pct: 100, color: C.blue },
];

/* ── TypeWriter ── */
function TypeWriter({ lines }) {
  const [display, setDisplay] = useState("");
  const [lineIdx, setLineIdx] = useState(0);
  const [charIdx, setCharIdx] = useState(0);
  const [deleting, setDeleting] = useState(false);
  useEffect(() => {
    const current = lines[lineIdx];
    let t;
    if (!deleting && charIdx < current.length)       t = setTimeout(() => setCharIdx(c => c+1), 42);
    else if (!deleting && charIdx === current.length) t = setTimeout(() => setDeleting(true), 2200);
    else if (deleting && charIdx > 0)                 t = setTimeout(() => setCharIdx(c => c-1), 20);
    else { setDeleting(false); setLineIdx(i => (i+1) % lines.length); }
    setDisplay(current.slice(0, charIdx));
    return () => clearTimeout(t);
  }, [charIdx, deleting, lineIdx, lines]);
  return (
    <span style={{ color: C.goldlt, fontFamily: "'EB Garamond', Georgia, serif", fontSize: "1rem", fontStyle: "italic", letterSpacing: "0.02em" }}>
      {display}<span style={{ animation: "blink 1.1s step-end infinite", color: C.gold }}>|</span>
    </span>
  );
}

/* ── CountUp ── */
function CountUp({ target, suffix }) {
  const [val, setVal] = useState(0);
  const ref = useRef(null);
  useEffect(() => {
    const num = parseFloat(target);
    const obs = new IntersectionObserver(([e]) => {
      if (e.isIntersecting) {
        let step = 0; const steps = 40; const inc = num / steps;
        const timer = setInterval(() => {
          step++;
          const v = Math.min(inc * step, num);
          setVal(Number.isInteger(num) ? Math.round(v) : parseFloat(v.toFixed(2)));
          if (step >= steps) clearInterval(timer);
        }, 35);
        obs.disconnect();
      }
    }, { threshold: 0.3 });
    if (ref.current) obs.observe(ref.current);
    return () => obs.disconnect();
  }, [target]);
  return <span ref={ref}>{val}{suffix || ""}</span>;
}

/* ── Section Header ── */
function Section({ title, children }) {
  return (
    <div style={{ marginBottom: "28px" }}>
      <div style={{ display: "flex", alignItems: "center", gap: "14px", marginBottom: "16px" }}>
        <span style={{ width: "20px", height: "1px", background: C.gold, display: "inline-block", flexShrink: 0 }} />
        <h2 style={{
          margin: 0, fontSize: "0.65rem",
          fontFamily: "'Cormorant Garamond', 'EB Garamond', Georgia, serif",
          textTransform: "uppercase", letterSpacing: "0.28em",
          color: C.gold, fontWeight: "600",
          whiteSpace: "nowrap",
        }}>{title}</h2>
        <div style={{ flex: 1, height: "1px", background: `linear-gradient(to right, ${C.gold}55, transparent)` }} />
      </div>
      {children}
    </div>
  );
}

/* ── Skill Pill ── */
function Pill({ name, color, delay }) {
  const [hov, setHov] = useState(false);
  return (
    <span
      onMouseEnter={() => setHov(true)} onMouseLeave={() => setHov(false)}
      style={{
        display: "inline-block", margin: "3px",
        background: hov ? color : "transparent",
        color: hov ? C.bg : color,
        border: `1px solid ${hov ? color : color + "55"}`,
        borderRadius: "3px", padding: "3px 11px",
        fontSize: "0.72rem",
        fontFamily: "'Cormorant Garamond', Georgia, serif",
        letterSpacing: "0.04em",
        cursor: "default", transition: "all 0.2s",
        animation: "fadeUp 0.45s ease forwards",
        animationDelay: delay || "0s", opacity: 0,
      }}
    >{name}</span>
  );
}

/* ── Language Bar ── */
function LangBar({ lang, level, pct, color }) {
  const [w, setW] = useState(0);
  const ref = useRef(null);
  useEffect(() => {
    const obs = new IntersectionObserver(([e]) => {
      if (e.isIntersecting) { setTimeout(() => setW(pct), 150); obs.disconnect(); }
    }, { threshold: 0.3 });
    if (ref.current) obs.observe(ref.current);
    return () => obs.disconnect();
  }, [pct]);
  return (
    <div ref={ref} style={{ marginBottom: "16px" }}>
      <div style={{ display: "flex", justifyContent: "space-between", marginBottom: "6px" }}>
        <span style={{ fontSize: "0.9rem", color: C.cream, fontFamily: "'EB Garamond', Georgia, serif" }}>{lang}</span>
        <span style={{ fontSize: "0.7rem", color, fontFamily: "'Cormorant Garamond', Georgia, serif", letterSpacing: "0.1em", textTransform: "uppercase" }}>{level}</span>
      </div>
      <div style={{ height: "2px", background: C.border, borderRadius: "1px" }}>
        <div style={{
          height: "100%", width: `${w}%`, borderRadius: "1px",
          background: `linear-gradient(to right, ${color}, ${color}88)`,
          transition: "width 1.3s cubic-bezier(0.4,0,0.2,1)",
        }} />
      </div>
    </div>
  );
}

/* ── MAIN ── */
export default function App() {
  const [tab, setTab] = useState("overview");
  const [expandedProj, setExpandedProj] = useState(null);

  const tabs = [
    { id: "overview",    label: "Overview"      },
    { id: "education",   label: "Education"     },
    { id: "skills",      label: "Skills"        },
    { id: "projects",    label: "Projects"      },
    { id: "certs",       label: "Certificates"  },
    { id: "leadership",  label: "Leadership"    },
    { id: "extras",      label: "Activities"    },
  ];

  const show = (...ids) => tab === "overview" || ids.includes(tab);

  return (
    <>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;0,700;1,300;1,400&family=JetBrains+Mono:wght@300;400&display=swap');
        * { box-sizing: border-box; }
        @keyframes blink   { 50% { opacity:0 } }
        @keyframes fadeUp  { from { opacity:0; transform:translateY(8px) } to { opacity:1; transform:none } }
        @keyframes slideIn { from { opacity:0; transform:translateX(-12px) } to { opacity:1; transform:none } }
        @keyframes shimmer {
          0%   { opacity: 0.6 }
          50%  { opacity: 1   }
          100% { opacity: 0.6 }
        }
        ::-webkit-scrollbar        { width: 4px }
        ::-webkit-scrollbar-track  { background: #0d1117 }
        ::-webkit-scrollbar-thumb  { background: #c9a84c55; border-radius: 2px }
        .card {
          background: #161c27;
          border: 1px solid #2a3347;
          border-radius: 8px;
          transition: all 0.22s;
        }
        .card:hover {
          background: #1c2333;
          border-color: #c9a84c44;
          transform: translateY(-2px);
          box-shadow: 0 8px 28px rgba(0,0,0,0.5), 0 0 0 1px #c9a84c18;
        }
        .tab {
          background: transparent;
          border: 1px solid #2a3347;
          color: #3d4f66;
          padding: 6px 15px;
          border-radius: 3px;
          cursor: pointer;
          font-family: 'Cormorant Garamond', Georgia, serif;
          font-size: 0.78rem;
          letter-spacing: 0.12em;
          text-transform: uppercase;
          transition: all 0.2s;
          white-space: nowrap;
        }
        .tab:hover { color: #c9a84c; border-color: #c9a84c44; background: #c9a84c09; }
        .tab.on    { color: #c9a84c; border-color: #c9a84c; background: #c9a84c12; }
      `}</style>

      <div style={{ background: C.bg, minHeight: "100vh", fontFamily: "'EB Garamond', Georgia, serif", color: C.cream, position: "relative" }}>

        {/* Subtle noise texture overlay */}
        <div style={{
          position: "fixed", inset: 0, zIndex: 0, pointerEvents: "none", opacity: 0.025,
          backgroundImage: "url(\"data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E\")",
          backgroundRepeat: "repeat", backgroundSize: "150px 150px",
        }}/>

        {/* Top radial glow */}
        <div style={{
          position: "fixed", inset: 0, zIndex: 0, pointerEvents: "none",
          backgroundImage: `radial-gradient(ellipse 70% 40% at 50% 0%, ${C.gold}0f 0%, transparent 65%)`,
        }}/>

        <div style={{ maxWidth: "860px", margin: "0 auto", padding: "32px 18px", position: "relative", zIndex: 1 }}>

          {/* ══ HERO ══ */}
          <div style={{
            background: `linear-gradient(160deg, #1a2235 0%, #131929 60%, #0f1520 100%)`,
            border: `1px solid ${C.gold}33`,
            borderRadius: "12px",
            padding: "clamp(24px,5vw,46px)",
            marginBottom: "20px",
            position: "relative",
            overflow: "hidden",
          }}>
            {/* corner ornament */}
            <div style={{ position:"absolute", top:0, right:0, width:"180px", height:"180px",
              backgroundImage:`radial-gradient(circle at top right, ${C.gold}0c, transparent 65%)`,
              pointerEvents:"none"
            }}/>
            {/* bottom ornament */}
            <div style={{ position:"absolute", bottom:"-1px", left:"0", right:"0", height:"1px",
              background:`linear-gradient(to right, transparent, ${C.gold}44, transparent)`,
            }}/>

            <div style={{ display:"flex", gap:"26px", alignItems:"flex-start", flexWrap:"wrap" }}>
              {/* monogram */}
              <div style={{
                width:"80px", height:"80px", borderRadius:"4px", flexShrink:0,
                border:`1px solid ${C.gold}55`,
                background:`linear-gradient(135deg, #1c2840, #111825)`,
                display:"flex", alignItems:"center", justifyContent:"center",
                position:"relative",
              }}>
                <span style={{
                  fontFamily:"'Cormorant Garamond', Georgia, serif",
                  fontSize:"2.4rem", color:C.goldlt, fontWeight:"300", lineHeight:1, userSelect:"none",
                }}>H</span>
                {/* corner ticks */}
                {[[0,0],[0,1],[1,0],[1,1]].map(([y,x],i)=>(
                  <div key={i} style={{
                    position:"absolute",
                    top: y ? "auto":"4px", bottom: y ? "4px":"auto",
                    left: x ? "auto":"4px", right: x ? "4px":"auto",
                    width:"7px", height:"7px",
                    borderTop: !y ? `1px solid ${C.gold}88` : "none",
                    borderBottom: y  ? `1px solid ${C.gold}88` : "none",
                    borderLeft: !x  ? `1px solid ${C.gold}88` : "none",
                    borderRight: x  ? `1px solid ${C.gold}88` : "none",
                  }}/>
                ))}
              </div>

              <div style={{ flex:1, minWidth:"200px" }}>
                <div style={{ display:"flex", flexWrap:"wrap", alignItems:"baseline", gap:"12px", marginBottom:"6px" }}>
                  <h1 style={{
                    margin:0,
                    fontSize:"clamp(1.9rem,5vw,2.7rem)", fontWeight:"500",
                    fontFamily:"'Cormorant Garamond', Georgia, serif",
                    color:C.cream, letterSpacing:"0.04em", lineHeight:1,
                  }}>Hemeshwaran M</h1>
                  <span style={{
                    fontFamily:"'Cormorant Garamond', Georgia, serif",
                    fontSize:"0.78rem", color:C.gold,
                    letterSpacing:"0.22em", textTransform:"uppercase",
                    borderBottom:`1px solid ${C.gold}55`, paddingBottom:"1px",
                  }}>AI · ML · Student</span>
                </div>

                <div style={{ height:"26px", marginBottom:"18px" }}>
                  <TypeWriter lines={TYPING_LINES}/>
                </div>

                {/* contact */}
                <div style={{ display:"flex", flexWrap:"wrap", gap:"8px" }}>
                  {[
                    { t:"hemeshwaranm@gmail.com",    h:"mailto:hemeshwaranm@gmail.com" },
                    { t:"7339422391",                h:"tel:7339422391" },
                    { t:"LinkedIn",                  h:"https://linkedin.com/in/m-hemeshwaran-867408290" },
                    { t:"github.com/Hemeshwaran-M",  h:"https://github.com/Hemeshwaran-M" },
                  ].map(c=>(
                    <a key={c.t} href={c.h} target="_blank" rel="noreferrer" style={{
                      color: C.muted, fontSize:"0.75rem",
                      fontFamily:"'JetBrains Mono', monospace", fontWeight:"300",
                      textDecoration:"none", borderBottom:`1px solid ${C.dim}`,
                      paddingBottom:"1px", transition:"all 0.2s",
                    }}
                      onMouseEnter={e=>{e.currentTarget.style.color=C.gold;e.currentTarget.style.borderColor=C.gold}}
                      onMouseLeave={e=>{e.currentTarget.style.color=C.muted;e.currentTarget.style.borderColor=C.dim}}
                    >{c.t}</a>
                  ))}
                </div>
              </div>
            </div>

            {/* divider */}
            <div style={{ margin:"22px 0 20px", height:"1px", background:`linear-gradient(to right, transparent, ${C.gold}33, ${C.gold}55, ${C.gold}33, transparent)` }}/>

            {/* profile summary */}
            <p style={{
              margin:0, fontSize:"1rem", lineHeight:"1.9",
              fontFamily:"'EB Garamond', Georgia, serif",
              color:C.muted, letterSpacing:"0.01em",
            }}>
              Machine Learning &amp; Deep Learning enthusiast passionate about building real-world AI solutions.
              Led the development of a{" "}
              <em style={{ color:C.cream, fontStyle:"italic" }}>Sign Language to Text &amp; Audio Converter</em>{" "}
              using ML/DL techniques. Experienced in{" "}
              <em style={{ color:C.cream, fontStyle:"italic" }}>project leadership, event management, and mentoring</em>{" "}
              — leveraging creativity and teamwork to drive meaningful impact.
            </p>
          </div>

          {/* ══ STATS ══ */}
          <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(110px,1fr))", gap:"10px", marginBottom:"20px" }}>
            {[
              { v:"8.26", s:"",  l:"CGPA",             c:C.gold },
              { v:"2",    s:"+", l:"Projects",          c:C.blue },
              { v:"4",    s:"",  l:"Certificates",      c:C.sage },
              { v:"3",    s:"",  l:"Hackathon Awards",  c:C.red  },
              { v:"3",    s:"",  l:"Education Levels",  c:C.gold },
              { v:"2",    s:"",  l:"Leadership Roles",  c:C.blue },
            ].map((s,i)=>(
              <div key={i} className="card" style={{ padding:"16px 10px", textAlign:"center", cursor:"default" }}>
                <div style={{
                  fontSize:"1.9rem", fontWeight:"500", lineHeight:1,
                  fontFamily:"'Cormorant Garamond', Georgia, serif",
                  color:s.c, marginBottom:"6px",
                }}>
                  <CountUp target={s.v} suffix={s.s}/>
                </div>
                <div style={{ fontSize:"0.6rem", color:C.dim, textTransform:"uppercase", letterSpacing:"0.14em",
                  fontFamily:"'Cormorant Garamond', Georgia, serif" }}>{s.l}</div>
              </div>
            ))}
          </div>

          {/* ══ TABS ══ */}
          <div style={{
            display:"flex", gap:"6px", flexWrap:"wrap", marginBottom:"24px",
            padding:"10px 14px", background:C.surf, borderRadius:"6px",
            border:`1px solid ${C.border}`,
          }}>
            {tabs.map(n=>(
              <button key={n.id} className={`tab ${tab===n.id?"on":""}`} onClick={()=>setTab(n.id)}>
                {n.label}
              </button>
            ))}
          </div>

          {/* ══ EDUCATION ══ */}
          {show("education") && (
            <Section title="Education">
              <div style={{ display:"flex", flexDirection:"column", gap:"10px" }}>
                {EDUCATION.map((ed,i)=>(
                  <div key={i} className="card" style={{
                    padding:"18px 22px", display:"flex", gap:"18px", alignItems:"flex-start",
                    borderLeft:`2px solid ${ed.color}55`,
                    animation:"slideIn 0.4s ease forwards", animationDelay:`${i*0.1}s`, opacity:0,
                  }}>
                    <span style={{
                      fontFamily:"'Cormorant Garamond',Georgia,serif",
                      fontSize:"1.5rem", color:ed.color, lineHeight:1, flexShrink:0, marginTop:"2px",
                    }}>{ed.icon}</span>
                    <div style={{ flex:1 }}>
                      <div style={{ fontWeight:"500", color:C.cream, fontSize:"0.97rem",
                        fontFamily:"'EB Garamond',Georgia,serif", marginBottom:"4px" }}>{ed.degree}</div>
                      <div style={{ fontSize:"0.82rem", color:C.muted, marginBottom:"10px",
                        fontFamily:"'EB Garamond',Georgia,serif", fontStyle:"italic" }}>
                        {ed.school} · {ed.place}
                      </div>
                      <div style={{ display:"flex", gap:"8px", flexWrap:"wrap" }}>
                        <span style={{
                          background:`${ed.color}14`, color:ed.color,
                          border:`1px solid ${ed.color}33`, borderRadius:"3px",
                          padding:"2px 10px", fontSize:"0.7rem",
                          fontFamily:"'JetBrains Mono',monospace", fontWeight:"300",
                        }}>{ed.result}</span>
                        <span style={{
                          background:"transparent", color:C.dim,
                          border:`1px solid ${C.border}`, borderRadius:"3px",
                          padding:"2px 10px", fontSize:"0.7rem",
                          fontFamily:"'JetBrains Mono',monospace", fontWeight:"300",
                        }}>{ed.year}</span>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ SKILLS ══ */}
          {show("skills") && (
            <Section title="Technical Skills">
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(260px,1fr))", gap:"12px" }}>
                {SKILLS.map((sg,gi)=>(
                  <div key={gi} className="card" style={{
                    padding:"18px 20px",
                    animation:"fadeUp 0.4s ease forwards", animationDelay:`${gi*0.1}s`, opacity:0,
                  }}>
                    <div style={{ display:"flex", alignItems:"center", gap:"10px", marginBottom:"12px" }}>
                      <span style={{ color:sg.color, fontSize:"0.95rem",
                        fontFamily:"'Cormorant Garamond',Georgia,serif", fontWeight:"600" }}>{sg.icon}</span>
                      <span style={{ fontSize:"0.63rem", textTransform:"uppercase", letterSpacing:"0.18em",
                        color:sg.color, fontFamily:"'Cormorant Garamond',Georgia,serif", fontWeight:"600" }}>{sg.cat}</span>
                    </div>
                    <div>
                      {sg.items.map((s,i)=>(
                        <Pill key={s} name={s} color={sg.color} delay={`${(gi*8+i)*0.04}s`}/>
                      ))}
                    </div>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ PROJECTS ══ */}
          {show("projects") && (
            <Section title="Projects">
              <div style={{ display:"flex", flexDirection:"column", gap:"12px" }}>
                {PROJECTS.map((p,i)=>(
                  <div key={i}
                    onClick={()=>setExpandedProj(expandedProj===i?null:i)}
                    style={{
                      background: C.surf, border:`1px solid ${C.border}`,
                      borderRadius:"8px", padding:"20px 22px", cursor:"pointer",
                      transition:"all 0.22s",
                      animation:"fadeUp 0.5s ease forwards", animationDelay:`${i*0.15}s`, opacity:0,
                      borderLeft:`2px solid ${p.color}77`,
                    }}
                    onMouseEnter={e=>{e.currentTarget.style.background=C.surf2;e.currentTarget.style.borderColor=`${p.color}55`;e.currentTarget.style.transform="translateY(-2px)";e.currentTarget.style.boxShadow="0 8px 26px rgba(0,0,0,0.45)"}}
                    onMouseLeave={e=>{e.currentTarget.style.background=C.surf;e.currentTarget.style.borderColor=C.border;e.currentTarget.style.transform="none";e.currentTarget.style.boxShadow="none"}}
                  >
                    <div style={{ display:"flex", alignItems:"flex-start", justifyContent:"space-between", gap:"12px" }}>
                      <div style={{ flex:1 }}>
                        <div style={{ display:"flex", alignItems:"center", gap:"10px", marginBottom:"4px" }}>
                          <span style={{ color:p.color, fontFamily:"'Cormorant Garamond',Georgia,serif", fontSize:"1.1rem" }}>{p.icon}</span>
                          <div style={{ fontWeight:"500", fontSize:"1rem", color:C.cream,
                            fontFamily:"'EB Garamond',Georgia,serif" }}>{p.title}</div>
                        </div>
                        <div style={{ fontSize:"0.72rem", color:C.muted, fontFamily:"'JetBrains Mono',monospace",
                          fontWeight:"300", marginLeft:"22px" }}>Role: {p.role}</div>
                      </div>
                      <span style={{ color:C.dim, fontSize:"0.8rem", transition:"transform 0.3s",
                        transform:expandedProj===i?"rotate(180deg)":"none", flexShrink:0, marginTop:"4px" }}>▾</span>
                    </div>

                    <div style={{ display:"flex", flexWrap:"wrap", gap:"5px", marginTop:"12px", marginLeft:"22px" }}>
                      {p.tech.map(t=>(
                        <span key={t} style={{
                          color:p.color, border:`1px solid ${p.color}44`,
                          background:`${p.color}0c`, borderRadius:"3px",
                          padding:"2px 9px", fontSize:"0.68rem",
                          fontFamily:"'JetBrains Mono',monospace", fontWeight:"300",
                        }}>{t}</span>
                      ))}
                    </div>

                    {expandedProj===i && (
                      <div style={{
                        marginTop:"16px", paddingTop:"16px",
                        borderTop:`1px solid ${C.border}`,
                        animation:"fadeUp 0.3s ease",
                        marginLeft:"22px",
                      }}>
                        <p style={{ margin:0, fontSize:"0.92rem", lineHeight:"1.8",
                          color:C.muted, fontFamily:"'EB Garamond',Georgia,serif", fontStyle:"italic" }}>{p.desc}</p>
                      </div>
                    )}
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ ACHIEVEMENTS ══ */}
          {show("projects","overview") && (
            <Section title="Achievements">
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(220px,1fr))", gap:"10px" }}>
                {ACHIEVEMENTS.map((a,i)=>(
                  <div key={i} className="card" style={{
                    padding:"20px 20px", cursor:"default",
                    animation:"fadeUp 0.4s ease forwards", animationDelay:`${i*0.1}s`, opacity:0,
                  }}>
                    <div style={{
                      fontFamily:"'Cormorant Garamond',Georgia,serif",
                      fontSize:"1.6rem", color:a.color, marginBottom:"10px",
                      fontWeight:"300", letterSpacing:"0.05em",
                    }}>{a.icon}</div>
                    <div style={{ fontWeight:"500", color:C.cream,
                      fontFamily:"'EB Garamond',Georgia,serif",
                      fontSize:"0.88rem", marginBottom:"5px", lineHeight:"1.45" }}>{a.label}</div>
                    <div style={{ fontSize:"0.68rem", color:C.dim,
                      fontFamily:"'Cormorant Garamond',Georgia,serif",
                      letterSpacing:"0.1em", textTransform:"uppercase" }}>{a.org}</div>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ CERTIFICATES ══ */}
          {show("certs") && (
            <Section title="Certificates">
              <div style={{ display:"flex", flexDirection:"column", gap:"9px" }}>
                {CERTS.map((c,i)=>(
                  <div key={i} className="card" style={{
                    padding:"15px 20px", display:"flex", alignItems:"center", gap:"16px",
                    borderLeft:`2px solid ${c.color}55`,
                    animation:"slideIn 0.4s ease forwards", animationDelay:`${i*0.1}s`, opacity:0,
                  }}>
                    <span style={{ width:"6px", height:"6px", borderRadius:"50%",
                      background:c.color, flexShrink:0, boxShadow:`0 0 6px ${c.color}88` }}/>
                    <div style={{ flex:1 }}>
                      <div style={{ fontWeight:"500", color:C.cream, fontSize:"0.9rem",
                        fontFamily:"'EB Garamond',Georgia,serif", marginBottom:"3px" }}>{c.name}</div>
                      <div style={{ fontSize:"0.7rem", color:C.muted, fontStyle:"italic",
                        fontFamily:"'EB Garamond',Georgia,serif" }}>{c.issuer}</div>
                    </div>
                    <span style={{
                      color:c.color, borderBottom:`1px solid ${c.color}55`,
                      fontSize:"0.7rem", fontFamily:"'JetBrains Mono',monospace",
                      fontWeight:"300", whiteSpace:"nowrap", flexShrink:0,
                    }}>{c.date}</span>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ LEADERSHIP ══ */}
          {show("leadership") && (
            <Section title="Leadership">
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(250px,1fr))", gap:"10px" }}>
                {LEADERSHIP.map((l,i)=>(
                  <div key={i} className="card" style={{
                    padding:"20px 22px", cursor:"default",
                    animation:"fadeUp 0.4s ease forwards", animationDelay:`${i*0.1}s`, opacity:0,
                  }}>
                    <div style={{ display:"flex", alignItems:"flex-start", justifyContent:"space-between", marginBottom:"10px" }}>
                      <div>
                        <div style={{ fontWeight:"500", color:l.color,
                          fontFamily:"'EB Garamond',Georgia,serif", fontSize:"0.95rem" }}>{l.role}</div>
                        <div style={{ fontSize:"0.72rem", color:C.muted,
                          fontFamily:"'Cormorant Garamond',Georgia,serif",
                          letterSpacing:"0.08em", textTransform:"uppercase", marginTop:"2px" }}>{l.org}</div>
                      </div>
                      <span style={{
                        color: l.status==="Present" ? C.gold : C.dim,
                        border:`1px solid ${l.status==="Present" ? C.gold+"44" : C.dim+"44"}`,
                        borderRadius:"3px", padding:"2px 8px",
                        fontSize:"0.6rem", fontFamily:"'Cormorant Garamond',Georgia,serif",
                        letterSpacing:"0.14em", textTransform:"uppercase",
                      }}>{l.status}</span>
                    </div>
                    <p style={{ margin:0, fontSize:"0.86rem", color:C.muted, lineHeight:"1.7",
                      fontFamily:"'EB Garamond',Georgia,serif", fontStyle:"italic" }}>{l.desc}</p>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ EXTRACURRICULAR ══ */}
          {show("extras") && (
            <Section title="Extracurricular Activities">
              <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fit,minmax(230px,1fr))", gap:"10px" }}>
                {EXTRACURRICULAR.map((ex,i)=>(
                  <div key={i} className="card" style={{
                    padding:"18px 20px", cursor:"default",
                    borderTop:`2px solid ${ex.color}55`,
                    animation:"fadeUp 0.4s ease forwards", animationDelay:`${i*0.1}s`, opacity:0,
                  }}>
                    <div style={{ fontWeight:"500", color:ex.color,
                      fontFamily:"'EB Garamond',Georgia,serif",
                      fontSize:"0.95rem", marginBottom:"8px" }}>{ex.title}</div>
                    <p style={{ margin:0, fontSize:"0.82rem", color:C.muted, lineHeight:"1.72",
                      fontFamily:"'EB Garamond',Georgia,serif", fontStyle:"italic" }}>{ex.desc}</p>
                  </div>
                ))}
              </div>
            </Section>
          )}

          {/* ══ LANGUAGES ══ */}
          {show("extras") && (
            <Section title="Languages">
              <div style={{
                background:C.surf, border:`1px solid ${C.border}`,
                borderRadius:"8px", padding:"22px 26px", maxWidth:"360px",
              }}>
                {LANGUAGES.map((l,i)=><LangBar key={i} {...l}/>)}
              </div>
            </Section>
          )}

          {/* ══ FOOTER ══ */}
          <div style={{
            marginTop:"32px", paddingTop:"20px",
            borderTop:`1px solid ${C.border}`,
          }}>
            <div style={{
              display:"flex", justifyContent:"space-between", flexWrap:"wrap",
              gap:"6px", alignItems:"center",
            }}>
              <span style={{ fontFamily:"'Cormorant Garamond',Georgia,serif", fontSize:"0.7rem",
                color:C.dim, letterSpacing:"0.16em", textTransform:"uppercase" }}>
                Hemeshwaran M · Pondicherry, India
              </span>
              <span style={{ fontFamily:"'JetBrains Mono',monospace", fontSize:"0.66rem",
                color:C.dim, fontWeight:"300" }}>
                hemeshwaranm@gmail.com · 7339422391
              </span>
            </div>
            {/* bottom rule */}
            <div style={{ marginTop:"16px", height:"1px",
              background:`linear-gradient(to right, transparent, ${C.gold}33, transparent)` }}/>
          </div>

        </div>
      </div>
    </>
  );
}

