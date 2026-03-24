import { useState, useEffect, useRef } from "react";

const MOVIES = [
  // === TIER 1: BLOCKBUSTERS ($20+) ===
  { id: 1, title: "The Super Mario Galaxy Movie", release: "Apr 1", studio: "Universal/Illumination", genre: "Animation", projGrossLow: 1200, projGrossHigh: 1500, auctionValue: 35, oscarProfile: "Animated Feature frontrunner, tech noms (Score, Sound)", oscarTier: "B", oscarPotentialPts: 25, poster: "🍄" },
  { id: 2, title: "Toy Story 5", release: "Jun 19", studio: "Disney/Pixar", genre: "Animation", projGrossLow: 950, projGrossHigh: 1050, auctionValue: 32, oscarProfile: "Animated Feature, tech noms, BP dark horse (TS3 was nom'd)", oscarTier: "B+", oscarPotentialPts: 30, poster: "🤠" },
  { id: 3, title: "Michael", release: "Apr 24", studio: "Lionsgate", genre: "Biopic", projGrossLow: 650, projGrossHigh: 900, auctionValue: 30, oscarProfile: "Best Picture, Actor, Supp Actor, Screenplay, Costume, Makeup, Score", oscarTier: "A", oscarPotentialPts: 65, poster: "🎤" },
  { id: 4, title: "Disclosure Day", release: "Jun 12", studio: "Universal", genre: "Sci-Fi", projGrossLow: 350, projGrossHigh: 550, auctionValue: 25, oscarProfile: "Best Picture, Director, Cinematography, VFX, Sound, Score", oscarTier: "A", oscarPotentialPts: 55, poster: "🛸" },
  { id: 5, title: "The Mandalorian & Grogu", release: "May 22", studio: "Disney/Lucasfilm", genre: "Sci-Fi", projGrossLow: 500, projGrossHigh: 900, auctionValue: 20, oscarProfile: "VFX, Sound, Score", oscarTier: "C", oscarPotentialPts: 15, poster: "⭐" },
  // === TIER 2: STRONG CONTENDERS ($10–$19) ===
  { id: 6, title: "The Devil Wears Prada 2", release: "May 1", studio: "20th Century", genre: "Comedy-Drama", projGrossLow: 300, projGrossHigh: 600, auctionValue: 16, oscarProfile: "Costume, Actress (Hathaway), Supp Actress (Streep/Blunt)", oscarTier: "B", oscarPotentialPts: 25, poster: "👠" },
  { id: 7, title: "Supergirl", release: "Jun 26", studio: "Warner Bros/DC", genre: "Superhero", projGrossLow: 400, projGrossHigh: 545, auctionValue: 14, oscarProfile: "VFX, Sound, Costume", oscarTier: "C", oscarPotentialPts: 10, poster: "🦸" },
  { id: 8, title: "The Drama", release: "Apr 3", studio: "A24", genre: "Drama", projGrossLow: 75, projGrossHigh: 150, auctionValue: 12, oscarProfile: "Best Picture dark horse, Actor (Pattinson), Actress (Zendaya)", oscarTier: "A-", oscarPotentialPts: 45, poster: "💔" },
  { id: 9, title: "Mortal Kombat II", release: "May 8", studio: "Warner Bros", genre: "Action", projGrossLow: 200, projGrossHigh: 390, auctionValue: 10, oscarProfile: "VFX possible", oscarTier: "D", oscarPotentialPts: 5, poster: "🐉" },
  // === TIER 3: MID-RANGE ($5–$9) ===
  { id: 10, title: "Scary Movie 6", release: "Jun 5", studio: "Paramount", genre: "Comedy-Horror", projGrossLow: 150, projGrossHigh: 200, auctionValue: 8, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "😱" },
  { id: 11, title: "Masters of the Universe", release: "Jun 5", studio: "Amazon/MGM", genre: "Action", projGrossLow: 150, projGrossHigh: 300, auctionValue: 7, oscarProfile: "VFX, Prod Design possible", oscarTier: "D", oscarPotentialPts: 5, poster: "⚔️" },
  { id: 12, title: "I Love Boosters", release: "May 22", studio: "NEON", genre: "Satire", projGrossLow: 30, projGrossHigh: 80, auctionValue: 7, oscarProfile: "Supp Actress (Demi Moore), Costume Design", oscarTier: "B-", oscarPotentialPts: 15, poster: "🛍️" },
  { id: 13, title: "The Mummy (Cronin)", release: "Apr 17", studio: "Warner Bros/Blumhouse", genre: "Horror", projGrossLow: 120, projGrossHigh: 180, auctionValue: 6, oscarProfile: "Makeup & Hairstyling possible", oscarTier: "D", oscarPotentialPts: 5, poster: "🧟" },
  { id: 14, title: "Normal", release: "Apr 17", studio: "Magnolia/Amazon", genre: "Action-Thriller", projGrossLow: 60, projGrossHigh: 120, auctionValue: 5, oscarProfile: "None likely; strong RT (83%) but genre ceiling", oscarTier: "F", oscarPotentialPts: 0, poster: "🔫" },
  { id: 15, title: "Nate Bargatze Movie", release: "May 29", studio: "TBD", genre: "Comedy", projGrossLow: 80, projGrossHigh: 150, auctionValue: 5, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🎙️" },
  // === TIER 4: VALUE PICKS ($3–$4) ===
  { id: 16, title: "Fuze", release: "Apr 24", studio: "TBD", genre: "Thriller", projGrossLow: 40, projGrossHigh: 90, auctionValue: 4, oscarProfile: "Sound, Editing outside shot", oscarTier: "D", oscarPotentialPts: 5, poster: "💣" },
  { id: 17, title: "The Sheep Detectives", release: "May 8", studio: "TBD", genre: "Animation", projGrossLow: 50, projGrossHigh: 100, auctionValue: 4, oscarProfile: "Animated Feature longshot", oscarTier: "D", oscarPotentialPts: 5, poster: "🐑" },
  { id: 18, title: "Jackass 5", release: "Jun 26", studio: "Paramount", genre: "Comedy", projGrossLow: 80, projGrossHigh: 140, auctionValue: 4, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🤕" },
  { id: 19, title: "Power Ballad", release: "Jun 26", studio: "TBD", genre: "Musical-Comedy", projGrossLow: 40, projGrossHigh: 90, auctionValue: 3, oscarProfile: "Original Song possible (Paul Rudd, Nick Jonas)", oscarTier: "D", oscarPotentialPts: 5, poster: "🎸" },
  { id: 20, title: "Mother Mary", release: "Apr 18", studio: "A24", genre: "Drama", projGrossLow: 20, projGrossHigh: 55, auctionValue: 3, oscarProfile: "Actress (Hathaway), Original Song, Costume. David Lowery dir.", oscarTier: "B", oscarPotentialPts: 20, poster: "🌹" },
  { id: 21, title: "4 Kids Walk Into a Bank", release: "Apr 17", studio: "Amazon/MGM", genre: "Action", projGrossLow: 35, projGrossHigh: 70, auctionValue: 3, oscarProfile: "None; Liam Neeson vehicle", oscarTier: "F", oscarPotentialPts: 0, poster: "🏦" },
  { id: 22, title: "PAW Patrol 3: Dino Movie", release: "Apr 3", studio: "Paramount", genre: "Animation", projGrossLow: 80, projGrossHigh: 120, auctionValue: 3, oscarProfile: "Animated Feature longshot (5th slot?)", oscarTier: "D", oscarPotentialPts: 5, poster: "🐾" },
  { id: 23, title: "Over Your Dead Body", release: "Apr 24", studio: "TBD", genre: "Horror", projGrossLow: 25, projGrossHigh: 55, auctionValue: 3, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "⚰️" },
  // === TIER 5: BUDGET PICKS ($2) ===
  { id: 24, title: "You, Me & Tuscany", release: "Apr 10", studio: "TBD", genre: "Romance", projGrossLow: 60, projGrossHigh: 120, auctionValue: 2, oscarProfile: "None likely", oscarTier: "F", oscarPotentialPts: 0, poster: "🇮🇹" },
  { id: 25, title: "Coyote vs. Acme", release: "May 15", studio: "Warner Bros", genre: "Animation/Hybrid", projGrossLow: 60, projGrossHigh: 100, auctionValue: 2, oscarProfile: "VFX, Animation possible; critics darling potential", oscarTier: "D", oscarPotentialPts: 5, poster: "🐺" },
  { id: 26, title: "The Christophers", release: "Apr 10", studio: "TBD", genre: "Comedy", projGrossLow: 15, projGrossHigh: 45, auctionValue: 2, oscarProfile: "Soderbergh dir, Ian McKellen, Michaela Coel. Supp Actor longshot", oscarTier: "C", oscarPotentialPts: 10, poster: "🎭" },
  { id: 27, title: "Backrooms", release: "May 22", studio: "A24", genre: "Horror", projGrossLow: 45, projGrossHigh: 115, auctionValue: 2, oscarProfile: "A24 wildcard; internet-culture IP", oscarTier: "D", oscarPotentialPts: 5, poster: "🚪" },
  { id: 28, title: "Hamlet", release: "Apr 10", studio: "TBD", genre: "Drama", projGrossLow: 10, projGrossHigh: 35, auctionValue: 2, oscarProfile: "Adapted Screenplay, Actor possible if prestige cast delivers", oscarTier: "C", oscarPotentialPts: 10, poster: "💀" },
  { id: 29, title: "Pressure", release: "May 29", studio: "TBD", genre: "Historical Drama", projGrossLow: 20, projGrossHigh: 50, auctionValue: 2, oscarProfile: "Actor (Andrew Scott), Screenplay. D-Day meteorologist story", oscarTier: "B-", oscarPotentialPts: 15, poster: "🌧️" },
  { id: 30, title: "Cliffhanger 2", release: "May 15", studio: "Sony", genre: "Action", projGrossLow: 50, projGrossHigh: 100, auctionValue: 2, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🧗" },
  { id: 31, title: "Exit 8", release: "Apr 10", studio: "TBD", genre: "Horror", projGrossLow: 20, projGrossHigh: 50, auctionValue: 2, oscarProfile: "None; video game adaptation", oscarTier: "F", oscarPotentialPts: 0, poster: "🚇" },
  { id: 32, title: "Passenger", release: "May 22", studio: "TBD", genre: "Thriller", projGrossLow: 30, projGrossHigh: 65, auctionValue: 2, oscarProfile: "None likely", oscarTier: "F", oscarPotentialPts: 0, poster: "✈️" },
  { id: 33, title: "Stop! That! Train!", release: "May 29", studio: "TBD", genre: "Action-Comedy", projGrossLow: 25, projGrossHigh: 55, auctionValue: 2, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🚂" },
  { id: 34, title: "The Death of Robin Hood", release: "Jun 19", studio: "TBD", genre: "Action", projGrossLow: 30, projGrossHigh: 70, auctionValue: 2, oscarProfile: "Costume, Prod Design outside shot", oscarTier: "D", oscarPotentialPts: 5, poster: "🏹" },
  { id: 35, title: "Deep Water", release: "May 8", studio: "TBD", genre: "Thriller", projGrossLow: 25, projGrossHigh: 55, auctionValue: 2, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🌊" },
  // === TIER 6: DOLLAR BIN ($1) ===
  { id: 36, title: "Insidious: The Bleeding", release: "May 8", studio: "Sony", genre: "Horror", projGrossLow: 50, projGrossHigh: 90, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "👁️" },
  { id: 37, title: "Faces of Death", release: "Apr 10", studio: "TBD", genre: "Horror", projGrossLow: 25, projGrossHigh: 60, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "☠️" },
  { id: 38, title: "A Great Awakening", release: "Apr 3", studio: "TBD", genre: "Faith-Based", projGrossLow: 15, projGrossHigh: 40, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "✝️" },
  { id: 39, title: "Girls Like Girls", release: "Jun 19", studio: "TBD", genre: "Romance", projGrossLow: 10, projGrossHigh: 35, auctionValue: 1, oscarProfile: "Original Song possible (Hayley Kiyoko)", oscarTier: "D", oscarPotentialPts: 5, poster: "🌈" },
  { id: 40, title: "The Blue Trail", release: "Apr 3", studio: "TBD", genre: "Drama", projGrossLow: 5, projGrossHigh: 20, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🥾" },
  { id: 41, title: "Broken Bird", release: "Apr 24", studio: "TBD", genre: "Drama", projGrossLow: 5, projGrossHigh: 20, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🐦" },
  { id: 42, title: "Newborn", release: "Apr 10", studio: "TBD", genre: "Horror", projGrossLow: 10, projGrossHigh: 30, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "👶" },
  { id: 43, title: "Mermaid", release: "Apr 10", studio: "TBD", genre: "Fantasy", projGrossLow: 8, projGrossHigh: 25, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🧜" },
  { id: 44, title: "The Yeti", release: "Apr 10", studio: "TBD", genre: "Horror", projGrossLow: 8, projGrossHigh: 25, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "❄️" },
  { id: 45, title: "Rose of Nevada", release: "Jun 19", studio: "TBD", genre: "Thriller", projGrossLow: 10, projGrossHigh: 30, auctionValue: 1, oscarProfile: "George MacKay, Callum Turner. Cinematography outside shot", oscarTier: "D", oscarPotentialPts: 5, poster: "🚢" },
  { id: 46, title: "Leviticus", release: "Jun 19", studio: "TBD", genre: "Horror", projGrossLow: 8, projGrossHigh: 25, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "📖" },
  { id: 47, title: "Beast", release: "Apr 10", studio: "TBD", genre: "Action", projGrossLow: 10, projGrossHigh: 30, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🐗" },
  { id: 48, title: "The Invite", release: "Jun 26", studio: "A24", genre: "Comedy-Drama", projGrossLow: 15, projGrossHigh: 45, auctionValue: 1, oscarProfile: "Olivia Wilde dir. Adapted Screenplay possible. Sundance buzz", oscarTier: "C", oscarPotentialPts: 10, poster: "🍷" },
  { id: 49, title: "California Schemin'", release: "Apr 10", studio: "TBD", genre: "Comedy", projGrossLow: 8, projGrossHigh: 25, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🌴" },
  { id: 50, title: "The Breadwinner", release: "May 29", studio: "TBD", genre: "Drama", projGrossLow: 5, projGrossHigh: 20, auctionValue: 1, oscarProfile: "None", oscarTier: "F", oscarPotentialPts: 0, poster: "🍞" },
];

const TEAMS = [
  { id: 1, name: "Reel Deal", color: "#e63946", icon: "🎬" },
  { id: 2, name: "Box Office Bandits", color: "#457b9d", icon: "💰" },
  { id: 3, name: "Oscar Hunters", color: "#d4a017", icon: "🏆" },
  { id: 4, name: "Popcorn Prophets", color: "#2a9d8f", icon: "🍿" },
  { id: 5, name: "Silver Screen Sharks", color: "#9b5de5", icon: "🦈" },
];

const STRATEGIES = [
  {
    name: "Oscar Bait + Blockbuster",
    icon: "🏆",
    desc: "Pair the #1 grosser with the top Oscar contenders. Maximum dual-scoring ceiling.",
    picks: [1, 3, 4, 8],
    total: 100,
    projGross: "2,275–3,100M",
    projOscar: "60–120 pts",
  },
  {
    name: "Balanced Prestige",
    icon: "🎯",
    desc: "Mario's gross floor plus three films with real awards paths. Costume Design alone could net 15 pts.",
    picks: [1, 3, 6, 7],
    total: 93,
    projGross: "2,550–3,545M",
    projOscar: "40–90 pts",
  },
  {
    name: "Box Office Hammer",
    icon: "💰",
    desc: "Four franchise titans. Bet that $3B+ in gross outpaces everyone else's Oscar bonuses.",
    picks: [1, 2, 5, 7],
    total: 101,
    projGross: "3,050–3,995M",
    projOscar: "20–50 pts",
  },
  {
    name: "Oscar Dark Horse Stack",
    icon: "🎲",
    desc: "Punt on top grossers. Load up on prestige plays and use leftover budget to bid up rivals.",
    picks: [3, 4, 6, 8],
    total: 83,
    projGross: "1,375–2,200M",
    projOscar: "80–150 pts",
  },
];

const SCORING_RULES = [
  { category: "Worldwide Gross", points: "1 pt / $10M", icon: "💵" },
  { category: "Best Picture Nom", points: "+25 pts", icon: "🎬" },
  { category: "Best Director Nom", points: "+15 pts", icon: "🎥" },
  { category: "Acting Nom (each)", points: "+10 pts", icon: "🎭" },
  { category: "Technical Nom (each)", points: "+5 pts", icon: "⚙️" },
  { category: "Any Oscar WIN", points: "2× nom pts", icon: "🏆" },
  { category: "Best Animated Feature Nom", points: "+10 pts", icon: "✨" },
  { category: "Best Animated Feature Win", points: "+20 pts", icon: "🌟" },
];

const tierColor = (tier) => {
  const colors = { "A": "#d4a017", "A-": "#c4932e", "B+": "#6ba368", "B": "#457b9d", "B-": "#5a7d9a", "C": "#8a8a8a", "D": "#6b6b6b", "F": "#4a4a4a" };
  return colors[tier] || "#4a4a4a";
};

function App() {
  const [view, setView] = useState("home");
  const [selectedMovie, setSelectedMovie] = useState(null);
  const [selectedStrategy, setSelectedStrategy] = useState(null);
  const [draftState, setDraftState] = useState(null);
  const [animating, setAnimating] = useState(false);

  useEffect(() => {
    setAnimating(true);
    const t = setTimeout(() => setAnimating(false), 400);
    return () => clearTimeout(t);
  }, [view]);

  const startMockDraft = () => {
    setDraftState({
      currentTeam: 0,
      currentMovie: null,
      currentBid: 0,
      rosters: TEAMS.map(t => ({ ...t, picks: [], budget: 100 })),
      available: [...MOVIES],
      log: [],
      phase: "picking",
    });
    setView("draft");
  };

  return (
    <div style={{
      fontFamily: "'Instrument Sans', 'DM Sans', system-ui, sans-serif",
      background: "#0a0a0f",
      color: "#e8e6e1",
      minHeight: "100vh",
      overflow: "hidden",
    }}>
      <link href="https://fonts.googleapis.com/css2?family=Instrument+Sans:wght@400;500;600;700&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet" />

      {/* NAV */}
      <nav style={{
        display: "flex",
        alignItems: "center",
        justifyContent: "space-between",
        padding: "16px 24px",
        borderBottom: "1px solid rgba(255,255,255,0.06)",
        background: "rgba(10,10,15,0.95)",
        backdropFilter: "blur(20px)",
        position: "sticky",
        top: 0,
        zIndex: 100,
      }}>
        <div style={{ display: "flex", alignItems: "center", gap: 12, cursor: "pointer" }} onClick={() => setView("home")}>
          <div style={{
            width: 36, height: 36, borderRadius: 8,
            background: "linear-gradient(135deg, #d4a017, #e63946)",
            display: "flex", alignItems: "center", justifyContent: "center",
            fontSize: 18, fontWeight: 700,
          }}>🎬</div>
          <div>
            <div style={{ fontWeight: 700, fontSize: 16, letterSpacing: "-0.02em", lineHeight: 1.1 }}>REEL STAKES</div>
            <div style={{ fontSize: 10, color: "#8a8a8a", fontFamily: "'Space Mono', monospace", letterSpacing: "0.08em" }}>FANTASY MOVIE LEAGUE</div>
          </div>
        </div>
        <div style={{ display: "flex", gap: 4 }}>
          {[
            { key: "home", label: "Home" },
            { key: "movies", label: "Movies" },
            { key: "strategies", label: "Strategies" },
            { key: "scoring", label: "Scoring" },
            { key: "draft", label: "Draft" },
            { key: "standings", label: "Standings" },
          ].map(tab => (
            <button key={tab.key} onClick={() => tab.key === "draft" && !draftState ? startMockDraft() : setView(tab.key)} style={{
              padding: "8px 14px",
              borderRadius: 6,
              border: "none",
              background: view === tab.key ? "rgba(212,160,23,0.15)" : "transparent",
              color: view === tab.key ? "#d4a017" : "#6b6b6b",
              fontWeight: view === tab.key ? 600 : 400,
              fontSize: 13,
              cursor: "pointer",
              transition: "all 0.2s",
              fontFamily: "inherit",
            }}>{tab.label}</button>
          ))}
        </div>
      </nav>

      {/* CONTENT */}
      <div style={{
        maxWidth: 1100,
        margin: "0 auto",
        padding: "32px 24px",
        opacity: animating ? 0 : 1,
        transform: animating ? "translateY(12px)" : "translateY(0)",
        transition: "all 0.4s cubic-bezier(0.16, 1, 0.3, 1)",
      }}>

        {/* HOME */}
        {view === "home" && (
          <div>
            <div style={{ textAlign: "center", marginBottom: 56 }}>
              <div style={{
                fontFamily: "'Space Mono', monospace",
                fontSize: 11,
                color: "#d4a017",
                letterSpacing: "0.2em",
                marginBottom: 16,
              }}>APRIL – JUNE 2026 SEASON</div>
              <h1 style={{
                fontSize: 52,
                fontWeight: 700,
                letterSpacing: "-0.04em",
                lineHeight: 1.05,
                margin: "0 0 16px",
                background: "linear-gradient(135deg, #e8e6e1 0%, #d4a017 50%, #e63946 100%)",
                WebkitBackgroundClip: "text",
                WebkitTextFillColor: "transparent",
              }}>Draft Your Box Office<br />Empire</h1>
              <p style={{ color: "#8a8a8a", fontSize: 16, maxWidth: 520, margin: "0 auto 32px", lineHeight: 1.6 }}>
                5 managers. $100 budgets. 50 films. 10 picks each. Score points through worldwide gross AND Oscar nominations. Every dollar counts.
              </p>
              <div style={{ display: "flex", gap: 12, justifyContent: "center" }}>
                <button onClick={startMockDraft} style={{
                  padding: "14px 28px", borderRadius: 8,
                  background: "linear-gradient(135deg, #d4a017, #c4932e)",
                  color: "#0a0a0f", fontWeight: 700, fontSize: 15,
                  border: "none", cursor: "pointer", fontFamily: "inherit",
                  letterSpacing: "-0.01em",
                }}>Start Mock Draft</button>
                <button onClick={() => setView("movies")} style={{
                  padding: "14px 28px", borderRadius: 8,
                  background: "rgba(255,255,255,0.06)",
                  color: "#e8e6e1", fontWeight: 600, fontSize: 15,
                  border: "1px solid rgba(255,255,255,0.08)", cursor: "pointer", fontFamily: "inherit",
                }}>Browse Movies</button>
              </div>
            </div>

            {/* Quick Stats */}
            <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 16, marginBottom: 48 }}>
              {[
                { label: "Films in Pool", value: "50", sub: "Apr–Jun 2026" },
                { label: "Budget per Team", value: "$100", sub: "10 picks each" },
                { label: "League Size", value: "5", sub: "managers" },
                { label: "Scoring", value: "Dual", sub: "Gross + Oscars" },
              ].map((s, i) => (
                <div key={i} style={{
                  background: "rgba(255,255,255,0.03)",
                  border: "1px solid rgba(255,255,255,0.06)",
                  borderRadius: 12, padding: "24px 20px", textAlign: "center",
                }}>
                  <div style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", color: "#d4a017" }}>{s.value}</div>
                  <div style={{ fontSize: 13, fontWeight: 600, marginTop: 4 }}>{s.label}</div>
                  <div style={{ fontSize: 11, color: "#6b6b6b", fontFamily: "'Space Mono', monospace", marginTop: 2 }}>{s.sub}</div>
                </div>
              ))}
            </div>

            {/* Top 5 Preview */}
            <div style={{ marginBottom: 16 }}>
              <div style={{ display: "flex", justifyContent: "space-between", alignItems: "baseline", marginBottom: 16 }}>
                <h2 style={{ fontSize: 20, fontWeight: 700, letterSpacing: "-0.02em", margin: 0 }}>Top Assets</h2>
                <button onClick={() => setView("movies")} style={{ background: "none", border: "none", color: "#d4a017", fontSize: 13, cursor: "pointer", fontFamily: "inherit" }}>View all 20 →</button>
              </div>
              {MOVIES.slice(0, 5).map((m, i) => (
                <div key={m.id} onClick={() => { setSelectedMovie(m); setView("movies"); }} style={{
                  display: "grid",
                  gridTemplateColumns: "40px 56px 1fr 100px 80px 70px",
                  alignItems: "center",
                  padding: "14px 16px",
                  borderRadius: 10,
                  background: i % 2 === 0 ? "rgba(255,255,255,0.02)" : "transparent",
                  cursor: "pointer",
                  transition: "background 0.2s",
                  gap: 12,
                }}>
                  <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 12, color: "#6b6b6b" }}>#{i + 1}</div>
                  <div style={{ fontSize: 32 }}>{m.poster}</div>
                  <div>
                    <div style={{ fontWeight: 600, fontSize: 14, letterSpacing: "-0.01em" }}>{m.title}</div>
                    <div style={{ fontSize: 11, color: "#6b6b6b" }}>{m.release} · {m.studio}</div>
                  </div>
                  <div style={{ textAlign: "right" }}>
                    <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 13, color: "#2a9d8f" }}>${m.projGrossLow}–{m.projGrossHigh}M</div>
                    <div style={{ fontSize: 10, color: "#6b6b6b" }}>WW GROSS</div>
                  </div>
                  <div style={{ textAlign: "center" }}>
                    <span style={{
                      display: "inline-block", padding: "3px 10px", borderRadius: 4,
                      background: `${tierColor(m.oscarTier)}22`,
                      color: tierColor(m.oscarTier),
                      fontSize: 12, fontWeight: 700, fontFamily: "'Space Mono', monospace",
                    }}>{m.oscarTier}</span>
                    <div style={{ fontSize: 10, color: "#6b6b6b", marginTop: 2 }}>OSCAR</div>
                  </div>
                  <div style={{ textAlign: "right" }}>
                    <div style={{
                      fontFamily: "'Space Mono', monospace", fontSize: 18, fontWeight: 700, color: "#d4a017",
                    }}>${m.auctionValue}</div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* MOVIES VIEW */}
        {view === "movies" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h2 style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", margin: "0 0 6px" }}>Player Pool</h2>
              <p style={{ color: "#6b6b6b", fontSize: 14, margin: 0 }}>50 films · April–June 2026 · Sorted by auction value</p>
            </div>

            {/* Legend */}
            <div style={{
              display: "flex", gap: 16, marginBottom: 20, padding: "12px 16px",
              background: "rgba(255,255,255,0.02)", borderRadius: 8, fontSize: 11, color: "#8a8a8a",
              fontFamily: "'Space Mono', monospace",
            }}>
              <span>Oscar Tiers:</span>
              {["A", "A-", "B+", "B", "B-", "C", "D", "F"].map(t => (
                <span key={t} style={{ color: tierColor(t), fontWeight: 700 }}>{t}</span>
              ))}
            </div>

            {/* Headers */}
            <div style={{
              display: "grid",
              gridTemplateColumns: "36px 44px 1fr 110px 70px 80px 60px",
              padding: "8px 16px",
              fontSize: 10,
              fontFamily: "'Space Mono', monospace",
              color: "#6b6b6b",
              letterSpacing: "0.06em",
              gap: 12,
            }}>
              <div>#</div>
              <div></div>
              <div>TITLE</div>
              <div style={{ textAlign: "right" }}>PROJ. WW GROSS</div>
              <div style={{ textAlign: "center" }}>OSCAR</div>
              <div style={{ textAlign: "right" }}>OSCAR PTS</div>
              <div style={{ textAlign: "right" }}>VALUE</div>
            </div>

            {MOVIES.map((m, i) => (
              <div key={m.id} onClick={() => setSelectedMovie(selectedMovie?.id === m.id ? null : m)} style={{
                display: "grid",
                gridTemplateColumns: "36px 44px 1fr 110px 70px 80px 60px",
                alignItems: "center",
                padding: "12px 16px",
                borderRadius: 10,
                background: selectedMovie?.id === m.id ? "rgba(212,160,23,0.08)" : i % 2 === 0 ? "rgba(255,255,255,0.015)" : "transparent",
                border: selectedMovie?.id === m.id ? "1px solid rgba(212,160,23,0.2)" : "1px solid transparent",
                cursor: "pointer",
                transition: "all 0.2s",
                gap: 12,
                marginBottom: 2,
              }}>
                <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 11, color: "#6b6b6b" }}>{i + 1}</div>
                <div style={{ fontSize: 26 }}>{m.poster}</div>
                <div>
                  <div style={{ fontWeight: 600, fontSize: 13.5, letterSpacing: "-0.01em" }}>{m.title}</div>
                  <div style={{ fontSize: 11, color: "#6b6b6b" }}>{m.release} · {m.genre} · {m.studio}</div>
                </div>
                <div style={{ textAlign: "right", fontFamily: "'Space Mono', monospace", fontSize: 12, color: "#2a9d8f" }}>
                  ${m.projGrossLow}–{m.projGrossHigh}M
                </div>
                <div style={{ textAlign: "center" }}>
                  <span style={{
                    display: "inline-block", padding: "2px 8px", borderRadius: 4,
                    background: `${tierColor(m.oscarTier)}22`,
                    color: tierColor(m.oscarTier),
                    fontSize: 11, fontWeight: 700, fontFamily: "'Space Mono', monospace",
                  }}>{m.oscarTier}</span>
                </div>
                <div style={{ textAlign: "right", fontFamily: "'Space Mono', monospace", fontSize: 12, color: "#8a8a8a" }}>
                  +{m.oscarPotentialPts}
                </div>
                <div style={{ textAlign: "right", fontFamily: "'Space Mono', monospace", fontSize: 16, fontWeight: 700, color: "#d4a017" }}>
                  ${m.auctionValue}
                </div>
              </div>
            ))}

            {/* Detail Panel */}
            {selectedMovie && (
              <div style={{
                marginTop: 24, padding: 28, borderRadius: 14,
                background: "rgba(255,255,255,0.03)",
                border: "1px solid rgba(255,255,255,0.08)",
              }}>
                <div style={{ display: "flex", alignItems: "center", gap: 16, marginBottom: 20 }}>
                  <div style={{ fontSize: 48 }}>{selectedMovie.poster}</div>
                  <div>
                    <h3 style={{ margin: 0, fontSize: 22, fontWeight: 700, letterSpacing: "-0.02em" }}>{selectedMovie.title}</h3>
                    <div style={{ color: "#8a8a8a", fontSize: 13 }}>{selectedMovie.release} · {selectedMovie.studio} · {selectedMovie.genre}</div>
                  </div>
                  <div style={{ marginLeft: "auto", textAlign: "right" }}>
                    <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 32, fontWeight: 700, color: "#d4a017" }}>${selectedMovie.auctionValue}</div>
                    <div style={{ fontSize: 11, color: "#6b6b6b" }}>AUCTION VALUE</div>
                  </div>
                </div>
                <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr 1fr", gap: 16 }}>
                  <div style={{ padding: 16, borderRadius: 10, background: "rgba(42,157,143,0.08)", border: "1px solid rgba(42,157,143,0.15)" }}>
                    <div style={{ fontSize: 11, color: "#2a9d8f", fontFamily: "'Space Mono', monospace", marginBottom: 6 }}>PROJECTED GROSS</div>
                    <div style={{ fontSize: 20, fontWeight: 700 }}>${selectedMovie.projGrossLow}–{selectedMovie.projGrossHigh}M</div>
                    <div style={{ fontSize: 12, color: "#6b6b6b", marginTop: 4 }}>{Math.round(selectedMovie.projGrossLow / 10)}–{Math.round(selectedMovie.projGrossHigh / 10)} gross pts</div>
                  </div>
                  <div style={{ padding: 16, borderRadius: 10, background: "rgba(212,160,23,0.08)", border: "1px solid rgba(212,160,23,0.15)" }}>
                    <div style={{ fontSize: 11, color: "#d4a017", fontFamily: "'Space Mono', monospace", marginBottom: 6 }}>OSCAR CEILING</div>
                    <div style={{ fontSize: 20, fontWeight: 700 }}>+{selectedMovie.oscarPotentialPts} pts</div>
                    <div style={{ fontSize: 12, color: "#6b6b6b", marginTop: 4 }}>Tier {selectedMovie.oscarTier}</div>
                  </div>
                  <div style={{ padding: 16, borderRadius: 10, background: "rgba(230,57,70,0.08)", border: "1px solid rgba(230,57,70,0.15)" }}>
                    <div style={{ fontSize: 11, color: "#e63946", fontFamily: "'Space Mono', monospace", marginBottom: 6 }}>TOTAL CEILING</div>
                    <div style={{ fontSize: 20, fontWeight: 700 }}>{Math.round(selectedMovie.projGrossHigh / 10) + selectedMovie.oscarPotentialPts} pts</div>
                    <div style={{ fontSize: 12, color: "#6b6b6b", marginTop: 4 }}>Gross + Oscar max</div>
                  </div>
                </div>
                <div style={{ marginTop: 16, padding: "12px 16px", borderRadius: 8, background: "rgba(255,255,255,0.02)", fontSize: 13, color: "#8a8a8a" }}>
                  <span style={{ fontWeight: 600, color: "#e8e6e1" }}>Oscar Profile: </span>{selectedMovie.oscarProfile}
                </div>
              </div>
            )}
          </div>
        )}

        {/* STRATEGIES VIEW */}
        {view === "strategies" && (
          <div>
            <h2 style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", margin: "0 0 6px" }}>Draft Strategies</h2>
            <p style={{ color: "#6b6b6b", fontSize: 14, margin: "0 0 28px" }}>Four core archetypes for your first 4 picks — fill remaining 6 slots from the value tiers</p>

            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 20 }}>
              {STRATEGIES.map((s, si) => (
                <div key={si} onClick={() => setSelectedStrategy(selectedStrategy === si ? null : si)} style={{
                  padding: 24, borderRadius: 14,
                  background: selectedStrategy === si ? "rgba(212,160,23,0.06)" : "rgba(255,255,255,0.02)",
                  border: selectedStrategy === si ? "1px solid rgba(212,160,23,0.2)" : "1px solid rgba(255,255,255,0.06)",
                  cursor: "pointer", transition: "all 0.3s",
                }}>
                  <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 12 }}>
                    <span style={{ fontSize: 28 }}>{s.icon}</span>
                    <div>
                      <div style={{ fontWeight: 700, fontSize: 16, letterSpacing: "-0.01em" }}>{s.name}</div>
                      <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 11, color: "#d4a017" }}>${s.total} / $100</div>
                    </div>
                  </div>
                  <p style={{ fontSize: 13, color: "#8a8a8a", lineHeight: 1.5, margin: "0 0 16px" }}>{s.desc}</p>

                  <div style={{ display: "flex", gap: 8, marginBottom: 16 }}>
                    {s.picks.map(pid => {
                      const mv = MOVIES.find(m => m.id === pid);
                      return (
                        <div key={pid} style={{
                          flex: 1, padding: "10px 8px", borderRadius: 8,
                          background: "rgba(255,255,255,0.03)", textAlign: "center",
                          border: "1px solid rgba(255,255,255,0.05)",
                        }}>
                          <div style={{ fontSize: 22, marginBottom: 4 }}>{mv.poster}</div>
                          <div style={{ fontSize: 10, fontWeight: 600, lineHeight: 1.2 }}>{mv.title.length > 16 ? mv.title.slice(0, 15) + "…" : mv.title}</div>
                          <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 11, color: "#d4a017", marginTop: 2 }}>${mv.auctionValue}</div>
                        </div>
                      );
                    })}
                  </div>

                  <div style={{ display: "flex", gap: 12 }}>
                    <div style={{ flex: 1, padding: "8px 12px", borderRadius: 6, background: "rgba(42,157,143,0.08)", fontSize: 11 }}>
                      <span style={{ color: "#2a9d8f", fontFamily: "'Space Mono', monospace" }}>GROSS</span>
                      <div style={{ fontWeight: 600, fontSize: 13, marginTop: 2 }}>{s.projGross}</div>
                    </div>
                    <div style={{ flex: 1, padding: "8px 12px", borderRadius: 6, background: "rgba(212,160,23,0.08)", fontSize: 11 }}>
                      <span style={{ color: "#d4a017", fontFamily: "'Space Mono', monospace" }}>OSCAR</span>
                      <div style={{ fontWeight: 600, fontSize: 13, marginTop: 2 }}>{s.projOscar}</div>
                    </div>
                    <div style={{ flex: 1, padding: "8px 12px", borderRadius: 6, background: "rgba(255,255,255,0.04)", fontSize: 11 }}>
                      <span style={{ color: "#8a8a8a", fontFamily: "'Space Mono', monospace" }}>BUDGET LEFT</span>
                      <div style={{ fontWeight: 600, fontSize: 13, marginTop: 2 }}>${100 - s.total}</div>
                    </div>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* SCORING VIEW */}
        {view === "scoring" && (
          <div>
            <h2 style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", margin: "0 0 6px" }}>Scoring System</h2>
            <p style={{ color: "#6b6b6b", fontSize: 14, margin: "0 0 32px" }}>Dual-weighted: Box office performance + Academy Awards recognition</p>

            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 20, marginBottom: 40 }}>
              {SCORING_RULES.map((r, i) => (
                <div key={i} style={{
                  display: "flex", alignItems: "center", gap: 16,
                  padding: "18px 20px", borderRadius: 12,
                  background: "rgba(255,255,255,0.02)",
                  border: "1px solid rgba(255,255,255,0.06)",
                }}>
                  <span style={{ fontSize: 28 }}>{r.icon}</span>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontWeight: 600, fontSize: 14 }}>{r.category}</div>
                  </div>
                  <div style={{
                    fontFamily: "'Space Mono', monospace", fontSize: 14, fontWeight: 700,
                    color: "#d4a017", background: "rgba(212,160,23,0.1)",
                    padding: "4px 12px", borderRadius: 6,
                  }}>{r.points}</div>
                </div>
              ))}
            </div>

            <div style={{
              padding: 24, borderRadius: 14,
              background: "linear-gradient(135deg, rgba(212,160,23,0.06), rgba(230,57,70,0.04))",
              border: "1px solid rgba(212,160,23,0.12)",
            }}>
              <h3 style={{ margin: "0 0 12px", fontSize: 16, fontWeight: 700 }}>Example Scoring: Michael (MJ Biopic)</h3>
              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12, fontSize: 13 }}>
                <div>
                  <div style={{ color: "#6b6b6b", marginBottom: 4, fontFamily: "'Space Mono', monospace", fontSize: 11 }}>IF $800M WORLDWIDE</div>
                  <div style={{ fontWeight: 600 }}>80 gross points</div>
                </div>
                <div>
                  <div style={{ color: "#6b6b6b", marginBottom: 4, fontFamily: "'Space Mono', monospace", fontSize: 11 }}>IF 7 OSCAR NOMS (BP, DIR, ACTOR, SUPP ACTOR, SCREENPLAY, COSTUME, SCORE)</div>
                  <div style={{ fontWeight: 600 }}>25 + 15 + 10 + 10 + 5 + 5 + 5 = 75 pts</div>
                </div>
                <div>
                  <div style={{ color: "#6b6b6b", marginBottom: 4, fontFamily: "'Space Mono', monospace", fontSize: 11 }}>IF BEST ACTOR WIN</div>
                  <div style={{ fontWeight: 600 }}>+10 bonus (2× the 10pt nom) = 85 Oscar pts</div>
                </div>
                <div>
                  <div style={{ color: "#d4a017", marginBottom: 4, fontFamily: "'Space Mono', monospace", fontSize: 11 }}>TOTAL POTENTIAL</div>
                  <div style={{ fontWeight: 700, fontSize: 18, color: "#d4a017" }}>165 points</div>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* DRAFT VIEW */}
        {view === "draft" && draftState && (
          <div>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 24 }}>
              <div>
                <h2 style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", margin: "0 0 4px" }}>Auction Draft Room</h2>
                <p style={{ color: "#6b6b6b", fontSize: 13, margin: 0 }}>
                  {draftState.rosters.reduce((sum, r) => sum + r.picks.length, 0)} / 50 picks made
                </p>
              </div>
              <button onClick={startMockDraft} style={{
                padding: "10px 20px", borderRadius: 8,
                background: "rgba(230,57,70,0.1)", color: "#e63946",
                border: "1px solid rgba(230,57,70,0.2)", cursor: "pointer",
                fontFamily: "inherit", fontSize: 13, fontWeight: 600,
              }}>Reset Draft</button>
            </div>

            {/* Team Budgets */}
            <div style={{ display: "grid", gridTemplateColumns: "repeat(5, 1fr)", gap: 10, marginBottom: 24 }}>
              {draftState.rosters.map((team, ti) => (
                <div key={team.id} style={{
                  padding: "14px 12px", borderRadius: 10,
                  background: ti === draftState.currentTeam ? `${team.color}18` : "rgba(255,255,255,0.02)",
                  border: ti === draftState.currentTeam ? `2px solid ${team.color}` : "1px solid rgba(255,255,255,0.06)",
                  textAlign: "center",
                }}>
                  <div style={{ fontSize: 20 }}>{team.icon}</div>
                  <div style={{ fontSize: 12, fontWeight: 600, marginTop: 4 }}>{team.name}</div>
                  <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 16, fontWeight: 700, color: team.color, marginTop: 4 }}>${team.budget}</div>
                  <div style={{ fontSize: 10, color: "#6b6b6b" }}>{team.picks.length}/10 picks</div>
                  {team.picks.map(p => (
                    <div key={p.id} style={{
                      marginTop: 4, fontSize: 10, padding: "3px 6px",
                      borderRadius: 4, background: "rgba(255,255,255,0.04)",
                    }}>{p.poster} {p.title.length > 12 ? p.title.slice(0, 11) + "…" : p.title}</div>
                  ))}
                </div>
              ))}
            </div>

            {/* Available Movies */}
            <div style={{
              padding: 20, borderRadius: 14,
              background: "rgba(255,255,255,0.02)",
              border: "1px solid rgba(255,255,255,0.06)",
            }}>
              <div style={{ fontWeight: 600, fontSize: 14, marginBottom: 14 }}>
                {draftState.rosters[draftState.currentTeam]?.icon} {draftState.rosters[draftState.currentTeam]?.name}'s Pick
              </div>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(5, 1fr)", gap: 8, maxHeight: 480, overflowY: "auto" }}>
                {draftState.available.map(m => {
                  const team = draftState.rosters[draftState.currentTeam];
                  const canAfford = team && team.budget >= m.auctionValue && team.picks.length < 10;
                  return (
                    <button key={m.id} disabled={!canAfford} onClick={() => {
                      const newRosters = [...draftState.rosters];
                      newRosters[draftState.currentTeam] = {
                        ...team,
                        picks: [...team.picks, m],
                        budget: team.budget - m.auctionValue,
                      };
                      const newAvailable = draftState.available.filter(mv => mv.id !== m.id);
                      let nextTeam = draftState.currentTeam;
                      for (let i = 1; i <= 5; i++) {
                        const candidate = (draftState.currentTeam + i) % 5;
                        if (newRosters[candidate].picks.length < 10) {
                          nextTeam = candidate;
                          break;
                        }
                      }
                      setDraftState({
                        ...draftState,
                        rosters: newRosters,
                        available: newAvailable,
                        currentTeam: nextTeam,
                        log: [...draftState.log, { team: team.name, movie: m.title, price: m.auctionValue }],
                      });
                    }} style={{
                      padding: "12px 10px", borderRadius: 10,
                      background: canAfford ? "rgba(255,255,255,0.03)" : "rgba(255,255,255,0.01)",
                      border: canAfford ? "1px solid rgba(255,255,255,0.08)" : "1px solid rgba(255,255,255,0.03)",
                      cursor: canAfford ? "pointer" : "not-allowed",
                      opacity: canAfford ? 1 : 0.35,
                      textAlign: "center", fontFamily: "inherit", color: "#e8e6e1",
                      transition: "all 0.2s",
                    }}>
                      <div style={{ fontSize: 24, marginBottom: 4 }}>{m.poster}</div>
                      <div style={{ fontSize: 11, fontWeight: 600, lineHeight: 1.2, minHeight: 28 }}>
                        {m.title.length > 18 ? m.title.slice(0, 17) + "…" : m.title}
                      </div>
                      <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 14, fontWeight: 700, color: "#d4a017", marginTop: 4 }}>${m.auctionValue}</div>
                      <div style={{
                        marginTop: 4, fontSize: 9, padding: "2px 6px", borderRadius: 3,
                        background: `${tierColor(m.oscarTier)}22`, color: tierColor(m.oscarTier),
                        display: "inline-block", fontFamily: "'Space Mono', monospace", fontWeight: 700,
                      }}>{m.oscarTier}</div>
                    </button>
                  );
                })}
              </div>
            </div>

            {/* Draft Log */}
            {draftState.log.length > 0 && (
              <div style={{ marginTop: 20 }}>
                <div style={{ fontWeight: 600, fontSize: 13, marginBottom: 8, color: "#8a8a8a" }}>Draft Log</div>
                <div style={{ maxHeight: 160, overflowY: "auto", fontSize: 12, fontFamily: "'Space Mono', monospace" }}>
                  {draftState.log.map((entry, i) => (
                    <div key={i} style={{ padding: "6px 0", borderBottom: "1px solid rgba(255,255,255,0.04)", color: "#8a8a8a" }}>
                      <span style={{ color: "#e8e6e1" }}>{entry.team}</span> drafts <span style={{ color: "#d4a017" }}>{entry.movie}</span> for <span style={{ color: "#2a9d8f" }}>${entry.price}</span>
                    </div>
                  ))}
                </div>
              </div>
            )}
          </div>
        )}

        {/* STANDINGS VIEW */}
        {view === "standings" && (
          <div>
            <h2 style={{ fontSize: 28, fontWeight: 700, letterSpacing: "-0.03em", margin: "0 0 6px" }}>League Standings</h2>
            <p style={{ color: "#6b6b6b", fontSize: 14, margin: "0 0 28px" }}>
              {draftState ? "Based on current draft rosters · Projected scores" : "Complete a draft to see standings"}
            </p>

            {draftState && draftState.rosters.filter(r => r.picks.length > 0).length > 0 ? (
              <div>
                {draftState.rosters
                  .map(team => {
                    const grossPts = team.picks.reduce((sum, m) => sum + Math.round((m.projGrossLow + m.projGrossHigh) / 2 / 10), 0);
                    const oscarPts = team.picks.reduce((sum, m) => sum + m.oscarPotentialPts, 0);
                    return { ...team, grossPts, oscarPts, totalPts: grossPts + oscarPts };
                  })
                  .sort((a, b) => b.totalPts - a.totalPts)
                  .map((team, rank) => (
                    <div key={team.id} style={{
                      padding: 20, borderRadius: 14, marginBottom: 12,
                      background: rank === 0 ? "rgba(212,160,23,0.06)" : "rgba(255,255,255,0.02)",
                      border: rank === 0 ? "1px solid rgba(212,160,23,0.15)" : "1px solid rgba(255,255,255,0.06)",
                    }}>
                      <div style={{ display: "flex", alignItems: "center", gap: 14, marginBottom: 14 }}>
                        <div style={{
                          width: 36, height: 36, borderRadius: 8,
                          background: `${team.color}22`, color: team.color,
                          display: "flex", alignItems: "center", justifyContent: "center",
                          fontSize: 14, fontWeight: 700, fontFamily: "'Space Mono', monospace",
                        }}>#{rank + 1}</div>
                        <span style={{ fontSize: 22 }}>{team.icon}</span>
                        <div style={{ flex: 1 }}>
                          <div style={{ fontWeight: 700, fontSize: 16 }}>{team.name}</div>
                          <div style={{ fontSize: 11, color: "#6b6b6b" }}>${100 - team.budget} spent · {team.picks.length}/10 picks</div>
                        </div>
                        <div style={{ textAlign: "right" }}>
                          <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 28, fontWeight: 700, color: rank === 0 ? "#d4a017" : "#e8e6e1" }}>
                            {team.totalPts}
                          </div>
                          <div style={{ fontSize: 10, color: "#6b6b6b" }}>PROJ. PTS</div>
                        </div>
                      </div>

                      <div style={{ display: "flex", gap: 8, marginBottom: 12 }}>
                        <div style={{ flex: 1, padding: "8px 12px", borderRadius: 6, background: "rgba(42,157,143,0.08)", fontSize: 12 }}>
                          <span style={{ color: "#2a9d8f", fontFamily: "'Space Mono', monospace", fontSize: 10 }}>GROSS</span>
                          <div style={{ fontWeight: 700, fontSize: 16, marginTop: 2 }}>{team.grossPts}</div>
                        </div>
                        <div style={{ flex: 1, padding: "8px 12px", borderRadius: 6, background: "rgba(212,160,23,0.08)", fontSize: 12 }}>
                          <span style={{ color: "#d4a017", fontFamily: "'Space Mono', monospace", fontSize: 10 }}>OSCAR</span>
                          <div style={{ fontWeight: 700, fontSize: 16, marginTop: 2 }}>{team.oscarPts}</div>
                        </div>
                      </div>

                      <div style={{ display: "flex", gap: 6 }}>
                        {team.picks.map(m => (
                          <div key={m.id} style={{
                            flex: 1, padding: "8px 6px", borderRadius: 8,
                            background: "rgba(255,255,255,0.03)", textAlign: "center",
                            border: "1px solid rgba(255,255,255,0.05)",
                          }}>
                            <div style={{ fontSize: 20 }}>{m.poster}</div>
                            <div style={{ fontSize: 9, fontWeight: 600, marginTop: 2 }}>
                              {m.title.length > 14 ? m.title.slice(0, 13) + "…" : m.title}
                            </div>
                            <div style={{ fontFamily: "'Space Mono', monospace", fontSize: 10, color: "#d4a017" }}>${m.auctionValue}</div>
                          </div>
                        ))}
                      </div>
                    </div>
                  ))}
              </div>
            ) : (
              <div style={{
                textAlign: "center", padding: "60px 20px",
                background: "rgba(255,255,255,0.02)", borderRadius: 14,
                border: "1px solid rgba(255,255,255,0.06)",
              }}>
                <div style={{ fontSize: 48, marginBottom: 16 }}>🏟️</div>
                <div style={{ fontSize: 16, fontWeight: 600, marginBottom: 8 }}>No rosters yet</div>
                <div style={{ fontSize: 13, color: "#6b6b6b", marginBottom: 20 }}>Complete the auction draft to populate standings</div>
                <button onClick={startMockDraft} style={{
                  padding: "12px 24px", borderRadius: 8,
                  background: "linear-gradient(135deg, #d4a017, #c4932e)",
                  color: "#0a0a0f", fontWeight: 700, fontSize: 14,
                  border: "none", cursor: "pointer", fontFamily: "inherit",
                }}>Start Draft</button>
              </div>
            )}
          </div>
        )}
      </div>
    </div>
  );
}

export default App;
