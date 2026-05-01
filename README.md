import React, { useState, useMemo } from 'react';
import { 
  BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, Cell 
} from 'recharts';
import { 
  Target, TrendingDown, TrendingUp, AlertTriangle, BarChart2, Zap, 
  Activity, Shield, Info, Layout, Fingerprint, BarChart3, Clock, 
  Flame, Crosshair, Waves, Compass, ArrowRightCircle, History
} from 'lucide-react';

const App = () => {
  // Initial date set to April 30
  const [activeDate, setActiveDate] = useState('April 30');

  // Master Data Repository
  const data = {
    'April 30': {
      sessionName: "Bullish Expansion • Multi-Whale Acceleration",
      range: "86.25 pts (7,165.25 → 7,251.50)",
      high: "7,251.50",
      low: "7,165.25",
      largestBid: "410 @ 7,165.25",
      largestAsk: "1,055 @ 7,232.00",
      totalBidVol: 9150,
      totalAskVol: 14820,
      netDelta: 5670,
      whalePrints: 2,
      sellingPressure: "38%",
      sentiment: "Bullish",
      description: "April 30 was a powerhouse session for institutional buyers. Two separate 1,000+ lot 'Whale' prints signaled aggressive trend entry points at 7,211 and 7,232. The day ended with an exhaustive 'Closing Chase' cluster as the market pushed through the 7,250 level, indicating a significant inventory imbalance being carried into the new month.",
      timelineData: [
        { time: '08:21 AM', vol: 1018, price: '7211.25', side: 'ask', label: 'MORNING WHALE: Trend Catalyst' },
        { time: '10:30 AM', vol: 561, price: '7175.00', side: 'ask', label: 'Value Area Accumulation' },
        { time: '10:37 AM', vol: 410, price: '7165.25', side: 'bid', label: 'Session Low Defense' },
        { time: '02:00 PM', vol: 536, price: '7227.75', side: 'ask', label: 'Mid-Day Absorption' },
        { time: '02:02 PM', vol: 1055, price: '7232.00', side: 'ask', label: 'AFTERNOON WHALE: Momentum Spike' },
        { time: '02:36 PM', vol: 564, price: '7238.50', side: 'ask', label: 'Trend Confirmation' },
        { time: '03:59 PM', vol: 3200, price: '7244.75', side: 'ask', label: 'Closing Chase Imbalance' },
      ],
      interpretation: [
        { title: "Whale Acceleration", price: "7232.00", desc: "The 1,055 lot print at 02:02 PM completely erased any remaining sell-side liquidity, forcing price into a high-velocity squeeze.", color: "blue" },
        { title: "Psychological Floor", price: "7211.00", desc: "Early whale buying established 7,211 as the structural base for the afternoon breakout.", color: "emerald" },
        { title: "Close Cluster", price: "7244 - 7251", desc: "Massive volume concentration at the bell. Acceptance at session highs suggests buyers are not yet satiated.", color: "orange" },
        { title: "Rotational Low", price: "7165.25", desc: "The defensive bid block here caught the morning low, creating a 'V-bottom' structure that held all day.", color: "rose" }
      ],
      notableZones: [
        { level: "7,232.00 Pivot", title: "Institutional Power Node", desc: "1,055 lot Whale print. This is the primary support for any pullback in the next session.", type: "ask", tags: ["WHALE", "MOMENTUM"], strength: 97, status: "Critical Support" },
        { level: "7,244.00 - 7,251.50", title: "Closing Imbalance", desc: "Late session squeeze zone. Dense cluster of high-volume market buys.", type: "ask", tags: ["CLOSE", "CHASE"], strength: 91, status: "Active Target" },
        { level: "7,211.25 Base", title: "Whale Anchor", desc: "The 1,018 lot print. This level serves as the 'Line in the Sand' for the bullish trend.", type: "ask", tags: ["WHALE", "SUPPORT"], strength: 89, status: "Major Support" }
      ],
      nextSessionPrep: {
        bias: "Strong Bullish Continuity",
        keyLevels: [
          { price: "7,251.50", label: "Monthly High Breakout", action: "Watch for Momentum Expansion" },
          { price: "7,232.00", label: "Whale Support Node", action: "High Confidence Long on Retest" },
          { price: "7,211.25", label: "Primary Trend Floor", action: "Must Hold for Bullish Thesis" }
        ],
        thesis: "Two whale prints on the ask in one session is rare. The trend is vertical. Look for 7,232 to provide immediate support on any opening shakeout. Bulls target 7,300 handle."
      }
    },
    'April 29': {
      sessionName: "Late Distribution • Whale Sell @ 7,174",
      range: "50.25 pts (7,134.75 → 7,185.00)",
      high: "7,185.00", low: "7,134.75", largestBid: "974 @ 7,174.25", largestAsk: "684 @ 7,179.00",
      totalBidVol: 12450, totalAskVol: 9120, netDelta: -3330, whalePrints: 2, sellingPressure: "58%", sentiment: "Bearish",
      description: "Shift in institutional sentiment during the final hour. Massive 974-contract sell block hit the bid late.",
      timelineData: [{ time: '04:07 PM', vol: 974, price: '7174.25', side: 'bid', label: 'Whale Distribution' }],
      interpretation: [{ title: "Whale Liquidation", price: "7174.25", desc: "Major institutional exit.", color: "rose" }],
      notableZones: [{ level: "7,174.25 Resistance", title: "Whale Sell Wall", desc: "Primary overhead resistance.", type: "bid", tags: ["WHALE"], strength: 96, status: "Critical Resistance" }],
      nextSessionPrep: { bias: "Bearish Distribution", keyLevels: [{ price: "7,174.25", label: "Supply Wall", action: "Sell Rejections" }], thesis: "Big money leaning short." }
    }
  };

  const formatNum = (val) => {
    if (val === undefined || val === null) return "0";
    return typeof val === "number" ? val.toLocaleString() : String(val);
  };

  const activeData = useMemo(() => {
    return data[activeDate] || {
      sessionName: "N/A", range: "0 pts", high: "0", low: "0", largestBid: "0 @ 0", largestAsk: "0 @ 0",
      totalBidVol: 0, totalAskVol: 0, netDelta: 0, whalePrints: 0, sellingPressure: "0%", sentiment: "Neutral",
      description: "", timelineData: [], interpretation: [], notableZones: [], nextSessionPrep: null
    };
  }, [activeDate]);

  const bidPercentage = useMemo(() => {
    const total = (activeData.totalBidVol || 0) + (activeData.totalAskVol || 0);
    return total === 0 ? 50 : Math.round((activeData.totalBidVol / total) * 100);
  }, [activeData]);

  const askPercentage = 100 - bidPercentage;

  return (
    <div className="min-h-screen bg-[#0a0c10] text-white p-4 md:p-8 selection:bg-blue-500/30 font-public-sans overflow-x-hidden">
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Public+Sans:ital,wght@0,100..900;1,100..900&display=swap');
        .font-public-sans { font-family: 'Public Sans', sans-serif; }
        .custom-scrollbar::-webkit-scrollbar { height: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: rgba(255,255,255,0.02); }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.2); border-radius: 10px; }
      `}</style>
      
      <div className="max-w-[1400px] mx-auto space-y-8">
        
        {/* RESTRUCTURED HEADER: DATE AT TOP, ANALYSIS UNDERNEATH */}
        <div className="flex flex-col items-start border-b border-white/5 pb-6 gap-6">
          <div className="flex justify-between items-start w-full">
            <div className="flex gap-1 p-1 bg-white/5 rounded-lg border border-white/5 shadow-inner">
                {Object.keys(data).map(date => (
                  <button
                    key={date}
                    onClick={() => setActiveDate(date)}
                    className={`px-3 py-1.5 rounded text-xs font-bold transition-all ${
                      activeDate === date ? 'bg-white/10 text-white shadow-md' : 'text-white/60 hover:text-white'
                    }`}
                  >
                    {date}
                  </button>
                ))}
            </div>

            <div className="bg-[#161b22] px-4 py-2 rounded-lg border border-white/5 shadow-xl">
                <span className="text-[10px] block text-right font-black text-white uppercase mb-0.5 tracking-widest">Session Delta</span>
                <span className={`text-xl font-mono font-black ${activeData.netDelta < 0 ? 'text-rose-500' : 'text-emerald-500'}`}>
                  {activeData.netDelta > 0 ? '+' : ''}{formatNum(activeData.netDelta)}
                </span>
            </div>
          </div>

          <div className="w-full">
            <div className="flex items-center gap-3 mb-1">
               <Fingerprint className={activeData.sentiment === 'Bearish' ? 'text-rose-500' : 'text-emerald-500'} size={28} />
               <h1 className="text-3xl font-black tracking-tight uppercase leading-none text-white">Tape Analysis</h1>
            </div>
            <p className="text-[10px] font-bold text-white uppercase tracking-[0.2em] flex items-center gap-4">
              Range: {String(activeData.range)} • <span className={activeData.sentiment === 'Bearish' ? 'text-rose-400' : 'text-emerald-400'}>Session: {String(activeData.sessionName)}</span>
            </p>
          </div>
        </div>

        {/* METRICS GRID */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          {[
            { label: 'Net Delta', value: formatNum(activeData.netDelta), icon: Activity, color: activeData.netDelta < 0 ? 'text-rose-500' : 'text-emerald-500' },
            { label: 'Whale Prints', value: formatNum(activeData.whalePrints), sub: 'High Volume Blocks', icon: AlertTriangle, color: 'text-amber-400' },
            { label: 'Pressure Index', value: String(activeData.sellingPressure), sub: 'Aggressor Side', icon: Clock, color: activeData.sentiment === 'Bearish' ? 'text-rose-500' : 'text-emerald-500' },
          ].map((m, i) => (
            <div key={i} className="bg-[#161b22] p-5 rounded-2xl border border-white/5 shadow-2xl hover:border-white/10 transition-all">
                <m.icon className={`mb-4 ${m.color || 'text-white'}`} size={20} />
                <div className="text-[10px] text-white uppercase font-black tracking-widest mb-1 opacity-80">{m.label}</div>
                <div className="text-2xl font-black text-white font-mono leading-none">{m.value}</div>
                {m.sub && <div className="text-[10px] text-white font-bold mt-2 uppercase tracking-widest opacity-60">{m.sub}</div>}
            </div>
          ))}
        </div>

        {/* Narrative Description */}
        <div className="bg-[#161b22] border-l-4 border-emerald-500 p-5 rounded-r-2xl bg-gradient-to-r from-emerald-500/5 to-transparent">
          <p className="text-sm text-white leading-relaxed font-medium">
            <span className="text-emerald-400 font-bold uppercase mr-2 tracking-tighter">{(activeData.largestAsk || "").split(' @ ')[0]} – impact print node</span>
            {String(activeData.description)}
          </p>
        </div>

        {/* INSTITUTIONAL FOOTPRINT TIMELINE */}
        <div className="space-y-4 pt-4">
          <div className="flex items-center gap-2 px-2">
             <History className="text-blue-400" size={18} />
             <h3 className="text-xs font-black uppercase text-white tracking-[0.3em]">Institutional Footprint Timeline</h3>
          </div>
          
          <div className="relative overflow-x-auto pb-6 custom-scrollbar">
            <div className="flex gap-4 min-w-max px-2">
              {(activeData.timelineData || []).map((item, i) => (
                <div key={i} className="flex-shrink-0 w-64">
                   <div className={`h-1 w-full mb-4 ${item.side === 'bid' ? 'bg-rose-500/30' : 'bg-emerald-500/30'} relative`}>
                      <div className={`absolute top-1/2 left-0 -translate-y-1/2 w-3 h-3 rounded-full border-2 border-[#0a0c10] ${item.vol >= 1000 ? 'bg-amber-500 scale-125' : item.side === 'bid' ? 'bg-rose-500' : 'bg-emerald-500'}`} />
                   </div>
                   <div className="bg-[#161b22] p-4 rounded-xl border border-white/5 shadow-lg group hover:border-blue-500/40 transition-all">
                      <div className="flex justify-between items-start mb-2">
                         <span className="text-[10px] font-black text-white opacity-60 font-mono uppercase">{String(item.time)}</span>
                         <span className={`text-[9px] px-1.5 py-0.5 rounded font-black border ${item.side === 'bid' ? 'border-rose-900 text-rose-400 bg-rose-900/10' : 'border-emerald-900 text-emerald-400 bg-emerald-900/10'}`}>
                           {item.side.toUpperCase()}
                         </span>
                      </div>
                      <div className="text-xl font-mono font-black text-white mb-1">@{String(item.price)}</div>
                      <div className="text-[10px] font-black text-white uppercase mb-2 opacity-90">Size: {formatNum(item.vol)} lots</div>
                      <p className="text-[11px] text-white opacity-70 italic font-medium border-t border-white/5 pt-2">
                        {String(item.label)}
                      </p>
                   </div>
                </div>
              ))}
            </div>
          </div>
        </div>

        {/* CLUSTER HEATMAP */}
        <div className="space-y-4">
          <div className="flex justify-between items-center px-2">
            <h3 className="text-xs font-black uppercase text-white tracking-[0.3em]">Institutional Cluster Heatmap</h3>
            <div className="flex gap-6 text-[10px] font-bold text-white uppercase tracking-widest">
              <span className="flex items-center gap-2 opacity-80"><Waves size={12} className="text-blue-400" /> High Integrity</span>
              <span className="flex items-center gap-2 opacity-80"><Flame size={12} className="text-orange-400" /> Hot Zone</span>
            </div>
          </div>
          <div className="grid grid-cols-1 xl:grid-cols-3 gap-6">
            {(activeData.notableZones || []).map((zone, i) => (
              <div key={i} className="bg-[#161b22] border border-white/5 rounded-2xl p-6 shadow-2xl relative group overflow-hidden hover:bg-[#1c2128] transition-all">
                <div className={`absolute top-0 left-0 w-1 h-full ${zone.type === 'bid' ? 'bg-rose-500' : 'bg-emerald-500'}`} />
                <div className="flex justify-between items-start mb-2">
                  <h4 className={`text-2xl font-black tracking-tighter ${zone.type === 'bid' ? 'text-rose-400' : 'text-emerald-400'}`}>
                    {String(zone.level)}
                  </h4>
                  <div className="bg-white/10 px-2 py-1 rounded text-[10px] font-black text-white border border-white/10">
                    {formatNum(zone.strength)}% STR
                  </div>
                </div>
                <p className="text-[10px] text-white font-black mb-4 uppercase tracking-tighter flex items-center gap-2">
                  <Crosshair size={12} className="text-blue-500" /> {String(zone.status)}
                </p>
                <p className="text-xs text-white leading-relaxed mb-6 font-medium opacity-80">{String(zone.desc)}</p>
                <div className="space-y-4">
                  <div className="h-1.5 w-full bg-[#0a0c10] rounded-full overflow-hidden">
                    <div 
                      style={{ width: `${zone.strength}%` }} 
                      className={`h-full rounded-full ${zone.strength > 85 ? 'bg-blue-500 shadow-[0_0_10px_rgba(59,130,246,0.5)]' : 'bg-white opacity-40'}`}
                    />
                  </div>
                  <div className="flex flex-wrap gap-2">
                    {(zone.tags || []).map((tag, j) => (
                      <span key={j} className={`text-[9px] px-2 py-0.5 rounded font-black tracking-widest uppercase border ${
                        zone.type === 'bid' ? 'text-rose-400 border-rose-900 bg-rose-900/10' : 'text-emerald-400 border-emerald-900 bg-emerald-900/10'
                      }`}>
                        {String(tag)}
                      </span>
                    ))}
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Volume Flow Bar */}
        <div className="space-y-3 px-2">
          <div className="flex justify-between text-[10px] font-black uppercase text-white tracking-[0.2em] opacity-80">
            <span>Volume Flow – Bid vs Ask Intensity</span>
            <div className="flex gap-6">
              <span className="text-rose-500">{bidPercentage}% bid ({formatNum(activeData.totalBidVol)})</span>
              <span className="text-emerald-500">{askPercentage}% ask ({formatNum(activeData.totalAskVol)})</span>
            </div>
          </div>
          <div className="h-4 bg-[#161b22] rounded-full overflow-hidden flex shadow-inner border border-white/5">
            <div style={{ width: `${bidPercentage}%` }} className="h-full bg-gradient-to-r from-rose-600 to-rose-400 transition-all duration-700" />
            <div style={{ width: `${askPercentage}%` }} className="h-full bg-gradient-to-r from-emerald-400 to-emerald-600 transition-all duration-700" />
          </div>
        </div>

        {/* Tape Interpretation */}
        <div className="space-y-4">
           <h3 className="text-[10px] font-black uppercase text-white tracking-[0.3em] px-2">Tape Interpretation</h3>
           <div className="grid grid-cols-1 md:grid-cols-2 2xl:grid-cols-3 gap-6">
              {(activeData.interpretation || []).map((item, i) => (
                <div key={i} className="bg-[#161b22] p-5 rounded-2xl border border-white/5 relative overflow-hidden shadow-lg group hover:bg-[#1c2128] transition-all">
                   <div className={`absolute top-0 left-0 w-1 h-full ${
                     item.color === 'rose' ? 'bg-rose-500' : 
                     item.color === 'orange' ? 'bg-orange-500' : 
                     item.color === 'yellow' ? 'bg-yellow-500' : 'bg-blue-500'
                   }`} />
                   <div className="flex justify-between items-start mb-2">
                      <span className={`text-[9px] font-black uppercase tracking-[0.1em] ${
                        item.color === 'rose' ? 'text-rose-500' : 
                        item.color === 'orange' ? 'text-orange-500' : 
                        item.color === 'yellow' ? 'text-yellow-500' : 'text-blue-500'
                      }`}>{String(item.title)}</span>
                      <span className="text-[10px] font-mono text-white font-black tracking-tighter">{String(item.price)}</span>
                   </div>
                   <p className="text-xs text-white leading-relaxed font-medium opacity-80">{String(item.desc)}</p>
                </div>
              ))}
           </div>
        </div>

        {/* FORWARD GUIDANCE SECTION */}
        {activeData.nextSessionPrep && (
          <div className="space-y-6 pt-6">
            <div className="flex items-center gap-4 px-2">
               <Compass className="text-blue-400" size={24} />
               <h3 className="text-xl font-black uppercase tracking-tight text-white">Forward Guidance: Next Session Prep</h3>
               <div className="h-px flex-1 bg-white/5 ml-4" />
            </div>
            
            <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
               <div className="lg:col-span-7 bg-[#161b22] rounded-3xl p-8 border border-white/5 shadow-2xl">
                  <div className="flex items-center gap-3 mb-6">
                    <Target className="text-emerald-400" size={20} />
                    <h4 className="text-[10px] font-black uppercase tracking-[0.3em] text-white opacity-70">Institutional Execution Plan</h4>
                  </div>
                  
                  <div className="space-y-4">
                    {(activeData.nextSessionPrep.keyLevels || []).map((lvl, idx) => (
                      <div key={idx} className="flex flex-col md:flex-row md:items-center justify-between p-5 rounded-2xl bg-[#0a0c10] border border-white/5 hover:border-blue-500/30 transition-all group">
                         <div className="flex items-center gap-6 mb-4 md:mb-0">
                            <span className="text-3xl font-mono font-black text-white">{String(lvl.price)}</span>
                            <div>
                               <div className="text-[10px] font-black uppercase tracking-widest text-blue-400 mb-0.5">{String(lvl.label)}</div>
                               <div className="text-xs text-white font-medium italic opacity-60">Primary Objective</div>
                            </div>
                         </div>
                         <div className="flex items-center gap-3 bg-blue-500/5 px-5 py-2 rounded-xl border border-blue-500/10 group-hover:bg-blue-500/10 transition-colors">
                            <ArrowRightCircle size={16} className="text-blue-500" />
                            <span className="text-[10px] font-black text-white uppercase tracking-widest">{String(lvl.action)}</span>
                         </div>
                      </div>
                    ))}
                  </div>
               </div>

               <div className="lg:col-span-5 flex flex-col gap-6">
                  <div className="bg-[#161b22] p-8 rounded-3xl border border-white/5 shadow-2xl flex-1 relative overflow-hidden">
                     <div className={`absolute top-0 right-0 p-6 opacity-5`}>
                        <Shield size={160} />
                     </div>
                     <h4 className="text-[10px] font-black uppercase tracking-[0.3em] text-white mb-6 flex items-center gap-3 opacity-70">
                        <Info size={18} className="text-blue-400" /> Strategic Thesis
                     </h4>
                     <div className="inline-block px-4 py-1 rounded-full bg-blue-500/10 border border-blue-500/20 text-[9px] font-black text-blue-400 uppercase mb-6 tracking-[0.2em]">
                        Bias: {String(activeData.nextSessionPrep.bias)}
                     </div>
                     <p className="text-xl text-white font-medium leading-relaxed mb-8">
                        {String(activeData.nextSessionPrep.thesis)}
                     </p>
                     <div className="p-5 rounded-2xl bg-[#0a0c10] border border-white/5 text-[11px] text-white opacity-50 italic leading-relaxed">
                        "Look for the aggressive closing imbalance to either find immediate continuation or deep absorption at {activeData.nextSessionPrep.keyLevels[1].price}."
                     </div>
                  </div>
               </div>
            </div>
          </div>
        )}

        {/* Global Footer */}
        <div className="flex justify-center items-center py-10 border-t border-white/5 text-[9px] text-white font-black uppercase tracking-[0.5em] gap-3 opacity-60">
             <Fingerprint size={16} /> TAPE INTEL PRO • v2.9.2 • APRIL 30 WHALE SESSION
        </div>
      </div>
    </div>
  );
};

export default App;
