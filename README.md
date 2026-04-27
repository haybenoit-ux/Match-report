import React, { useState, useEffect, useRef, useCallback } from "react";
import {
  Trophy, Users, Calendar, MapPin, Camera, Sparkles, Download, Upload,
  Plus, X, Check, AlertCircle, ChevronLeft, ChevronRight, Search,
  BarChart3, FileText, Star, Award, Shield, Target, Edit3, Trash2,
  Languages, Loader2, Image as ImageIcon, Crop, ChevronDown, Save,
  Home, Plane, RefreshCw, TrendingUp, User, Settings, Eye
} from "lucide-react";

// =====================================================
// CONSTANTS
// =====================================================

const TEAMS = [
  "U6/Cubs", "U6/Advanced Cubs", "U8 A", "U8 B", "U8C",
  "U10A", "U10B", "U10C", "U12A", "U12B", "U12B+", "U12C",
  "U14A", "U14B", "U14B+", "U14C", "U16A", "U16B", "U21",
  "U8 Vixens", "U10 Vixens", "U12 Vixens", "U15 Vixens", "U19 Vixens",
  "Recreational"
];

const MATCH_TYPES = [
  "Friendly", "Saigon Youth League", "Tournament", "Internal Scrimmage", "Cup"
];

// Fox Football brand colors
const COLORS = {
  black: "#0A0A0A",
  red: "#D32027",
  orange: "#F57A1F",
  cream: "#FFF8F0",
  blue: "#2D4F8C",
  gold: "#F4C430"
};

// Fox Football official logo (embedded as base64 for portability)
const FOX_LOGO_B64 = "data:image/png;base64,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7LPPKioqZs2aNWrUqPnz5584ceIXRoSp+X/11VfT0tI2bdpEOzKokiOERFGkszUHDx684447/v73v3cNLejjikQi119//c6dO2+//fb33nuv6xTlRd7OyF7gn08AK/AWizXBZBkyYqBr824pEGRYhoo+/MUjmxAMGMJygGExwwDIAshBTssLWkGvl1mehwSy4U6mY79U/a1S9TVz6jOr4k3qc+W7H3y5Y8fW/ftKysvLaRZPlhXajky9BUVRZEWWJZkaM0VWZFmRZTnmn/ycMNEAcfDgwffee69Op/v444/feOONWJcldVcmTpz44osvvvrqq2VlZdXV1c3NzW63m/aKnjO3ReVPluVoNBoOh2PtN0uWLNm1a1dTU1NszQTDME1NTRMnToyLizt8+HB7e7vb7fb5fFVVVT169FixYoXP5zt16tQv5GcghHV1dePGjUtLSzt16lQkEqHjlPS4o30ZCKFrr7322LFjtbW1NG/btdDm9/s3bdp03333xcXFtbW10f6li3+anr3wZ5DAx8WZzSbToNHFoSOn/bVVvEb45ScGAQAEQgABJAzPMhxgWMgykOEwwxDIQMBCASo6NqLRAKzRsryBhQwbcUWJIZxz+cZvvyWKohUEQaOBkMGYIKRIsixJkiRJsiwrSFEQor0uZ3VAjo20/4Ippd759OnTr7766mg0umzZsubmZupgUFfBZDJ9+umnJSUlO3bsqKmpaW5u9vv9/3TIEGNM01m0EJaenj5ixIg1a9ZQfYv93MzMzJEjR+7bt8/hcHR2doqiqNFogsGgwWBYunTptm3b6uvruwpuV02jKvTll19OmzZt4MCBtbW1NGVE9wLRBBTP8/n5+T179vzkk0/OWZSCMeY4zuVy7d69+/e//72iKA6HgxaJL/IRygu874UQgkVJDAU73A67OxTXr7ciScyviMwhgZCwBEOsEKJAggDGhCgQI0gQhJJGIkIAsmHAAkXLKjJiZMQKWDBxrMnGQg5CTAgVbYSUWAWKEELwmZIT6gI9Fn5Z+sHZPacDBgwAAFRVVZWWlnZ1aRBC8+bN02g027dvb2tra29vp935vywf1H+gs4i0KXrHjh2DBg0aMmQIbfmMuWTff/+9xWKx2WzUOwoEAoFAIBqNxsfHf/nllyUlJbTw/HPvnOf5xsbGyy+/vL29fcWKFb169SooKMjIyDAa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TRvXXC6XKIq0Qy62443neaPRSGdokpKSEhISvF5v3759ExISnE4nzazTfOWzzz47derUYcOG+f1+OjhCG61/UgeogYjZ/tzcXLrDuaGh4fXXX6duD3WNZFkeO3bs0KFDm5ub8/Pzp02bRjf5ULWkge999903efLkVatW1dfXNzY2+nw+6ojTaD4cDgeDQVEUGxsbp02bVlJS8sADD3zwwQcAAEEQYsINIVy5cmV5efkf//hHt9ut1+vnz5//4IMP/nJuhxYBS0pK5s6d+/7779M1GbRN6IKv2r3AtQmGYfR6g81mHTps6C23TOvep09KapopzmSJrU0zxek0OoHnMQMkJEcjITEUCIki4WBEwn0uH0UiXl/JIUGnIwxmOMBwkGEZliUMB1mOMBxhWcCykOMBC2VgsEXyr9mzt0yiPZ7yD80+XTt/kKLIiowJdrtcDz38sMls/nrjV3V1dbHWHboJ1GQy0VlhehtAVlZWYWHh0KFDR4wYkZiYmJ2dffr06fLy8ljLGl2p4Pf7V6xYUVtbSx2Arh2gP3449FLNxMTEvLy8nJyc66+/vm/fvtdff73L5YoZXfrF008/3aNHD7PZ/Nlnnz322GOxshdVjylTpjz33HN/+9vfjh49SvdW0FVFsZVykiSFw+FwOByNRCsqK2RJuvfee3v36rV7924qrxSrxdq3X1+ASf/+A/r269fU1NS9e/fVq1dHIpFf0AGqyYIgnDx50uVy3XfffY2NjfTiGVoPuYAKcIFdIAAAywCdQZdgTph20xRfRRXU60M63u7ztbd2NNY3NTW3+jrsAZfbK4UDkZAohiQxComAoowrGCo/fOzyaya1vLeeYMQqBCFGUBiCCQ0DEKIW/8yBwPKQARAQgjD1flDM18eERgKEVgToryQUCObl5V83ZcqXGzdGolE69m4ymWKrPI1Go81mS0pKysvLy83NTU1NNRqNdBd5QUGB2+1evHjxmjVrYvkQ6gy88cYbhYWFy5Yte+211+i8fGdnJ22HjrlSdBiASj/d4Jmenn7zzTf36dNnypQp1dXVgiDQV9L+6tGjR994443UkN93332xiUr6t7169Xr77bc/+eSTsrKyxsbG2FpPQgALIGIAIATIsiiGnS4SxXIoGg5GwlX1dbNmzty1e9fjjzzGQb5X397de3fv3rtXpjUlgMSG6qovP9sgStLE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B923MLt4uLtkJy9oIzp9wQ2T5d49RUF4cMXKHVu3bt26VaPRRBXlrrvu0ul0+/fvp/f+nlOIIQDIshyMiv6Ar/ZEdc+br2UI11Td3NTacPDgwePHj9fU1NTW1na9L4NKJF1q+xOLbyFUFEUQhHXr1k2fPv3jjz/2+XwtLS1UgqnoU8udlJRUWlq6du3a5ubmrooBzt5OEMt+0nTNv72F4TxAG3527tyJELrssssaGxsHDBjw7LPPUvN/AS+Oh+DSgmUYjV6Xl5XVp1vv6YvmDAH81zPnJMXF2+ZNf+3Ivk07SsIeX1ZO9pTrr+/s7Fy9erXFYvn+++9LSko+++yzioqK9vZ2uuSwK1qNJjMtI6d7/qP/+/tT9fV/euTxTndnyBf4/3+3Vqu1pqbGbrf36NED/Hdwyy23TJ48WZKkoqKi559/vrKysrGxMRQKXSgFuKROAOpjyNGo3eNs9jn3b9kzYsWChGFD+LSUB7d8tWf7XgA4loFVNTXPPPPMst/9rqCgYNy4cTqdLpb8iUajP2m9omLE7fceP3388tHjX/7rs3ychmO1HMPG9pV2LSF1TSb+wlsFANC9f2lpaVu3bqV593NeE1sTffH4M//2ryb2TEaMGOFyuf7yl7/Q1v+uEZd6AvwHYBioNejy03MLe/eaMfvWKzJz33rvgxV/fVbgeQUhevU0xjgpKWnu3LlTp049cuTIhg0bTp061dHR8WPzT/2NOKMxNS0tLy93ePHwzMxMjuPAf2JVE8MwNOQwGAwX8K7c8+ymulyu+vr68vLypqYmWnG/gOrNXYqPmMiRiNfldrjd2/fvn3jllZ0AMRDGRJYmJTo7O4cNG2a1Wvft20fXvP2cKcIYh0XR4XAghDo6OnU6nUaj+Y8YRaqNF0mu5jwd0RhHo9FQKERb3y7U7aiXsgIAABRC/GLQZ3e11rUePVFhTU7GhEDInJOUsNls+/bto3noXxjCoGVLv99Pb1mkAS7DMEDlX8/U0WSdJEl0wd6PJwRUBfhPPGhEwlLE53UG2ztPHjg86cqJ//vg/8qyRC/8oleIDho0KD8//7PPPnO73X6//5d/E3RqXlEUenXAxZxv+a2oAbiYErWX1jlLL37iGJPNNLB/3xkzppefOPH3v/+9a8NMZmbmsWPHNmzYMHny5MLCQoPBoD63/07YS1UHqJEhAEKGaWlpuW7KdXPmzDGZTBkZGbNnz161alVjY+NHH33U3NxMr+JRrdF/rbm8NKMtQAjLsDq9LikxKTMrMzMzc8yYMQMGDDAYDA6Ho6Sk5NChQ62trc3NzTQTpyqAqgCX4gHHsjqdzmq1JiYmGgwGg8EgCIIoiuFw2Ofz0QtI6AayrolqFVUBLh0YhhEEQavVGgwGjUZDJ9/prGMkEjkzEaaiKsAl6gr90GATu42dSjxCSLX3KuCSVwA1ZamioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKicvHw/wAcJJoH+X3bSAAAAABJRU5ErkJggg==";

// =====================================================
// STORAGE HELPERS
// =====================================================

const storage = {
  async get(key) {
    try {
      const result = await window.storage.get(key);
      return result ? JSON.parse(result.value) : null;
    } catch {
      return null;
    }
  },
  async set(key, value) {
    try {
      await window.storage.set(key, JSON.stringify(value));
      return true;
    } catch (e) {
      console.error("Storage set error:", e);
      return false;
    }
  },
  async delete(key) {
    try {
      await window.storage.delete(key);
      return true;
    } catch {
      return false;
    }
  },
  async list(prefix) {
    try {
      const result = await window.storage.list(prefix);
      return result?.keys || [];
    } catch {
      return [];
    }
  }
};

// =====================================================
// SEASON HELPERS (Vietnamese school year: Aug to Jun)
// =====================================================

function getCurrentSeason(date = new Date()) {
  const year = date.getFullYear();
  const month = date.getMonth(); // 0-11
  // School year starts in August (month 7)
  if (month >= 7) {
    return `${year}-${year + 1}`;
  } else {
    return `${year - 1}-${year}`;
  }
}

function getSeasonLabel(seasonKey) {
  return `Season ${seasonKey}`;
}

// =====================================================
// CLAUDE API HELPER
// =====================================================

async function callClaude(prompt, systemPrompt = "") {
  try {
    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 1500,
        system: systemPrompt || undefined,
        messages: [{ role: "user", content: prompt }]
      })
    });
    const data = await response.json();
    const textBlocks = data.content?.filter(b => b.type === "text") || [];
    return textBlocks.map(b => b.text).join("\n").trim();
  } catch (e) {
    console.error("Claude API error:", e);
    throw e;
  }
}

// =====================================================
// REUSABLE UI COMPONENTS
// =====================================================

function Button({ children, onClick, variant = "primary", size = "md", disabled, className = "", type = "button", icon: Icon }) {
  const base = "inline-flex items-center justify-center gap-2 font-bold uppercase tracking-wider transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed rounded-md";
  const variants = {
    primary: "bg-gradient-to-r from-red-600 to-orange-500 text-white hover:shadow-lg hover:shadow-orange-500/30 hover:scale-[1.02] active:scale-[0.98]",
    secondary: "bg-neutral-900 text-white hover:bg-neutral-800 border border-neutral-700",
    ghost: "bg-transparent text-neutral-700 hover:bg-neutral-100",
    danger: "bg-red-600 text-white hover:bg-red-700",
    outline: "bg-white border-2 border-neutral-900 text-neutral-900 hover:bg-neutral-900 hover:text-white"
  };
  const sizes = {
    sm: "px-3 py-1.5 text-xs",
    md: "px-5 py-2.5 text-sm",
    lg: "px-7 py-3.5 text-base"
  };
  return (
    <button type={type} onClick={onClick} disabled={disabled} className={`${base} ${variants[variant]} ${sizes[size]} ${className}`}>
      {Icon && <Icon className="w-4 h-4" />}
      {children}
    </button>
  );
}

function Input({ label, value, onChange, placeholder, type = "text", required, error, icon: Icon }) {
  return (
    <div className="space-y-1.5">
      {label && (
        <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
          {label} {required && <span className="text-red-600">*</span>}
        </label>
      )}
      <div className="relative">
        {Icon && <Icon className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-neutral-400" />}
        <input
          type={type}
          value={value || ""}
          onChange={(e) => onChange(e.target.value)}
          placeholder={placeholder}
          className={`w-full ${Icon ? "pl-10" : "pl-4"} pr-4 py-2.5 bg-white border-2 ${error ? "border-red-500" : "border-neutral-200"} rounded-md focus:border-orange-500 focus:outline-none transition-colors text-neutral-900 placeholder:text-neutral-400`}
        />
      </div>
      {error && <p className="text-xs text-red-600 font-medium">{error}</p>}
    </div>
  );
}

function Select({ label, value, onChange, options, placeholder = "Select...", required }) {
  return (
    <div className="space-y-1.5">
      {label && (
        <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
          {label} {required && <span className="text-red-600">*</span>}
        </label>
      )}
      <div className="relative">
        <select
          value={value || ""}
          onChange={(e) => onChange(e.target.value)}
          className="w-full appearance-none px-4 py-2.5 pr-10 bg-white border-2 border-neutral-200 rounded-md focus:border-orange-500 focus:outline-none transition-colors text-neutral-900 cursor-pointer"
        >
          <option value="">{placeholder}</option>
          {options.map(opt => (
            <option key={opt} value={opt}>{opt}</option>
          ))}
        </select>
        <ChevronDown className="absolute right-3 top-1/2 -translate-y-1/2 w-4 h-4 text-neutral-400 pointer-events-none" />
      </div>
    </div>
  );
}

function Textarea({ label, value, onChange, placeholder, rows = 4, required, error }) {
  return (
    <div className="space-y-1.5">
      {label && (
        <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
          {label} {required && <span className="text-red-600">*</span>}
        </label>
      )}
      <textarea
        value={value || ""}
        onChange={(e) => onChange(e.target.value)}
        placeholder={placeholder}
        rows={rows}
        className={`w-full px-4 py-3 bg-white border-2 ${error ? "border-red-500" : "border-neutral-200"} rounded-md focus:border-orange-500 focus:outline-none transition-colors text-neutral-900 placeholder:text-neutral-400 resize-y`}
      />
      {error && <p className="text-xs text-red-600 font-medium">{error}</p>}
    </div>
  );
}

// Autocomplete input that learns from previous entries
function AutocompleteInput({ label, value, onChange, suggestions = [], placeholder, required, onAddNew }) {
  const [showSuggestions, setShowSuggestions] = useState(false);
  const [filtered, setFiltered] = useState([]);
  const wrapperRef = useRef(null);

  useEffect(() => {
    if (value && value.length > 0) {
      const f = suggestions.filter(s =>
        s.toLowerCase().includes(value.toLowerCase()) && s.toLowerCase() !== value.toLowerCase()
      );
      setFiltered(f.slice(0, 6));
    } else {
      setFiltered(suggestions.slice(0, 6));
    }
  }, [value, suggestions]);

  useEffect(() => {
    const handleClick = (e) => {
      if (wrapperRef.current && !wrapperRef.current.contains(e.target)) {
        setShowSuggestions(false);
      }
    };
    document.addEventListener("mousedown", handleClick);
    return () => document.removeEventListener("mousedown", handleClick);
  }, []);

  return (
    <div className="space-y-1.5" ref={wrapperRef}>
      {label && (
        <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
          {label} {required && <span className="text-red-600">*</span>}
        </label>
      )}
      <div className="relative">
        <input
          type="text"
          value={value || ""}
          onChange={(e) => onChange(e.target.value)}
          onFocus={() => setShowSuggestions(true)}
          placeholder={placeholder}
          className="w-full px-4 py-2.5 bg-white border-2 border-neutral-200 rounded-md focus:border-orange-500 focus:outline-none transition-colors text-neutral-900 placeholder:text-neutral-400"
        />
        {showSuggestions && filtered.length > 0 && (
          <div className="absolute z-20 w-full mt-1 bg-white border-2 border-neutral-200 rounded-md shadow-lg max-h-56 overflow-y-auto">
            {filtered.map((s, i) => (
              <button
                key={i}
                type="button"
                onClick={() => {
                  onChange(s);
                  setShowSuggestions(false);
                }}
                className="w-full text-left px-4 py-2.5 hover:bg-orange-50 text-neutral-900 text-sm border-b border-neutral-100 last:border-b-0 transition-colors"
              >
                {s}
              </button>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}

function Badge({ children, color = "neutral", className = "" }) {
  const colors = {
    neutral: "bg-neutral-100 text-neutral-800 border-neutral-200",
    green: "bg-green-100 text-green-800 border-green-300",
    red: "bg-red-100 text-red-800 border-red-300",
    yellow: "bg-yellow-100 text-yellow-800 border-yellow-300",
    blue: "bg-blue-100 text-blue-800 border-blue-300",
    orange: "bg-orange-100 text-orange-800 border-orange-300"
  };
  return (
    <span className={`inline-flex items-center gap-1 px-2.5 py-1 text-xs font-bold uppercase tracking-wider rounded border ${colors[color]} ${className}`}>
      {children}
    </span>
  );
}

function Toast({ message, type = "success", onClose }) {
  useEffect(() => {
    const t = setTimeout(onClose, 3000);
    return () => clearTimeout(t);
  }, [onClose]);

  const colors = {
    success: "bg-green-600",
    error: "bg-red-600",
    info: "bg-neutral-900"
  };

  return (
    <div className={`fixed top-6 right-6 z-50 ${colors[type]} text-white px-5 py-3 rounded-md shadow-2xl flex items-center gap-3 animate-slide-in font-medium`}>
      {type === "success" && <Check className="w-5 h-5" />}
      {type === "error" && <AlertCircle className="w-5 h-5" />}
      <span className="text-sm">{message}</span>
    </div>
  );
}

function Modal({ isOpen, onClose, title, children, maxWidth = "max-w-lg" }) {
  if (!isOpen) return null;
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/60 backdrop-blur-sm" onClick={onClose}>
      <div className={`bg-white rounded-lg shadow-2xl w-full ${maxWidth} max-h-[90vh] overflow-hidden flex flex-col`} onClick={e => e.stopPropagation()}>
        <div className="flex items-center justify-between px-6 py-4 border-b border-neutral-200">
          <h3 className="text-lg font-black uppercase tracking-wider text-neutral-900">{title}</h3>
          <button onClick={onClose} className="text-neutral-400 hover:text-neutral-900 transition-colors">
            <X className="w-5 h-5" />
          </button>
        </div>
        <div className="overflow-y-auto p-6">{children}</div>
      </div>
    </div>
  );
}

// =====================================================
// MAIN APP COMPONENT
// =====================================================

export default function FoxFootballMatchReport() {
  const [view, setView] = useState("loading"); // loading, coach-select, dashboard, form, preview
  const [currentCoach, setCurrentCoach] = useState(null);
  const [coaches, setCoaches] = useState([]);
  const [opponents, setOpponents] = useState([]);
  const [venues, setVenues] = useState([]);
  const [playersByTeam, setPlayersByTeam] = useState({});
  const [matches, setMatches] = useState([]);
  const [editingMatch, setEditingMatch] = useState(null);
  const [toast, setToast] = useState(null);

  const showToast = useCallback((message, type = "success") => {
    setToast({ message, type, id: Date.now() });
  }, []);

  // Load all data on mount
  useEffect(() => {
    (async () => {
      const c = await storage.get("ffv:coaches") || [];
      const o = await storage.get("ffv:opponents") || [];
      const v = await storage.get("ffv:venues") || [];
      const p = await storage.get("ffv:playersByTeam") || {};
      const m = await storage.get("ffv:matches") || [];
      const cc = await storage.get("ffv:currentCoach");
      setCoaches(c);
      setOpponents(o);
      setVenues(v);
      setPlayersByTeam(p);
      setMatches(m);
      if (cc && c.includes(cc)) {
        setCurrentCoach(cc);
        setView("dashboard");
      } else {
        setView("coach-select");
      }
    })();
  }, []);

  // Save coach selection
  const selectCoach = async (name) => {
    const trimmed = name.trim();
    if (!trimmed) return;
    let updated = coaches;
    if (!coaches.includes(trimmed)) {
      updated = [...coaches, trimmed].sort();
      setCoaches(updated);
      await storage.set("ffv:coaches", updated);
    }
    setCurrentCoach(trimmed);
    await storage.set("ffv:currentCoach", trimmed);
    setView("dashboard");
  };

  const switchCoach = async () => {
    await storage.delete("ffv:currentCoach");
    setCurrentCoach(null);
    setView("coach-select");
  };

  // Save match (and update related learned data)
  const saveMatch = async (matchData) => {
    const isEdit = !!matchData.id;
    let updatedMatches;
    if (isEdit) {
      updatedMatches = matches.map(m => m.id === matchData.id ? matchData : m);
    } else {
      const newMatch = { ...matchData, id: `match-${Date.now()}`, createdAt: new Date().toISOString() };
      updatedMatches = [...matches, newMatch];
    }
    setMatches(updatedMatches);
    await storage.set("ffv:matches", updatedMatches);

    // Learn opponents
    if (matchData.opponent && !opponents.includes(matchData.opponent)) {
      const upd = [...opponents, matchData.opponent].sort();
      setOpponents(upd);
      await storage.set("ffv:opponents", upd);
    }
    // Learn venues
    if (matchData.venue && !venues.includes(matchData.venue)) {
      const upd = [...venues, matchData.venue].sort();
      setVenues(upd);
      await storage.set("ffv:venues", upd);
    }
    // Learn players per team
    const team = matchData.team;
    if (team) {
      const teamPlayers = new Set(playersByTeam[team] || []);
      [...(matchData.goalscorers || []), ...(matchData.assists || []), ...(matchData.mvps || []),
       ...(matchData.specialMentions || []), ...(matchData.goalkeepers || [])].forEach(item => {
        const name = typeof item === "string" ? item : item.name;
        if (name && name.trim()) teamPlayers.add(name.trim());
      });
      if (matchData.captain) teamPlayers.add(matchData.captain);
      const updatedPlayers = { ...playersByTeam, [team]: Array.from(teamPlayers).sort() };
      setPlayersByTeam(updatedPlayers);
      await storage.set("ffv:playersByTeam", updatedPlayers);
    }

    showToast(isEdit ? "Match report updated!" : "Match report saved!");
    setEditingMatch(null);
    setView("dashboard");
  };

  const deleteMatch = async (id) => {
    const updated = matches.filter(m => m.id !== id);
    setMatches(updated);
    await storage.set("ffv:matches", updated);
    showToast("Match deleted");
  };

  // Import bulk data from JSON file
  const importData = async (data) => {
    if (!data || typeof data !== "object") {
      showToast("Invalid file format", "error");
      return { ok: false };
    }
    try {
      let imported = 0;
      // Merge matches (avoid duplicates by id)
      if (Array.isArray(data.matches)) {
        const existingIds = new Set(matches.map(m => m.id));
        const newMatches = data.matches.filter(m => !existingIds.has(m.id));
        const merged = [...matches, ...newMatches];
        setMatches(merged);
        await storage.set("ffv:matches", merged);
        imported = newMatches.length;
      }
      // Merge coaches
      if (Array.isArray(data.coaches)) {
        const merged = Array.from(new Set([...coaches, ...data.coaches])).sort();
        setCoaches(merged);
        await storage.set("ffv:coaches", merged);
      }
      // Merge opponents
      if (Array.isArray(data.opponents)) {
        const merged = Array.from(new Set([...opponents, ...data.opponents])).sort();
        setOpponents(merged);
        await storage.set("ffv:opponents", merged);
      }
      // Merge venues
      if (Array.isArray(data.venues)) {
        const merged = Array.from(new Set([...venues, ...data.venues])).sort();
        setVenues(merged);
        await storage.set("ffv:venues", merged);
      }
      // Merge playersByTeam
      if (data.playersByTeam && typeof data.playersByTeam === "object") {
        const merged = { ...playersByTeam };
        for (const [team, players] of Object.entries(data.playersByTeam)) {
          if (Array.isArray(players)) {
            const existing = new Set(merged[team] || []);
            players.forEach(p => existing.add(p));
            merged[team] = Array.from(existing).sort();
          }
        }
        setPlayersByTeam(merged);
        await storage.set("ffv:playersByTeam", merged);
      }
      showToast(`Imported ${imported} new match${imported !== 1 ? "es" : ""}`);
      return { ok: true, imported };
    } catch (e) {
      console.error("Import error:", e);
      showToast("Import failed", "error");
      return { ok: false };
    }
  };

  // Reset all data (with confirmation)
  const resetAllData = async () => {
    setMatches([]);
    setCoaches([]);
    setOpponents([]);
    setVenues([]);
    setPlayersByTeam({});
    await storage.delete("ffv:matches");
    await storage.delete("ffv:coaches");
    await storage.delete("ffv:opponents");
    await storage.delete("ffv:venues");
    await storage.delete("ffv:playersByTeam");
    showToast("All data cleared");
  };

  // Get next match number for a team in current season
  const getNextMatchNumber = (team, date) => {
    const season = getCurrentSeason(new Date(date));
    const teamMatches = matches.filter(m =>
      m.team === team && getCurrentSeason(new Date(m.date)) === season
    );
    return teamMatches.length + 1;
  };

  if (view === "loading") {
    return (
      <div className="fixed inset-0 bg-neutral-950 flex items-center justify-center">
        <div className="text-center">
          <Loader2 className="w-10 h-10 text-orange-500 animate-spin mx-auto mb-4" />
          <p className="text-neutral-400 text-sm uppercase tracking-widest font-bold">Loading Fox Football</p>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-neutral-50 font-sans">
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;500;600;700;800;900&family=Playfair+Display:ital,wght@1,400;1,500;1,600&family=Be+Vietnam+Pro:ital,wght@0,400;0,500;0,600;0,700;0,800;0,900;1,400;1,500;1,600&family=Oswald:wght@400;500;600;700&display=swap');

        body, .font-sans { font-family: 'Inter', system-ui, sans-serif; }
        .font-display { font-family: 'Bebas Neue', sans-serif; letter-spacing: 0.02em; }
        .font-serif-italic { font-family: 'Playfair Display', serif; font-style: italic; }

        /* Vietnamese-optimized fonts (full diacritics support) */
        .lang-vi.font-sans, .lang-vi { font-family: 'Be Vietnam Pro', system-ui, sans-serif; }
        .lang-vi .font-display { font-family: 'Oswald', sans-serif; letter-spacing: 0.02em; font-weight: 700; }
        .lang-vi .font-serif-italic { font-family: 'Be Vietnam Pro', serif; font-style: italic; font-weight: 500; }

        @keyframes slide-in {
          from { transform: translateX(20px); opacity: 0; }
          to { transform: translateX(0); opacity: 1; }
        }
        .animate-slide-in { animation: slide-in 0.3s ease-out; }

        @keyframes fade-in {
          from { opacity: 0; transform: translateY(8px); }
          to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in { animation: fade-in 0.4s ease-out; }

        .scrollbar-thin::-webkit-scrollbar { width: 6px; }
        .scrollbar-thin::-webkit-scrollbar-track { background: transparent; }
        .scrollbar-thin::-webkit-scrollbar-thumb { background: #d4d4d4; border-radius: 3px; }
      `}</style>

      {toast && <Toast key={toast.id} message={toast.message} type={toast.type} onClose={() => setToast(null)} />}

      {view === "coach-select" && (
        <CoachSelectView coaches={coaches} onSelect={selectCoach} />
      )}

      {view === "dashboard" && (
        <DashboardView
          currentCoach={currentCoach}
          matches={matches}
          onSwitchCoach={switchCoach}
          onNewMatch={() => { setEditingMatch(null); setView("form"); }}
          onEditMatch={(m) => { setEditingMatch(m); setView("form"); }}
          onDeleteMatch={deleteMatch}
          onPreviewMatch={(m) => { setEditingMatch(m); setView("preview"); }}
          onImportData={importData}
          onResetData={resetAllData}
        />
      )}

      {view === "form" && (
        <MatchFormView
          currentCoach={currentCoach}
          editingMatch={editingMatch}
          coaches={coaches}
          opponents={opponents}
          venues={venues}
          playersByTeam={playersByTeam}
          getNextMatchNumber={getNextMatchNumber}
          onSave={saveMatch}
          onCancel={() => { setEditingMatch(null); setView("dashboard"); }}
          showToast={showToast}
        />
      )}

      {view === "preview" && editingMatch && (
        <PreviewView
          match={editingMatch}
          onBack={() => setView("dashboard")}
          showToast={showToast}
        />
      )}
    </div>
  );
}

// =====================================================
// COACH SELECTION VIEW
// =====================================================

function CoachSelectView({ coaches, onSelect }) {
  const [newName, setNewName] = useState("");
  const [search, setSearch] = useState("");

  const filtered = coaches.filter(c => c.toLowerCase().includes(search.toLowerCase()));

  return (
    <div className="min-h-screen relative overflow-hidden bg-neutral-950">
      {/* Atmospheric background */}
      <div className="absolute inset-0 opacity-30">
        <div className="absolute top-0 left-0 w-[600px] h-[600px] bg-red-600 rounded-full blur-[180px] -translate-x-1/3 -translate-y-1/3" />
        <div className="absolute bottom-0 right-0 w-[700px] h-[700px] bg-orange-500 rounded-full blur-[200px] translate-x-1/3 translate-y-1/3" />
      </div>

      {/* Grain texture overlay */}
      <div className="absolute inset-0 opacity-[0.04] pointer-events-none"
           style={{
             backgroundImage: `url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E")`
           }} />

      <div className="relative z-10 min-h-screen flex flex-col items-center justify-center p-6">
        <div className="w-full max-w-md animate-fade-in">
          {/* Logo */}
          <div className="text-center mb-12">
            <div className="inline-block relative mb-6">
              <div className="absolute -inset-4 bg-gradient-to-r from-red-600 to-orange-500 rounded-full blur-2xl opacity-50" />
              <div className="relative w-24 h-24 mx-auto bg-neutral-950 rounded-full border-2 border-orange-500/30 flex items-center justify-center">
                <FoxLogoMark className="w-14 h-14" />
              </div>
            </div>
            <h1 className="font-display text-5xl text-white tracking-tight leading-none mb-2">
              FOX FOOTBALL
            </h1>
            <div className="flex items-center justify-center gap-3 mb-1">
              <div className="h-px w-12 bg-orange-500/50" />
              <span className="font-display text-orange-500 text-lg tracking-[0.3em]">VIETNAM</span>
              <div className="h-px w-12 bg-orange-500/50" />
            </div>
            <p className="text-neutral-400 text-xs uppercase tracking-[0.25em] mt-4">Match Report Generator</p>
          </div>

          {/* Coach selection card */}
          <div className="bg-white/5 backdrop-blur-xl border border-white/10 rounded-2xl p-7 shadow-2xl">
            <h2 className="text-white text-sm font-bold uppercase tracking-widest mb-1">
              Welcome, Coach
            </h2>
            <p className="text-neutral-400 text-xs mb-5">Select your name or add yourself to the roster</p>

            {coaches.length > 0 && (
              <>
                <div className="relative mb-3">
                  <Search className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-neutral-500" />
                  <input
                    type="text"
                    value={search}
                    onChange={e => setSearch(e.target.value)}
                    placeholder="Search coaches..."
                    className="w-full pl-10 pr-4 py-2.5 bg-white/5 border border-white/10 rounded-lg text-white placeholder:text-neutral-500 focus:border-orange-500/60 focus:outline-none text-sm"
                  />
                </div>

                <div className="max-h-56 overflow-y-auto scrollbar-thin space-y-1.5 mb-5">
                  {filtered.map(coach => (
                    <button
                      key={coach}
                      onClick={() => onSelect(coach)}
                      className="w-full text-left px-4 py-3 bg-white/5 hover:bg-gradient-to-r hover:from-red-600/20 hover:to-orange-500/20 border border-white/10 hover:border-orange-500/40 rounded-lg transition-all duration-200 group"
                    >
                      <div className="flex items-center gap-3">
                        <div className="w-9 h-9 rounded-full bg-gradient-to-br from-red-600 to-orange-500 flex items-center justify-center text-white font-bold text-sm">
                          {coach.charAt(0).toUpperCase()}
                        </div>
                        <span className="text-white font-medium text-sm">{coach}</span>
                        <ChevronRight className="ml-auto w-4 h-4 text-neutral-500 group-hover:text-orange-400 group-hover:translate-x-0.5 transition-all" />
                      </div>
                    </button>
                  ))}
                  {filtered.length === 0 && search && (
                    <p className="text-neutral-500 text-xs text-center py-4">No coaches found</p>
                  )}
                </div>

                <div className="flex items-center gap-3 my-5">
                  <div className="flex-1 h-px bg-white/10" />
                  <span className="text-neutral-500 text-xs uppercase tracking-widest">Or</span>
                  <div className="flex-1 h-px bg-white/10" />
                </div>
              </>
            )}

            <div className="space-y-3">
              <input
                type="text"
                value={newName}
                onChange={e => setNewName(e.target.value)}
                onKeyDown={e => { if (e.key === "Enter" && newName.trim()) onSelect(newName); }}
                placeholder="Enter your full name..."
                className="w-full px-4 py-3 bg-white/5 border border-white/10 rounded-lg text-white placeholder:text-neutral-500 focus:border-orange-500/60 focus:outline-none text-sm"
              />
              <button
                onClick={() => newName.trim() && onSelect(newName)}
                disabled={!newName.trim()}
                className="w-full py-3 bg-gradient-to-r from-red-600 to-orange-500 text-white font-bold uppercase tracking-widest text-sm rounded-lg hover:shadow-lg hover:shadow-orange-500/30 transition-all disabled:opacity-40 disabled:cursor-not-allowed"
              >
                Continue as new coach
              </button>
            </div>
          </div>

          <p className="text-center text-neutral-500 text-xs mt-6 tracking-wider">
            1, 2, 3... <span className="text-orange-500">Fox!</span> 🦊
          </p>
        </div>
      </div>
    </div>
  );
}

// Fox Football official logo (uses embedded base64)
function FoxLogoMark({ className = "", rounded = false }) {
  return (
    <img
      src={FOX_LOGO_B64}
      alt="Fox Football Vietnam"
      className={className}
      style={{
        objectFit: "contain",
        borderRadius: rounded ? "50%" : 0,
        display: "block"
      }}
      crossOrigin="anonymous"
    />
  );
}


// =====================================================
// DASHBOARD VIEW
// =====================================================

function DashboardView({ currentCoach, matches, onSwitchCoach, onNewMatch, onEditMatch, onDeleteMatch, onPreviewMatch, onImportData, onResetData }) {
  const [activeTab, setActiveTab] = useState("overview"); // overview, reports, stats
  const [filterTeam, setFilterTeam] = useState("");
  const [filterCoach, setFilterCoach] = useState("");
  const [filterType, setFilterType] = useState("");
  const [searchQuery, setSearchQuery] = useState("");
  const [selectedSeason, setSelectedSeason] = useState(getCurrentSeason());
  const [confirmDelete, setConfirmDelete] = useState(null);
  const [showSettingsMenu, setShowSettingsMenu] = useState(false);
  const [showImportModal, setShowImportModal] = useState(false);
  const [showResetConfirm, setShowResetConfirm] = useState(false);
  const settingsRef = useRef(null);

  useEffect(() => {
    const handleClick = (e) => {
      if (settingsRef.current && !settingsRef.current.contains(e.target)) {
        setShowSettingsMenu(false);
      }
    };
    document.addEventListener("mousedown", handleClick);
    return () => document.removeEventListener("mousedown", handleClick);
  }, []);

  const handleExport = () => {
    const exportData = {
      matches: matches,
      coaches: Array.from(new Set(matches.map(m => m.coach).filter(Boolean))).sort(),
      opponents: Array.from(new Set(matches.map(m => m.opponent).filter(Boolean))).sort(),
      venues: Array.from(new Set(matches.map(m => m.venue).filter(Boolean))).sort(),
      playersByTeam: {},
      exportedAt: new Date().toISOString()
    };
    // Build playersByTeam from matches
    matches.forEach(m => {
      if (!m.team) return;
      if (!exportData.playersByTeam[m.team]) exportData.playersByTeam[m.team] = new Set();
      [...(m.goalscorers || []), ...(m.assists || []), ...(m.mvps || []),
       ...(m.specialMentions || []), ...(m.goalkeepers || [])].forEach(item => {
        const name = typeof item === "string" ? item : item.name;
        if (name) exportData.playersByTeam[m.team].add(name);
      });
    });
    Object.keys(exportData.playersByTeam).forEach(t => {
      exportData.playersByTeam[t] = Array.from(exportData.playersByTeam[t]).sort();
    });
    const blob = new Blob([JSON.stringify(exportData, null, 2)], { type: "application/json" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = `ffv_backup_${new Date().toISOString().split("T")[0]}.json`;
    a.click();
    URL.revokeObjectURL(url);
  };

  const handleImportFile = async (e) => {
    const file = e.target.files?.[0];
    if (!file) return;
    try {
      const text = await file.text();
      const data = JSON.parse(text);
      const result = await onImportData(data);
      if (result?.ok) setShowImportModal(false);
    } catch (err) {
      console.error("Parse error:", err);
      alert("Could not parse the file. Make sure it is a valid JSON export.");
    }
    e.target.value = "";
  };

  // Filter matches
  const filteredMatches = matches.filter(m => {
    if (filterTeam && m.team !== filterTeam) return false;
    if (filterCoach && m.coach !== filterCoach) return false;
    if (filterType && m.matchType !== filterType) return false;
    if (selectedSeason && getCurrentSeason(new Date(m.date)) !== selectedSeason) return false;
    if (searchQuery) {
      const q = searchQuery.toLowerCase();
      const matchStr = `${m.opponent} ${m.team} ${m.coach} ${m.venue}`.toLowerCase();
      if (!matchStr.includes(q)) return false;
    }
    return true;
  }).sort((a, b) => new Date(b.date) - new Date(a.date));

  // Get all unique seasons from matches
  const allSeasons = Array.from(new Set(matches.map(m => getCurrentSeason(new Date(m.date)))))
    .sort().reverse();
  if (!allSeasons.includes(getCurrentSeason())) allSeasons.unshift(getCurrentSeason());

  // Get unique values for filters
  const uniqueTeams = Array.from(new Set(matches.map(m => m.team).filter(Boolean))).sort();
  const uniqueCoaches = Array.from(new Set(matches.map(m => m.coach).filter(Boolean))).sort();

  // Stats for selected season
  const seasonMatches = matches.filter(m =>
    getCurrentSeason(new Date(m.date)) === selectedSeason &&
    (!filterTeam || m.team === filterTeam)
  );

  const stats = computeStats(seasonMatches);

  return (
    <div className="min-h-screen bg-neutral-50">
      {/* Header */}
      <header className="bg-neutral-950 border-b-4 border-orange-500 sticky top-0 z-30">
        <div className="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between">
          <div className="flex items-center gap-4">
            <div className="w-11 h-11 rounded-full bg-gradient-to-br from-red-600 to-orange-500 flex items-center justify-center">
              <FoxLogoMark className="w-6 h-6" />
            </div>
            <div>
              <h1 className="font-display text-white text-2xl tracking-wider leading-none">FOX FOOTBALL</h1>
              <p className="text-orange-500 text-[10px] uppercase tracking-[0.3em] font-bold">Vietnam · Match Reports</p>
            </div>
          </div>

          <div className="flex items-center gap-4">
            <div className="hidden md:flex items-center gap-2.5 px-3 py-1.5 bg-white/5 rounded-lg border border-white/10">
              <div className="w-7 h-7 rounded-full bg-gradient-to-br from-red-600 to-orange-500 flex items-center justify-center text-white font-bold text-xs">
                {currentCoach?.charAt(0).toUpperCase()}
              </div>
              <div className="text-left">
                <p className="text-[10px] text-neutral-400 uppercase tracking-wider leading-none">Coach</p>
                <p className="text-white text-xs font-bold leading-tight">{currentCoach}</p>
              </div>
              <button onClick={onSwitchCoach} className="ml-2 text-neutral-500 hover:text-orange-400 transition-colors" title="Switch coach">
                <RefreshCw className="w-3.5 h-3.5" />
              </button>
            </div>
            <div className="relative" ref={settingsRef}>
              <button
                onClick={() => setShowSettingsMenu(!showSettingsMenu)}
                className="p-2.5 rounded-lg bg-white/5 hover:bg-white/10 border border-white/10 text-neutral-400 hover:text-white transition-colors"
                title="Settings"
              >
                <Settings className="w-4 h-4" />
              </button>
              {showSettingsMenu && (
                <div className="absolute right-0 mt-2 w-56 bg-neutral-900 border border-white/10 rounded-lg shadow-2xl z-40 overflow-hidden">
                  <button
                    onClick={() => { setShowImportModal(true); setShowSettingsMenu(false); }}
                    className="w-full flex items-center gap-3 px-4 py-3 text-left text-sm text-neutral-300 hover:bg-white/5 hover:text-white transition-colors"
                  >
                    <Upload className="w-4 h-4 text-orange-500" />
                    <div>
                      <p className="font-bold text-xs uppercase tracking-wider">Import Data</p>
                      <p className="text-[10px] text-neutral-500">Load JSON backup</p>
                    </div>
                  </button>
                  <button
                    onClick={() => { handleExport(); setShowSettingsMenu(false); }}
                    className="w-full flex items-center gap-3 px-4 py-3 text-left text-sm text-neutral-300 hover:bg-white/5 hover:text-white transition-colors border-t border-white/5"
                  >
                    <Download className="w-4 h-4 text-blue-500" />
                    <div>
                      <p className="font-bold text-xs uppercase tracking-wider">Export Data</p>
                      <p className="text-[10px] text-neutral-500">Download JSON backup</p>
                    </div>
                  </button>
                  <button
                    onClick={() => { setShowResetConfirm(true); setShowSettingsMenu(false); }}
                    className="w-full flex items-center gap-3 px-4 py-3 text-left text-sm text-red-400 hover:bg-red-500/10 transition-colors border-t border-white/5"
                  >
                    <Trash2 className="w-4 h-4" />
                    <div>
                      <p className="font-bold text-xs uppercase tracking-wider">Reset All Data</p>
                      <p className="text-[10px] text-red-500/70">Delete all matches</p>
                    </div>
                  </button>
                </div>
              )}
            </div>
            <Button onClick={onNewMatch} icon={Plus} size="md">New Report</Button>
          </div>
        </div>

        {/* Tab navigation */}
        <div className="max-w-7xl mx-auto px-6 flex gap-1">
          {[
            { id: "overview", label: "Overview", icon: TrendingUp },
            { id: "reports", label: "Match Reports", icon: FileText },
            { id: "stats", label: "Statistics", icon: BarChart3 }
          ].map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`flex items-center gap-2 px-4 py-3 text-xs font-bold uppercase tracking-widest transition-colors relative ${
                activeTab === tab.id ? "text-orange-500" : "text-neutral-400 hover:text-white"
              }`}
            >
              <tab.icon className="w-4 h-4" />
              {tab.label}
              {activeTab === tab.id && (
                <div className="absolute bottom-0 left-0 right-0 h-0.5 bg-orange-500" />
              )}
            </button>
          ))}
        </div>
      </header>

      <main className="max-w-7xl mx-auto px-6 py-8">
        {/* Season selector + filters bar */}
        <div className="flex flex-wrap items-center gap-3 mb-6">
          <Select
            value={selectedSeason}
            onChange={setSelectedSeason}
            options={allSeasons.map(s => `${s}`)}
            placeholder="Season"
          />
          <div className="text-xs text-neutral-500 uppercase tracking-wider font-bold">
            {seasonMatches.length} match{seasonMatches.length !== 1 ? "es" : ""} this season
          </div>
        </div>

        {activeTab === "overview" && (
          <OverviewTab
            stats={stats}
            recentMatches={filteredMatches.slice(0, 6)}
            onPreviewMatch={onPreviewMatch}
            onNewMatch={onNewMatch}
          />
        )}

        {activeTab === "reports" && (
          <ReportsTab
            matches={filteredMatches}
            uniqueTeams={uniqueTeams}
            uniqueCoaches={uniqueCoaches}
            filterTeam={filterTeam}
            setFilterTeam={setFilterTeam}
            filterCoach={filterCoach}
            setFilterCoach={setFilterCoach}
            filterType={filterType}
            setFilterType={setFilterType}
            searchQuery={searchQuery}
            setSearchQuery={setSearchQuery}
            onPreviewMatch={onPreviewMatch}
            onEditMatch={onEditMatch}
            onDeleteMatch={(id) => setConfirmDelete(id)}
          />
        )}

        {activeTab === "stats" && (
          <StatsTab
            seasonMatches={seasonMatches}
            uniqueTeams={uniqueTeams}
            filterTeam={filterTeam}
            setFilterTeam={setFilterTeam}
          />
        )}
      </main>

      {/* Confirm delete modal */}
      <Modal isOpen={!!confirmDelete} onClose={() => setConfirmDelete(null)} title="Delete Match Report">
        <p className="text-neutral-700 mb-6">Are you sure you want to delete this match report? This action cannot be undone.</p>
        <div className="flex gap-3 justify-end">
          <Button onClick={() => setConfirmDelete(null)} variant="ghost">Cancel</Button>
          <Button onClick={() => { onDeleteMatch(confirmDelete); setConfirmDelete(null); }} variant="danger" icon={Trash2}>Delete</Button>
        </div>
      </Modal>

      {/* Import modal */}
      <Modal isOpen={showImportModal} onClose={() => setShowImportModal(false)} title="Import Match Data">
        <div className="space-y-4">
          <p className="text-sm text-neutral-700">
            Upload a JSON backup file to import matches, coaches, opponents, and players.
            Existing data will be preserved. Duplicate matches (matching id) will be skipped.
          </p>
          <label className="flex flex-col items-center justify-center h-40 border-2 border-dashed border-neutral-300 rounded-lg cursor-pointer hover:border-orange-500 hover:bg-orange-50/30 transition-colors">
            <Upload className="w-10 h-10 text-neutral-300 mb-2" />
            <p className="text-sm font-bold text-neutral-700 uppercase tracking-wider">Choose JSON file</p>
            <p className="text-xs text-neutral-400 mt-1">or drop it here</p>
            <input type="file" accept="application/json,.json" onChange={handleImportFile} className="hidden" />
          </label>
          <div className="bg-orange-50 border border-orange-200 rounded-md p-3">
            <p className="text-xs text-orange-900">
              <strong>Tip:</strong> Use the "Export Data" option to back up your data regularly.
              Imported data merges with existing data without overwriting.
            </p>
          </div>
        </div>
      </Modal>

      {/* Reset confirmation modal */}
      <Modal isOpen={showResetConfirm} onClose={() => setShowResetConfirm(false)} title="Reset All Data">
        <p className="text-neutral-700 mb-2"><strong>Warning:</strong> This will permanently delete:</p>
        <ul className="text-sm text-neutral-700 mb-4 list-disc pl-6 space-y-0.5">
          <li>All match reports ({matches.length})</li>
          <li>All saved coaches, opponents, venues</li>
          <li>All learned player names</li>
        </ul>
        <p className="text-sm text-red-600 font-bold mb-6">This action cannot be undone. Consider exporting a backup first.</p>
        <div className="flex gap-3 justify-end">
          <Button onClick={() => setShowResetConfirm(false)} variant="ghost">Cancel</Button>
          <Button onClick={() => { onResetData(); setShowResetConfirm(false); }} variant="danger" icon={Trash2}>Yes, Delete Everything</Button>
        </div>
      </Modal>
    </div>
  );
}

// Compute stats from a set of matches
function computeStats(matches) {
  const stats = {
    totalMatches: matches.length,
    wins: 0,
    draws: 0,
    losses: 0,
    goalsFor: 0,
    goalsAgainst: 0,
    cleanSheets: 0,
    topScorers: {},
    topAssisters: {},
    mvpCounts: {},
    upcomingMatches: []
  };

  matches.forEach(m => {
    const ourScore = parseInt(m.ourScore) || 0;
    const oppScore = parseInt(m.opponentScore) || 0;
    stats.goalsFor += ourScore;
    stats.goalsAgainst += oppScore;
    if (ourScore > oppScore) stats.wins++;
    else if (ourScore < oppScore) stats.losses++;
    else stats.draws++;
    if (oppScore === 0 && ourScore > 0) stats.cleanSheets++;

    (m.goalscorers || []).forEach(g => {
      const name = g.name || g;
      const goals = g.goals || 1;
      if (name) stats.topScorers[name] = (stats.topScorers[name] || 0) + goals;
    });
    (m.assists || []).forEach(a => {
      const name = a.name || a;
      if (name) stats.topAssisters[name] = (stats.topAssisters[name] || 0) + 1;
    });
    (m.mvps || []).forEach(mvp => {
      const name = mvp.name || mvp;
      if (name) stats.mvpCounts[name] = (stats.mvpCounts[name] || 0) + 1;
    });
    if (m.nextMatchPreview && new Date(m.date) > new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)) {
      stats.upcomingMatches.push({ team: m.team, preview: m.nextMatchPreview, fromMatchDate: m.date });
    }
  });

  return stats;
}

// =====================================================
// OVERVIEW TAB
// =====================================================

function OverviewTab({ stats, recentMatches, onPreviewMatch, onNewMatch }) {
  if (stats.totalMatches === 0) {
    return (
      <div className="bg-white border-2 border-dashed border-neutral-200 rounded-2xl p-16 text-center">
        <div className="w-20 h-20 mx-auto mb-5 rounded-full bg-gradient-to-br from-red-600 to-orange-500 flex items-center justify-center">
          <Trophy className="w-9 h-9 text-white" />
        </div>
        <h3 className="font-display text-3xl text-neutral-900 mb-2 tracking-wider">YOUR FIRST MATCH AWAITS</h3>
        <p className="text-neutral-500 text-sm mb-7 max-w-sm mx-auto">
          Create your first match report to start building your team's season story.
        </p>
        <Button onClick={onNewMatch} icon={Plus} size="lg">Create First Report</Button>
      </div>
    );
  }

  const winPct = stats.totalMatches > 0 ? Math.round((stats.wins / stats.totalMatches) * 100) : 0;
  const goalDiff = stats.goalsFor - stats.goalsAgainst;

  const topScorerEntries = Object.entries(stats.topScorers).sort((a, b) => b[1] - a[1]).slice(0, 5);
  const topMvpEntries = Object.entries(stats.mvpCounts).sort((a, b) => b[1] - a[1]).slice(0, 3);

  return (
    <div className="space-y-6">
      {/* KPI Cards */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        <KpiCard label="Matches" value={stats.totalMatches} icon={Calendar} accent="neutral" />
        <KpiCard label="Win Rate" value={`${winPct}%`} sublabel={`${stats.wins}W ${stats.draws}D ${stats.losses}L`} icon={Trophy} accent="green" />
        <KpiCard label="Goal Difference" value={goalDiff > 0 ? `+${goalDiff}` : goalDiff} sublabel={`${stats.goalsFor} for · ${stats.goalsAgainst} against`} icon={Target} accent="orange" />
        <KpiCard label="Clean Sheets" value={stats.cleanSheets} icon={Shield} accent="blue" />
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Recent matches */}
        <div className="lg:col-span-2 bg-white rounded-xl border border-neutral-200 overflow-hidden">
          <div className="px-5 py-4 border-b border-neutral-200 flex items-center justify-between">
            <h3 className="font-display text-xl tracking-wider text-neutral-900">RECENT MATCHES</h3>
            <FileText className="w-4 h-4 text-neutral-400" />
          </div>
          <div className="divide-y divide-neutral-100">
            {recentMatches.length === 0 ? (
              <p className="p-8 text-center text-neutral-500 text-sm">No matches in this season yet</p>
            ) : recentMatches.map(m => (
              <MatchListItem key={m.id} match={m} onClick={() => onPreviewMatch(m)} compact />
            ))}
          </div>
        </div>

        {/* Top scorers */}
        <div className="bg-white rounded-xl border border-neutral-200 overflow-hidden">
          <div className="px-5 py-4 border-b border-neutral-200 flex items-center justify-between">
            <h3 className="font-display text-xl tracking-wider text-neutral-900">TOP SCORERS</h3>
            <Target className="w-4 h-4 text-orange-500" />
          </div>
          {topScorerEntries.length === 0 ? (
            <p className="p-8 text-center text-neutral-500 text-sm">No goals yet</p>
          ) : (
            <div className="p-3 space-y-1">
              {topScorerEntries.map(([name, goals], i) => (
                <div key={name} className="flex items-center gap-3 p-2.5 rounded-lg hover:bg-neutral-50">
                  <div className={`w-7 h-7 rounded-full flex items-center justify-center text-xs font-black ${
                    i === 0 ? "bg-gradient-to-br from-yellow-400 to-orange-500 text-white" :
                    i === 1 ? "bg-neutral-300 text-neutral-700" :
                    i === 2 ? "bg-orange-200 text-orange-800" :
                    "bg-neutral-100 text-neutral-500"
                  }`}>{i + 1}</div>
                  <span className="flex-1 text-sm font-medium text-neutral-900 truncate">{name}</span>
                  <span className="text-sm font-black text-orange-600">{goals}<span className="text-xs ml-0.5">⚽</span></span>
                </div>
              ))}
            </div>
          )}

          {topMvpEntries.length > 0 && (
            <>
              <div className="px-5 py-3 border-y border-neutral-200 flex items-center gap-2">
                <Star className="w-4 h-4 text-yellow-500" />
                <h4 className="font-display text-sm tracking-wider text-neutral-900">MVP LEADERS</h4>
              </div>
              <div className="p-3 space-y-1">
                {topMvpEntries.map(([name, count]) => (
                  <div key={name} className="flex items-center gap-3 p-2 rounded-lg hover:bg-neutral-50">
                    <Star className="w-4 h-4 text-yellow-500" />
                    <span className="flex-1 text-sm font-medium text-neutral-900 truncate">{name}</span>
                    <span className="text-xs font-bold text-neutral-600">{count}×</span>
                  </div>
                ))}
              </div>
            </>
          )}
        </div>
      </div>

      {/* Upcoming matches */}
      {stats.upcomingMatches.length > 0 && (
        <div className="bg-gradient-to-br from-neutral-950 to-neutral-900 rounded-xl border border-orange-500/30 overflow-hidden">
          <div className="px-5 py-4 border-b border-white/10 flex items-center gap-2">
            <Calendar className="w-4 h-4 text-orange-500" />
            <h3 className="font-display text-xl tracking-wider text-white">UPCOMING</h3>
          </div>
          <div className="p-5 space-y-3">
            {stats.upcomingMatches.slice(0, 3).map((u, i) => (
              <div key={i} className="flex items-start gap-4">
                <div className="px-2.5 py-1 bg-orange-500/20 text-orange-400 text-[10px] font-bold uppercase tracking-widest rounded">
                  {u.team}
                </div>
                <p className="text-neutral-300 text-sm flex-1">{u.preview}</p>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}

function KpiCard({ label, value, sublabel, icon: Icon, accent = "neutral" }) {
  const accents = {
    neutral: "from-neutral-100 to-neutral-50 text-neutral-600",
    green: "from-green-100 to-green-50 text-green-700",
    red: "from-red-100 to-red-50 text-red-700",
    orange: "from-orange-100 to-orange-50 text-orange-700",
    blue: "from-blue-100 to-blue-50 text-blue-700"
  };
  return (
    <div className="bg-white rounded-xl border border-neutral-200 p-5 hover:shadow-lg hover:-translate-y-0.5 transition-all duration-300">
      <div className="flex items-start justify-between mb-2">
        <p className="text-[10px] text-neutral-500 uppercase tracking-[0.2em] font-bold">{label}</p>
        <div className={`w-8 h-8 rounded-lg bg-gradient-to-br ${accents[accent]} flex items-center justify-center`}>
          <Icon className="w-4 h-4" />
        </div>
      </div>
      <p className="font-display text-4xl text-neutral-900 leading-none mb-1">{value}</p>
      {sublabel && <p className="text-xs text-neutral-500 font-medium">{sublabel}</p>}
    </div>
  );
}


// =====================================================
// REPORTS TAB (list of all match reports)
// =====================================================

function ReportsTab({ matches, uniqueTeams, uniqueCoaches, filterTeam, setFilterTeam, filterCoach, setFilterCoach, filterType, setFilterType, searchQuery, setSearchQuery, onPreviewMatch, onEditMatch, onDeleteMatch }) {
  return (
    <div className="space-y-5">
      {/* Filters */}
      <div className="bg-white rounded-xl border border-neutral-200 p-4">
        <div className="grid grid-cols-1 md:grid-cols-4 gap-3">
          <Input
            value={searchQuery}
            onChange={setSearchQuery}
            placeholder="Search opponent, venue..."
            icon={Search}
          />
          <Select value={filterTeam} onChange={setFilterTeam} options={uniqueTeams} placeholder="All teams" />
          <Select value={filterCoach} onChange={setFilterCoach} options={uniqueCoaches} placeholder="All coaches" />
          <Select value={filterType} onChange={setFilterType} options={MATCH_TYPES} placeholder="All match types" />
        </div>
      </div>

      {/* Matches list */}
      {matches.length === 0 ? (
        <div className="bg-white border-2 border-dashed border-neutral-200 rounded-xl p-12 text-center">
          <FileText className="w-12 h-12 mx-auto mb-3 text-neutral-300" />
          <p className="text-neutral-500 text-sm">No matches found with current filters</p>
        </div>
      ) : (
        <div className="bg-white rounded-xl border border-neutral-200 divide-y divide-neutral-100 overflow-hidden">
          {matches.map(m => (
            <MatchListItem
              key={m.id}
              match={m}
              onClick={() => onPreviewMatch(m)}
              onEdit={() => onEditMatch(m)}
              onDelete={() => onDeleteMatch(m.id)}
            />
          ))}
        </div>
      )}
    </div>
  );
}

function MatchListItem({ match, onClick, onEdit, onDelete, compact = false }) {
  const ourScore = parseInt(match.ourScore) || 0;
  const oppScore = parseInt(match.opponentScore) || 0;
  const result = ourScore > oppScore ? "W" : ourScore < oppScore ? "L" : "D";
  const resultColors = {
    W: "bg-green-500 text-white",
    D: "bg-yellow-500 text-white",
    L: "bg-red-500 text-white"
  };
  const dateStr = new Date(match.date).toLocaleDateString("en-GB", { day: "numeric", month: "short", year: "numeric" });

  return (
    <div className="flex items-center gap-4 p-4 hover:bg-neutral-50 transition-colors group">
      <div className={`flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center font-display text-lg ${resultColors[result]}`}>
        {result}
      </div>
      <button onClick={onClick} className="flex-1 min-w-0 text-left">
        <div className="flex items-center gap-2 mb-1 flex-wrap">
          <Badge color="orange">{match.team}</Badge>
          <span className="text-[10px] text-neutral-500 uppercase tracking-widest font-bold">Match #{match.matchNumber}</span>
          {match.matchType && !compact && <Badge color="neutral">{match.matchType}</Badge>}
          {oppScore === 0 && ourScore > 0 && <Badge color="blue">🧤 Clean Sheet</Badge>}
        </div>
        <div className="flex items-baseline gap-2">
          <h4 className="font-display text-lg tracking-wider text-neutral-900 truncate">
            FOX vs {match.opponent || "TBD"}
          </h4>
          <span className="font-display text-lg text-neutral-900">
            {ourScore}<span className="text-neutral-400 mx-1">-</span>{oppScore}
          </span>
        </div>
        <div className="flex items-center gap-3 text-xs text-neutral-500 mt-0.5">
          <span className="flex items-center gap-1"><Calendar className="w-3 h-3" />{dateStr}</span>
          {match.venue && <span className="flex items-center gap-1"><MapPin className="w-3 h-3" />{match.venue}</span>}
          {!compact && <span className="flex items-center gap-1"><User className="w-3 h-3" />{match.coach}</span>}
        </div>
      </button>

      {!compact && (
        <div className="flex items-center gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
          <button onClick={onClick} className="p-2 rounded-lg hover:bg-neutral-200 text-neutral-600" title="View">
            <Eye className="w-4 h-4" />
          </button>
          <button onClick={onEdit} className="p-2 rounded-lg hover:bg-neutral-200 text-neutral-600" title="Edit">
            <Edit3 className="w-4 h-4" />
          </button>
          <button onClick={onDelete} className="p-2 rounded-lg hover:bg-red-100 text-red-600" title="Delete">
            <Trash2 className="w-4 h-4" />
          </button>
        </div>
      )}
      {compact && <ChevronRight className="w-4 h-4 text-neutral-400" />}
    </div>
  );
}

// =====================================================
// STATS TAB (detailed stats per team)
// =====================================================

function StatsTab({ seasonMatches, uniqueTeams, filterTeam, setFilterTeam }) {
  const stats = computeStats(seasonMatches);

  const topScorers = Object.entries(stats.topScorers).sort((a, b) => b[1] - a[1]);
  const topAssisters = Object.entries(stats.topAssisters).sort((a, b) => b[1] - a[1]);
  const topMvps = Object.entries(stats.mvpCounts).sort((a, b) => b[1] - a[1]);

  return (
    <div className="space-y-6">
      <div className="flex flex-wrap items-center gap-3">
        <Select value={filterTeam} onChange={setFilterTeam} options={uniqueTeams} placeholder="All teams" />
        {filterTeam && (
          <button onClick={() => setFilterTeam("")} className="text-xs text-neutral-500 hover:text-orange-600 font-bold uppercase tracking-wider flex items-center gap-1">
            <X className="w-3 h-3" /> Clear filter
          </button>
        )}
      </div>

      {seasonMatches.length === 0 ? (
        <div className="bg-white border-2 border-dashed border-neutral-200 rounded-xl p-12 text-center">
          <BarChart3 className="w-12 h-12 mx-auto mb-3 text-neutral-300" />
          <p className="text-neutral-500 text-sm">No data for this filter</p>
        </div>
      ) : (
        <>
          {/* Season summary */}
          <div className="grid grid-cols-2 md:grid-cols-5 gap-4">
            <KpiCard label="Played" value={stats.totalMatches} icon={Calendar} />
            <KpiCard label="Won" value={stats.wins} icon={Trophy} accent="green" />
            <KpiCard label="Drawn" value={stats.draws} icon={RefreshCw} accent="orange" />
            <KpiCard label="Lost" value={stats.losses} icon={X} accent="red" />
            <KpiCard label="Clean Sheets" value={stats.cleanSheets} icon={Shield} accent="blue" />
          </div>

          {/* Leaderboards */}
          <div className="grid grid-cols-1 md:grid-cols-3 gap-5">
            <Leaderboard title="Top Scorers" icon={Target} entries={topScorers} suffix="⚽" accent="orange" />
            <Leaderboard title="Top Assists" icon={Sparkles} entries={topAssisters} suffix="🎯" accent="blue" />
            <Leaderboard title="MVPs" icon={Star} entries={topMvps} suffix="⭐" accent="yellow" />
          </div>
        </>
      )}
    </div>
  );
}

function Leaderboard({ title, icon: Icon, entries, suffix, accent = "orange" }) {
  const accents = {
    orange: "text-orange-500",
    blue: "text-blue-500",
    yellow: "text-yellow-500"
  };
  return (
    <div className="bg-white rounded-xl border border-neutral-200 overflow-hidden">
      <div className="px-5 py-4 border-b border-neutral-200 flex items-center justify-between">
        <h3 className="font-display text-lg tracking-wider text-neutral-900">{title}</h3>
        <Icon className={`w-4 h-4 ${accents[accent]}`} />
      </div>
      {entries.length === 0 ? (
        <p className="p-8 text-center text-neutral-400 text-sm">No data yet</p>
      ) : (
        <div className="p-3 space-y-1 max-h-80 overflow-y-auto scrollbar-thin">
          {entries.map(([name, count], i) => (
            <div key={name} className="flex items-center gap-3 p-2.5 rounded-lg hover:bg-neutral-50">
              <div className={`w-6 h-6 rounded-full flex items-center justify-center text-[10px] font-black ${
                i === 0 ? "bg-gradient-to-br from-yellow-400 to-orange-500 text-white" :
                i === 1 ? "bg-neutral-300 text-neutral-700" :
                i === 2 ? "bg-orange-200 text-orange-800" :
                "bg-neutral-100 text-neutral-500"
              }`}>{i + 1}</div>
              <span className="flex-1 text-sm font-medium text-neutral-900 truncate">{name}</span>
              <span className="text-sm font-black text-neutral-900">{count}<span className="text-xs ml-1">{suffix}</span></span>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}


// =====================================================
// MATCH FORM VIEW
// =====================================================

function MatchFormView({ currentCoach, editingMatch, coaches, opponents, venues, playersByTeam, getNextMatchNumber, onSave, onCancel, showToast }) {
  const today = new Date().toISOString().split("T")[0];

  // Form state - initialize from editing match or defaults
  const [form, setForm] = useState(() => editingMatch || {
    coach: currentCoach,
    date: today,
    team: "",
    opponent: "",
    matchType: "Friendly",
    venue: "",
    isHome: true,
    ourScore: "",
    opponentScore: "",
    goalkeepers: [],
    goalscorers: [],
    assists: [],
    mvps: [],
    specialMentions: [],
    captain: "",
    summary: "",
    summaryCorrected: "",
    areasOfImprovement: "",
    nextMatchPreview: "",
    teamPhoto: null
  });

  const [errors, setErrors] = useState({});
  const [isCorrectingGrammar, setIsCorrectingGrammar] = useState(false);
  const [grammarComparison, setGrammarComparison] = useState(null);
  const [showCropModal, setShowCropModal] = useState(false);
  const [rawPhoto, setRawPhoto] = useState(null);

  // Auto-save draft
  useEffect(() => {
    const t = setTimeout(() => {
      if (!editingMatch) {
        storage.set("ffv:draft", form);
      }
    }, 1500);
    return () => clearTimeout(t);
  }, [form, editingMatch]);

  // Load draft on mount (only for new matches)
  useEffect(() => {
    if (!editingMatch) {
      storage.get("ffv:draft").then(draft => {
        if (draft && draft.team) {
          // Show prompt to restore? For now, restore silently if recent
          const draftAge = Date.now() - new Date(draft._savedAt || 0).getTime();
          if (draftAge < 24 * 60 * 60 * 1000) { // less than 24 hours
            // Don't auto-restore for now to avoid confusion; could add a banner
          }
        }
      });
    }
  }, []);

  const update = (field, value) => {
    setForm(prev => ({ ...prev, [field]: value, _savedAt: new Date().toISOString() }));
    if (errors[field]) setErrors(prev => ({ ...prev, [field]: null }));
  };

  const matchNumber = form.team && form.date ? getNextMatchNumber(form.team, form.date) : null;
  const teamPlayers = form.team ? (playersByTeam[form.team] || []) : [];

  // ========== Goal scorers helpers ==========
  const addGoalscorer = () => {
    update("goalscorers", [...form.goalscorers, { name: "", goals: 1 }]);
  };
  const updateGoalscorer = (idx, field, value) => {
    const upd = [...form.goalscorers];
    upd[idx] = { ...upd[idx], [field]: value };
    update("goalscorers", upd);
  };
  const removeGoalscorer = (idx) => {
    update("goalscorers", form.goalscorers.filter((_, i) => i !== idx));
  };

  // ========== List helpers (assists, mvps, mentions, gks) ==========
  const addToList = (field, defaultValue = { name: "" }) => {
    update(field, [...(form[field] || []), defaultValue]);
  };
  const updateInList = (field, idx, key, value) => {
    const upd = [...(form[field] || [])];
    upd[idx] = { ...upd[idx], [key]: value };
    update(field, upd);
  };
  const removeFromList = (field, idx) => {
    update(field, (form[field] || []).filter((_, i) => i !== idx));
  };

  // ========== Total goals validation ==========
  const totalGoalsCounted = form.goalscorers.reduce((sum, g) => sum + (parseInt(g.goals) || 0), 0);
  const ourScoreNum = parseInt(form.ourScore) || 0;
  const goalsMismatch = form.goalscorers.length > 0 && totalGoalsCounted !== ourScoreNum;

  // ========== Grammar correction ==========
  const correctGrammar = async () => {
    if (!form.summary?.trim()) return;
    setIsCorrectingGrammar(true);
    try {
      const corrected = await callClaude(
        `Please correct the grammar, syntax, spelling, and punctuation of the following match summary written by a youth football coach. Keep the same meaning, tone, and approximate length. Maintain a warm but professional tone. Do not add new information. Important: do not use hyphens to separate clauses; rewrite to use periods, commas, or "and" instead. Return ONLY the corrected text, nothing else.\n\nText to correct:\n"""\n${form.summary}\n"""`,
        "You are an expert English editor specialized in sports writing for youth football clubs."
      );
      setGrammarComparison({ original: form.summary, corrected: corrected });
    } catch (e) {
      showToast("Could not correct grammar. Try again.", "error");
    } finally {
      setIsCorrectingGrammar(false);
    }
  };

  // ========== Photo handling ==========
  const handlePhotoUpload = (e) => {
    const file = e.target.files?.[0];
    if (!file) return;
    if (!file.type.startsWith("image/")) {
      showToast("Please upload an image file", "error");
      return;
    }
    const reader = new FileReader();
    reader.onload = () => {
      setRawPhoto(reader.result);
      setShowCropModal(true);
    };
    reader.readAsDataURL(file);
  };

  // ========== Validation & submit ==========
  const validate = () => {
    const e = {};
    if (!form.coach) e.coach = "Required";
    if (!form.date) e.date = "Required";
    if (!form.team) e.team = "Required";
    if (!form.opponent?.trim()) e.opponent = "Required";
    if (form.ourScore === "" || form.ourScore === null) e.ourScore = "Required";
    if (form.opponentScore === "" || form.opponentScore === null) e.opponentScore = "Required";
    if (!form.summary?.trim()) e.summary = "Required";
    if (!form.teamPhoto) e.teamPhoto = "Team photo required";
    return e;
  };

  const handleSubmit = () => {
    const e = validate();
    if (Object.keys(e).length > 0) {
      setErrors(e);
      showToast("Please complete the required fields", "error");
      // Scroll to first error
      const firstErrorField = Object.keys(e)[0];
      const el = document.getElementById(`field-${firstErrorField}`);
      if (el) el.scrollIntoView({ behavior: "smooth", block: "center" });
      return;
    }
    const finalMatch = {
      ...form,
      matchNumber: form.matchNumber || matchNumber,
      season: getCurrentSeason(new Date(form.date)),
      // Clean empty entries
      goalscorers: form.goalscorers.filter(g => g.name?.trim()),
      assists: (form.assists || []).filter(a => a.name?.trim()),
      mvps: (form.mvps || []).filter(m => m.name?.trim()),
      specialMentions: (form.specialMentions || []).filter(s => s.name?.trim()),
      goalkeepers: (form.goalkeepers || []).filter(g => g.name?.trim())
    };
    onSave(finalMatch);
    storage.delete("ffv:draft");
  };

  return (
    <div className="min-h-screen bg-neutral-50">
      {/* Header */}
      <header className="bg-neutral-950 border-b-4 border-orange-500 sticky top-0 z-30">
        <div className="max-w-5xl mx-auto px-6 py-4 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <button onClick={onCancel} className="p-2 -ml-2 text-neutral-400 hover:text-white transition-colors">
              <ChevronLeft className="w-5 h-5" />
            </button>
            <div>
              <h1 className="font-display text-white text-xl tracking-wider leading-none">
                {editingMatch ? "EDIT MATCH REPORT" : "NEW MATCH REPORT"}
              </h1>
              <p className="text-neutral-500 text-[10px] uppercase tracking-[0.25em]">
                {form.team && matchNumber ? `${form.team} · Match #${matchNumber}` : "Fill in match details"}
              </p>
            </div>
          </div>
          <div className="flex items-center gap-2">
            <Button onClick={onCancel} variant="ghost" size="sm" className="!text-neutral-400 hover:!text-white hover:!bg-white/5">Cancel</Button>
            <Button onClick={handleSubmit} icon={Save} size="md">Save & Generate</Button>
          </div>
        </div>
      </header>

      <main className="max-w-5xl mx-auto px-6 py-8 space-y-6">
        {/* SECTION 1: Match Information */}
        <FormSection title="Match Information" icon={Calendar} step={1}>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <AutocompleteInput
              label="Coach in charge"
              value={form.coach}
              onChange={(v) => update("coach", v)}
              suggestions={coaches}
              placeholder="Coach name..."
              required
            />
            <Input
              label="Match Date"
              type="date"
              value={form.date}
              onChange={(v) => update("date", v)}
              required
              error={errors.date}
            />
          </div>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <Select
              label="Team coached"
              value={form.team}
              onChange={(v) => update("team", v)}
              options={TEAMS}
              required
            />
            <Select
              label="Match Type"
              value={form.matchType}
              onChange={(v) => update("matchType", v)}
              options={MATCH_TYPES}
              required
            />
          </div>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
            <AutocompleteInput
              label="Opponent"
              value={form.opponent}
              onChange={(v) => update("opponent", v)}
              suggestions={opponents}
              placeholder="Opponent team name..."
              required
            />
            <AutocompleteInput
              label="Venue / Pitch"
              value={form.venue}
              onChange={(v) => update("venue", v)}
              suggestions={venues}
              placeholder="e.g. RMIT, ERC..."
            />
          </div>
          <div>
            <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 mb-2">Match Location</label>
            <div className="flex gap-2">
              <button
                type="button"
                onClick={() => update("isHome", true)}
                className={`flex-1 flex items-center justify-center gap-2 px-4 py-2.5 rounded-md border-2 font-bold uppercase tracking-wider text-sm transition-all ${
                  form.isHome ? "bg-neutral-900 text-white border-neutral-900" : "bg-white text-neutral-500 border-neutral-200 hover:border-neutral-400"
                }`}
              >
                <Home className="w-4 h-4" /> Home
              </button>
              <button
                type="button"
                onClick={() => update("isHome", false)}
                className={`flex-1 flex items-center justify-center gap-2 px-4 py-2.5 rounded-md border-2 font-bold uppercase tracking-wider text-sm transition-all ${
                  !form.isHome ? "bg-neutral-900 text-white border-neutral-900" : "bg-white text-neutral-500 border-neutral-200 hover:border-neutral-400"
                }`}
              >
                <Plane className="w-4 h-4" /> Away
              </button>
            </div>
          </div>
        </FormSection>

        {/* SECTION 2: Score */}
        <FormSection title="Score & Result" icon={Trophy} step={2}>
          <div id="field-ourScore" className="grid grid-cols-3 gap-4 items-end">
            <div>
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 mb-1.5">
                {form.isHome ? "Fox" : "Fox (Away)"} <span className="text-red-600">*</span>
              </label>
              <input
                type="number"
                min="0"
                value={form.ourScore}
                onChange={(e) => update("ourScore", e.target.value)}
                className={`w-full text-center text-4xl font-display py-4 bg-white border-2 ${errors.ourScore ? "border-red-500" : "border-neutral-200"} rounded-md focus:border-orange-500 focus:outline-none`}
              />
            </div>
            <div className="text-center pb-4">
              <span className="font-display text-3xl text-neutral-300">VS</span>
            </div>
            <div>
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 mb-1.5 truncate">
                {form.opponent || "Opponent"} <span className="text-red-600">*</span>
              </label>
              <input
                type="number"
                min="0"
                value={form.opponentScore}
                onChange={(e) => update("opponentScore", e.target.value)}
                className={`w-full text-center text-4xl font-display py-4 bg-white border-2 ${errors.opponentScore ? "border-red-500" : "border-neutral-200"} rounded-md focus:border-orange-500 focus:outline-none`}
              />
            </div>
          </div>

          {/* Result badge */}
          {form.ourScore !== "" && form.opponentScore !== "" && (
            <div className="flex justify-center mt-3">
              {ourScoreNum > parseInt(form.opponentScore) ? (
                <Badge color="green" className="!text-sm !px-4 !py-1.5">🏆 Victory</Badge>
              ) : ourScoreNum < parseInt(form.opponentScore) ? (
                <Badge color="red" className="!text-sm !px-4 !py-1.5">Loss</Badge>
              ) : (
                <Badge color="yellow" className="!text-sm !px-4 !py-1.5">Draw</Badge>
              )}
              {parseInt(form.opponentScore) === 0 && ourScoreNum > 0 && (
                <Badge color="blue" className="!text-sm !px-4 !py-1.5 ml-2">🧤 Clean Sheet</Badge>
              )}
            </div>
          )}

          {/* Goalkeepers */}
          <div>
            <div className="flex items-center justify-between mb-2">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
                Goalkeeper(s)
              </label>
              <button type="button" onClick={() => addToList("goalkeepers")} className="text-xs font-bold text-orange-600 hover:text-orange-700 flex items-center gap-1 uppercase tracking-wider">
                <Plus className="w-3 h-3" /> Add
              </button>
            </div>
            <div className="space-y-2">
              {(form.goalkeepers || []).map((gk, i) => (
                <div key={i} className="flex gap-2">
                  <div className="flex-1">
                    <AutocompleteInput
                      value={gk.name}
                      onChange={(v) => updateInList("goalkeepers", i, "name", v)}
                      suggestions={teamPlayers}
                      placeholder="Goalkeeper name..."
                    />
                  </div>
                  <button type="button" onClick={() => removeFromList("goalkeepers", i)} className="p-2.5 text-neutral-400 hover:text-red-600 hover:bg-red-50 rounded-md transition-colors">
                    <X className="w-4 h-4" />
                  </button>
                </div>
              ))}
              {(!form.goalkeepers || form.goalkeepers.length === 0) && (
                <p className="text-xs text-neutral-400 italic">No goalkeeper added yet</p>
              )}
            </div>
          </div>
        </FormSection>

        {/* SECTION 3: Players & Performance */}
        <FormSection title="Players & Performance" icon={Users} step={3}>
          {/* Goalscorers */}
          <div>
            <div className="flex items-center justify-between mb-2">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 flex items-center gap-1.5">
                <Target className="w-3.5 h-3.5 text-orange-500" /> Goal Scorers
              </label>
              <button type="button" onClick={addGoalscorer} className="text-xs font-bold text-orange-600 hover:text-orange-700 flex items-center gap-1 uppercase tracking-wider">
                <Plus className="w-3 h-3" /> Add scorer
              </button>
            </div>
            <div className="space-y-2">
              {form.goalscorers.map((g, i) => (
                <div key={i} className="flex gap-2 items-start">
                  <div className="flex-1">
                    <AutocompleteInput
                      value={g.name}
                      onChange={(v) => updateGoalscorer(i, "name", v)}
                      suggestions={teamPlayers}
                      placeholder="Player name..."
                    />
                  </div>
                  <div className="w-28">
                    <div className="flex items-center bg-white border-2 border-neutral-200 rounded-md overflow-hidden">
                      <button type="button" onClick={() => updateGoalscorer(i, "goals", Math.max(1, (parseInt(g.goals) || 1) - 1))} className="px-3 py-2.5 text-neutral-500 hover:bg-neutral-100">
                        −
                      </button>
                      <input
                        type="number"
                        min="1"
                        value={g.goals}
                        onChange={(e) => updateGoalscorer(i, "goals", parseInt(e.target.value) || 1)}
                        className="flex-1 text-center py-2.5 focus:outline-none font-bold w-12"
                      />
                      <button type="button" onClick={() => updateGoalscorer(i, "goals", (parseInt(g.goals) || 1) + 1)} className="px-3 py-2.5 text-neutral-500 hover:bg-neutral-100">
                        +
                      </button>
                    </div>
                  </div>
                  <div className="text-2xl pt-1.5 select-none">
                    {"⚽".repeat(Math.min(parseInt(g.goals) || 0, 5))}{(parseInt(g.goals) || 0) > 5 ? "…" : ""}
                  </div>
                  <button type="button" onClick={() => removeGoalscorer(i)} className="p-2.5 text-neutral-400 hover:text-red-600 hover:bg-red-50 rounded-md transition-colors">
                    <X className="w-4 h-4" />
                  </button>
                </div>
              ))}
              {form.goalscorers.length === 0 && <p className="text-xs text-neutral-400 italic">No goal scorers yet</p>}
            </div>
            {goalsMismatch && (
              <div className="mt-2 flex items-start gap-2 p-3 bg-yellow-50 border border-yellow-200 rounded-md">
                <AlertCircle className="w-4 h-4 text-yellow-600 flex-shrink-0 mt-0.5" />
                <p className="text-xs text-yellow-800">
                  <strong>Heads up:</strong> Total goals counted ({totalGoalsCounted}) doesn't match team score ({ourScoreNum}).
                </p>
              </div>
            )}
          </div>

          {/* Assists */}
          <div>
            <div className="flex items-center justify-between mb-2">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 flex items-center gap-1.5">
                <Sparkles className="w-3.5 h-3.5 text-blue-500" /> Assists
              </label>
              <button type="button" onClick={() => addToList("assists")} className="text-xs font-bold text-orange-600 hover:text-orange-700 flex items-center gap-1 uppercase tracking-wider">
                <Plus className="w-3 h-3" /> Add assist
              </button>
            </div>
            <div className="space-y-2">
              {(form.assists || []).map((a, i) => (
                <div key={i} className="flex gap-2">
                  <div className="flex-1">
                    <AutocompleteInput
                      value={a.name}
                      onChange={(v) => updateInList("assists", i, "name", v)}
                      suggestions={teamPlayers}
                      placeholder="Player name..."
                    />
                  </div>
                  <button type="button" onClick={() => removeFromList("assists", i)} className="p-2.5 text-neutral-400 hover:text-red-600 hover:bg-red-50 rounded-md transition-colors">
                    <X className="w-4 h-4" />
                  </button>
                </div>
              ))}
              {(!form.assists || form.assists.length === 0) && <p className="text-xs text-neutral-400 italic">No assists yet</p>}
            </div>
          </div>

          {/* MVPs */}
          <div>
            <div className="flex items-center justify-between mb-2">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 flex items-center gap-1.5">
                <Star className="w-3.5 h-3.5 text-yellow-500" /> Most Valuable Player(s)
              </label>
              <button type="button" onClick={() => addToList("mvps", { name: "", comment: "" })} className="text-xs font-bold text-orange-600 hover:text-orange-700 flex items-center gap-1 uppercase tracking-wider">
                <Plus className="w-3 h-3" /> Add MVP
              </button>
            </div>
            <div className="space-y-2">
              {(form.mvps || []).map((mvp, i) => (
                <div key={i} className="flex gap-2 items-start bg-yellow-50/50 border border-yellow-200 rounded-md p-2">
                  <div className="flex-1 space-y-2">
                    <AutocompleteInput
                      value={mvp.name}
                      onChange={(v) => updateInList("mvps", i, "name", v)}
                      suggestions={teamPlayers}
                      placeholder="MVP player name..."
                    />
                    <input
                      type="text"
                      value={mvp.comment || ""}
                      onChange={(e) => updateInList("mvps", i, "comment", e.target.value)}
                      placeholder="Optional comment about their performance..."
                      className="w-full px-3 py-2 text-sm bg-white border border-neutral-200 rounded-md focus:border-orange-500 focus:outline-none"
                    />
                  </div>
                  <button type="button" onClick={() => removeFromList("mvps", i)} className="p-2 text-neutral-400 hover:text-red-600 rounded-md">
                    <X className="w-4 h-4" />
                  </button>
                </div>
              ))}
              {(!form.mvps || form.mvps.length === 0) && <p className="text-xs text-neutral-400 italic">No MVPs selected yet</p>}
            </div>
          </div>

          {/* Special Mentions */}
          <div>
            <div className="flex items-center justify-between mb-2">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700 flex items-center gap-1.5">
                <Award className="w-3.5 h-3.5 text-purple-500" /> Special Mentions
              </label>
              <button type="button" onClick={() => addToList("specialMentions", { name: "", comment: "" })} className="text-xs font-bold text-orange-600 hover:text-orange-700 flex items-center gap-1 uppercase tracking-wider">
                <Plus className="w-3 h-3" /> Add mention
              </button>
            </div>
            <div className="space-y-2">
              {(form.specialMentions || []).map((sm, i) => (
                <div key={i} className="flex gap-2 items-start">
                  <div className="flex-1 space-y-2">
                    <AutocompleteInput
                      value={sm.name}
                      onChange={(v) => updateInList("specialMentions", i, "name", v)}
                      suggestions={teamPlayers}
                      placeholder="Player name..."
                    />
                    <input
                      type="text"
                      value={sm.comment || ""}
                      onChange={(e) => updateInList("specialMentions", i, "comment", e.target.value)}
                      placeholder="Optional short comment..."
                      className="w-full px-3 py-2 text-sm bg-white border border-neutral-200 rounded-md focus:border-orange-500 focus:outline-none"
                    />
                  </div>
                  <button type="button" onClick={() => removeFromList("specialMentions", i)} className="p-2 text-neutral-400 hover:text-red-600 rounded-md">
                    <X className="w-4 h-4" />
                  </button>
                </div>
              ))}
              {(!form.specialMentions || form.specialMentions.length === 0) && <p className="text-xs text-neutral-400 italic">No special mentions yet</p>}
            </div>
          </div>

          {/* Captain */}
          <AutocompleteInput
            label="Captain of the day (optional)"
            value={form.captain}
            onChange={(v) => update("captain", v)}
            suggestions={teamPlayers}
            placeholder="Captain name..."
          />
        </FormSection>

        {/* SECTION 4: Match Analysis */}
        <FormSection title="Match Analysis" icon={FileText} step={4}>
          <div id="field-summary">
            <div className="flex items-center justify-between mb-1.5">
              <label className="block text-xs font-bold uppercase tracking-widest text-neutral-700">
                Match Summary <span className="text-red-600">*</span>
              </label>
              <button
                type="button"
                onClick={correctGrammar}
                disabled={!form.summary?.trim() || isCorrectingGrammar}
                className="flex items-center gap-1.5 text-xs font-bold uppercase tracking-wider text-orange-600 hover:text-orange-700 disabled:opacity-40"
              >
                {isCorrectingGrammar ? <Loader2 className="w-3 h-3 animate-spin" /> : <Sparkles className="w-3 h-3" />}
                {isCorrectingGrammar ? "Correcting..." : "Improve my text"}
              </button>
            </div>
            <textarea
              value={form.summary}
              onChange={(e) => update("summary", e.target.value)}
              placeholder="Write your match analysis here. Cover key moments, team performance, what went well, transitions, and overall game flow..."
              rows={6}
              className={`w-full px-4 py-3 bg-white border-2 ${errors.summary ? "border-red-500" : "border-neutral-200"} rounded-md focus:border-orange-500 focus:outline-none transition-colors text-neutral-900 placeholder:text-neutral-400 resize-y`}
            />
            {errors.summary && <p className="text-xs text-red-600 font-medium mt-1">{errors.summary}</p>}
            <p className="text-[10px] text-neutral-400 mt-1.5">
              Tip: Use the "Improve my text" button to polish grammar and syntax with AI
            </p>
          </div>

          <Textarea
            label="Areas of Improvement (optional)"
            value={form.areasOfImprovement}
            onChange={(v) => update("areasOfImprovement", v)}
            placeholder="1 or 2 points to work on next training session..."
            rows={2}
          />
          <Textarea
            label="Next Match Preview (optional)"
            value={form.nextMatchPreview}
            onChange={(v) => update("nextMatchPreview", v)}
            placeholder="Mention the upcoming game to engage parents..."
            rows={2}
          />
        </FormSection>

        {/* SECTION 5: Team Photo */}
        <FormSection title="Team Photo" icon={Camera} step={5}>
          <div id="field-teamPhoto">
            {form.teamPhoto ? (
              <div className="relative group">
                <img src={form.teamPhoto} alt="Team" className="w-full h-64 object-cover rounded-lg border-2 border-neutral-200" />
                <div className="absolute inset-0 bg-black/0 group-hover:bg-black/40 transition-colors rounded-lg flex items-center justify-center gap-2 opacity-0 group-hover:opacity-100">
                  <label className="px-4 py-2 bg-white text-neutral-900 font-bold uppercase tracking-wider text-xs rounded-md cursor-pointer flex items-center gap-2">
                    <RefreshCw className="w-3.5 h-3.5" /> Replace
                    <input type="file" accept="image/*" onChange={handlePhotoUpload} className="hidden" />
                  </label>
                  <button type="button" onClick={() => update("teamPhoto", null)} className="px-4 py-2 bg-red-600 text-white font-bold uppercase tracking-wider text-xs rounded-md flex items-center gap-2">
                    <Trash2 className="w-3.5 h-3.5" /> Remove
                  </button>
                </div>
              </div>
            ) : (
              <label className={`flex flex-col items-center justify-center h-64 border-2 border-dashed ${errors.teamPhoto ? "border-red-400" : "border-neutral-300"} rounded-lg cursor-pointer hover:border-orange-500 hover:bg-orange-50/30 transition-colors`}>
                <ImageIcon className="w-12 h-12 text-neutral-300 mb-3" />
                <p className="text-sm font-bold text-neutral-700 uppercase tracking-wider">Upload team photo</p>
                <p className="text-xs text-neutral-400 mt-1">You will be able to crop it for landscape format</p>
                <input type="file" accept="image/*" onChange={handlePhotoUpload} className="hidden" />
              </label>
            )}
            {errors.teamPhoto && <p className="text-xs text-red-600 font-medium mt-1.5">{errors.teamPhoto}</p>}
          </div>
        </FormSection>

        {/* Bottom action bar */}
        <div className="flex flex-col sm:flex-row gap-3 justify-end pt-4 pb-8">
          <Button onClick={onCancel} variant="ghost">Cancel</Button>
          <Button onClick={handleSubmit} icon={Save} size="lg">Save & Generate Report</Button>
        </div>
      </main>

      {/* Grammar correction modal */}
      <Modal
        isOpen={!!grammarComparison}
        onClose={() => setGrammarComparison(null)}
        title="✨ AI Grammar Suggestion"
        maxWidth="max-w-3xl"
      >
        {grammarComparison && (
          <div className="space-y-5">
            <p className="text-sm text-neutral-600">Compare your original text with the AI corrected version. You can accept the suggestion or keep your original.</p>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <h4 className="text-xs font-bold uppercase tracking-widest text-neutral-500 mb-2">Original</h4>
                <div className="p-4 bg-neutral-50 border border-neutral-200 rounded-lg text-sm text-neutral-800 max-h-72 overflow-y-auto whitespace-pre-wrap">
                  {grammarComparison.original}
                </div>
              </div>
              <div>
                <h4 className="text-xs font-bold uppercase tracking-widest text-orange-600 mb-2 flex items-center gap-1">
                  <Sparkles className="w-3 h-3" /> AI Corrected
                </h4>
                <div className="p-4 bg-orange-50 border border-orange-200 rounded-lg text-sm text-neutral-800 max-h-72 overflow-y-auto whitespace-pre-wrap">
                  {grammarComparison.corrected}
                </div>
              </div>
            </div>
            <div className="flex flex-col sm:flex-row gap-2 justify-end pt-2 border-t border-neutral-100">
              <Button onClick={() => setGrammarComparison(null)} variant="ghost">Keep Original</Button>
              <Button onClick={() => { update("summary", grammarComparison.corrected); setGrammarComparison(null); showToast("Text updated"); }} icon={Check}>Use Corrected</Button>
            </div>
          </div>
        )}
      </Modal>

      {/* Photo crop modal */}
      <Modal
        isOpen={showCropModal}
        onClose={() => { setShowCropModal(false); setRawPhoto(null); }}
        title="Adjust Team Photo"
        maxWidth="max-w-3xl"
      >
        {rawPhoto && (
          <PhotoCropper
            src={rawPhoto}
            onCancel={() => { setShowCropModal(false); setRawPhoto(null); }}
            onSave={(cropped) => {
              update("teamPhoto", cropped);
              setShowCropModal(false);
              setRawPhoto(null);
              showToast("Photo added");
            }}
          />
        )}
      </Modal>
    </div>
  );
}

function FormSection({ title, icon: Icon, step, children }) {
  return (
    <section className="bg-white rounded-2xl border border-neutral-200 overflow-hidden">
      <div className="px-6 py-4 border-b border-neutral-100 flex items-center gap-3">
        <div className="w-9 h-9 rounded-lg bg-gradient-to-br from-red-600 to-orange-500 flex items-center justify-center text-white font-display text-base">
          {step}
        </div>
        <div className="flex-1">
          <h2 className="font-display text-xl tracking-wider text-neutral-900 leading-none">{title}</h2>
        </div>
        <Icon className="w-5 h-5 text-neutral-300" />
      </div>
      <div className="p-6 space-y-5">{children}</div>
    </section>
  );
}


// =====================================================
// PHOTO CROPPER COMPONENT
// =====================================================

function PhotoCropper({ src, onCancel, onSave }) {
  const canvasRef = useRef(null);
  const imgRef = useRef(null);
  const [scale, setScale] = useState(1);
  const [offsetX, setOffsetX] = useState(0);
  const [offsetY, setOffsetY] = useState(0);
  const [isDragging, setIsDragging] = useState(false);
  const [dragStart, setDragStart] = useState({ x: 0, y: 0 });
  const [imgLoaded, setImgLoaded] = useState(false);

  // Output canvas dimensions (landscape format like reference)
  const OUTPUT_W = 1200;
  const OUTPUT_H = 600;
  const PREVIEW_W = 600;
  const PREVIEW_H = 300;

  // When image loads, fit it to canvas
  const handleImgLoad = () => {
    const img = imgRef.current;
    if (!img) return;
    const imgAspect = img.naturalWidth / img.naturalHeight;
    const canvasAspect = OUTPUT_W / OUTPUT_H;
    let initScale;
    if (imgAspect > canvasAspect) {
      // Image wider — fit to height
      initScale = OUTPUT_H / img.naturalHeight;
    } else {
      // Image taller — fit to width
      initScale = OUTPUT_W / img.naturalWidth;
    }
    setScale(initScale);
    setOffsetX((OUTPUT_W - img.naturalWidth * initScale) / 2);
    setOffsetY((OUTPUT_H - img.naturalHeight * initScale) / 2);
    setImgLoaded(true);
  };

  // Draw on canvas
  useEffect(() => {
    if (!imgLoaded || !canvasRef.current || !imgRef.current) return;
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = OUTPUT_W;
    canvas.height = OUTPUT_H;
    ctx.fillStyle = "#1a1a1a";
    ctx.fillRect(0, 0, OUTPUT_W, OUTPUT_H);
    ctx.drawImage(imgRef.current, offsetX, offsetY, imgRef.current.naturalWidth * scale, imgRef.current.naturalHeight * scale);
  }, [scale, offsetX, offsetY, imgLoaded]);

  // Mouse handlers (work in preview coordinates, mapped to output)
  const previewToOutput = PREVIEW_W / OUTPUT_W;

  const handleMouseDown = (e) => {
    setIsDragging(true);
    const rect = e.currentTarget.getBoundingClientRect();
    setDragStart({
      x: (e.clientX - rect.left) / previewToOutput - offsetX,
      y: (e.clientY - rect.top) / previewToOutput - offsetY
    });
  };
  const handleMouseMove = (e) => {
    if (!isDragging) return;
    const rect = e.currentTarget.getBoundingClientRect();
    setOffsetX((e.clientX - rect.left) / previewToOutput - dragStart.x);
    setOffsetY((e.clientY - rect.top) / previewToOutput - dragStart.y);
  };
  const handleMouseUp = () => setIsDragging(false);

  const handleWheel = (e) => {
    e.preventDefault();
    const delta = e.deltaY > 0 ? 0.95 : 1.05;
    setScale(prev => Math.max(0.1, Math.min(5, prev * delta)));
  };

  const handleSave = () => {
    if (!canvasRef.current) return;
    const dataUrl = canvasRef.current.toDataURL("image/jpeg", 0.85);
    onSave(dataUrl);
  };

  return (
    <div className="space-y-4">
      <p className="text-sm text-neutral-600">Drag to position, use the slider or scroll to zoom. The visible area will be saved.</p>

      {/* Preview container */}
      <div className="flex justify-center">
        <div
          className="relative bg-neutral-900 rounded-lg overflow-hidden cursor-move shadow-xl border-4 border-orange-500/30"
          style={{ width: PREVIEW_W, height: PREVIEW_H }}
          onMouseDown={handleMouseDown}
          onMouseMove={handleMouseMove}
          onMouseUp={handleMouseUp}
          onMouseLeave={handleMouseUp}
          onWheel={handleWheel}
        >
          <canvas
            ref={canvasRef}
            style={{
              width: "100%",
              height: "100%",
              imageRendering: "auto",
              pointerEvents: "none"
            }}
          />
          {/* Hidden source image */}
          <img
            ref={imgRef}
            src={src}
            alt=""
            onLoad={handleImgLoad}
            crossOrigin="anonymous"
            style={{ display: "none" }}
          />
          {/* Crop guide overlay */}
          <div className="absolute inset-0 pointer-events-none border-2 border-white/20" />
        </div>
      </div>

      {/* Zoom slider */}
      <div className="flex items-center gap-3 max-w-md mx-auto">
        <span className="text-xs font-bold text-neutral-500 uppercase tracking-wider">Zoom</span>
        <input
          type="range"
          min="0.1"
          max="3"
          step="0.05"
          value={scale}
          onChange={(e) => setScale(parseFloat(e.target.value))}
          className="flex-1 accent-orange-500"
        />
        <span className="text-xs font-mono text-neutral-500 w-10 text-right">{Math.round(scale * 100)}%</span>
      </div>

      <div className="flex gap-3 justify-end pt-2 border-t border-neutral-100">
        <Button onClick={onCancel} variant="ghost">Cancel</Button>
        <Button onClick={handleSave} icon={Crop}>Save Crop</Button>
      </div>
    </div>
  );
}


// =====================================================
// PREVIEW VIEW (final report visual + export)
// =====================================================

function PreviewView({ match, onBack, showToast }) {
  const [language, setLanguage] = useState("en"); // en | vi
  const [format, setFormat] = useState("portrait"); // portrait (1080x1350) | square (1080x1080)
  const [translatedMatch, setTranslatedMatch] = useState(null);
  const [isTranslating, setIsTranslating] = useState(false);
  const [isExporting, setIsExporting] = useState(false);
  const reportRef = useRef(null);

  // Translate match data when switching to Vietnamese
  useEffect(() => {
    if (language === "vi" && !translatedMatch) {
      translateMatch();
    }
  }, [language]);

  const translateMatch = async () => {
    setIsTranslating(true);
    try {
      const fieldsToTranslate = {
        summary: match.summary || "",
        areasOfImprovement: match.areasOfImprovement || "",
        nextMatchPreview: match.nextMatchPreview || "",
        mvpComments: (match.mvps || []).map(m => m.comment || "").join("|||"),
        mentionComments: (match.specialMentions || []).map(s => s.comment || "").join("|||")
      };

      const prompt = `Translate the following English football match report fields to Vietnamese. Maintain the warm, professional tone of a youth football coach. Preserve all player names exactly as they are (do not translate names). Do not use hyphens; use commas, periods, or "and" instead. Use proper Vietnamese diacritics. Return ONLY a JSON object with the same keys, no other text, no markdown fences.

Input JSON:
${JSON.stringify(fieldsToTranslate, null, 2)}

Return ONLY the JSON with translated values.`;

      const result = await callClaude(prompt, "You are a professional English to Vietnamese translator specialized in youth sports content.");
      const cleaned = result.replace(/```json|```/g, "").trim();
      const parsed = JSON.parse(cleaned);

      const mvpComments = (parsed.mvpComments || "").split("|||");
      const mentionComments = (parsed.mentionComments || "").split("|||");

      setTranslatedMatch({
        ...match,
        summary: parsed.summary || match.summary,
        areasOfImprovement: parsed.areasOfImprovement || match.areasOfImprovement,
        nextMatchPreview: parsed.nextMatchPreview || match.nextMatchPreview,
        mvps: (match.mvps || []).map((m, i) => ({ ...m, comment: mvpComments[i] || m.comment })),
        specialMentions: (match.specialMentions || []).map((s, i) => ({ ...s, comment: mentionComments[i] || s.comment }))
      });
    } catch (e) {
      console.error("Translation error:", e);
      showToast("Translation failed, showing English version", "error");
      setLanguage("en");
    } finally {
      setIsTranslating(false);
    }
  };

  // Export report as PNG using html2canvas
  const exportPNG = async () => {
    setIsExporting(true);
    try {
      if (!window.html2canvas) {
        await new Promise((resolve, reject) => {
          const script = document.createElement("script");
          script.src = "https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js";
          script.onload = resolve;
          script.onerror = reject;
          document.head.appendChild(script);
        });
      }

      const node = reportRef.current;
      if (!node) return;

      // Target dimensions for social media (high res)
      const targetW = 1080;
      const targetH = format === "portrait" ? 1350 : 1080;
      // We render at 540 wide on screen, html2canvas with scale=2 gives us 1080
      const renderedW = node.offsetWidth;
      const exportScale = targetW / renderedW;

      const canvas = await window.html2canvas(node, {
        scale: exportScale,
        backgroundColor: "#ffffff",
        useCORS: true,
        allowTaint: true,
        logging: false,
        width: renderedW,
        height: node.offsetHeight
      });

      const link = document.createElement("a");
      const langSuffix = language === "en" ? "EN" : "VI";
      const formatSuffix = format === "portrait" ? "Portrait" : "Square";
      link.download = `FFV_${match.team.replace(/[^a-z0-9]/gi, "")}_Match${match.matchNumber}_${langSuffix}_${formatSuffix}.png`;
      link.href = canvas.toDataURL("image/png");
      link.click();
      showToast(`Report exported (${langSuffix}, ${formatSuffix})`);
    } catch (e) {
      console.error("Export error:", e);
      showToast("Export failed", "error");
    } finally {
      setIsExporting(false);
    }
  };

  const displayMatch = language === "vi" && translatedMatch ? translatedMatch : match;

  return (
    <div className="min-h-screen bg-neutral-100">
      {/* Header */}
      <header className="bg-neutral-950 border-b-4 border-orange-500 sticky top-0 z-30">
        <div className="max-w-6xl mx-auto px-6 py-4 flex items-center justify-between flex-wrap gap-3">
          <div className="flex items-center gap-3">
            <button onClick={onBack} className="p-2 -ml-2 text-neutral-400 hover:text-white transition-colors">
              <ChevronLeft className="w-5 h-5" />
            </button>
            <div>
              <h1 className="font-display text-white text-xl tracking-wider leading-none">REPORT PREVIEW</h1>
              <p className="text-neutral-500 text-[10px] uppercase tracking-[0.25em]">
                {match.team} · Match #{match.matchNumber}
              </p>
            </div>
          </div>
          <div className="flex items-center gap-2 flex-wrap">
            {/* Format toggle */}
            <div className="flex bg-white/5 rounded-lg p-1 border border-white/10">
              <button
                onClick={() => setFormat("portrait")}
                title="Portrait 1080x1350 — best for Instagram feed"
                className={`px-3 py-1.5 text-[10px] font-bold uppercase tracking-wider rounded transition-colors ${
                  format === "portrait" ? "bg-orange-500 text-white" : "text-neutral-400 hover:text-white"
                }`}
              >
                📱 4:5
              </button>
              <button
                onClick={() => setFormat("square")}
                title="Square 1080x1080 — best for Facebook & IG"
                className={`px-3 py-1.5 text-[10px] font-bold uppercase tracking-wider rounded transition-colors ${
                  format === "square" ? "bg-orange-500 text-white" : "text-neutral-400 hover:text-white"
                }`}
              >
                ⬛ 1:1
              </button>
            </div>
            {/* Language toggle */}
            <div className="flex bg-white/5 rounded-lg p-1 border border-white/10">
              <button
                onClick={() => setLanguage("en")}
                className={`px-3 py-1.5 text-xs font-bold uppercase tracking-wider rounded transition-colors ${
                  language === "en" ? "bg-orange-500 text-white" : "text-neutral-400 hover:text-white"
                }`}
              >
                🇬🇧 EN
              </button>
              <button
                onClick={() => setLanguage("vi")}
                disabled={isTranslating}
                className={`px-3 py-1.5 text-xs font-bold uppercase tracking-wider rounded transition-colors flex items-center gap-1 ${
                  language === "vi" ? "bg-orange-500 text-white" : "text-neutral-400 hover:text-white"
                }`}
              >
                {isTranslating && <Loader2 className="w-3 h-3 animate-spin" />}
                🇻🇳 VI
              </button>
            </div>
            <Button onClick={exportPNG} icon={Download} disabled={isExporting || isTranslating}>
              {isExporting ? "Exporting..." : "Download PNG"}
            </Button>
          </div>
        </div>
      </header>

      <main className="max-w-3xl mx-auto px-6 py-8">
        <div className="flex flex-col items-center">
          {isTranslating && language === "vi" ? (
            <div className="bg-white rounded-2xl shadow-2xl p-20 text-center" style={{ width: 540 }}>
              <Loader2 className="w-10 h-10 text-orange-500 animate-spin mx-auto mb-3" />
              <p className="text-neutral-500 text-sm font-medium">Translating to Vietnamese...</p>
            </div>
          ) : (
            <div
              ref={reportRef}
              className={language === "vi" ? "lang-vi" : ""}
              style={{
                width: 540,
                height: format === "portrait" ? 675 : 540, // 540x675 = 1080x1350 / 2 ; 540x540 = 1080x1080/2
                boxShadow: "0 25px 50px -12px rgba(0,0,0,0.25)",
                overflow: "hidden",
                position: "relative",
                background: "white"
              }}
            >
              <MatchReportCard match={displayMatch} language={language} format={format} />
            </div>
          )}
        </div>

        <div className="mt-6 flex flex-col items-center gap-2 text-xs text-neutral-500 text-center">
          <div className="flex items-center gap-2">
            <Sparkles className="w-3.5 h-3.5 text-orange-500" />
            <span>Output resolution: <strong>{format === "portrait" ? "1080 × 1350" : "1080 × 1080"} px</strong> · {format === "portrait" ? "Best for Instagram feed" : "Best for Facebook & IG"}</span>
          </div>
          <p className="text-[11px] text-neutral-400">Toggle EN/VI and download both versions for English and Vietnamese parents</p>
        </div>
      </main>
    </div>
  );
}

// =====================================================
// MATCH REPORT CARD (the actual visual that gets exported)
// Optimized for Instagram/Facebook: 1080x1350 (portrait) or 1080x1080 (square)
// Rendered at 540x675 or 540x540 in preview, exported at 2x
// =====================================================

function MatchReportCard({ match, language = "en", format = "portrait" }) {
  const ourScore = parseInt(match.ourScore) || 0;
  const oppScore = parseInt(match.opponentScore) || 0;
  const isWin = ourScore > oppScore;
  const isDraw = ourScore === oppScore;
  const isCleanSheet = oppScore === 0 && ourScore > 0;
  const isPortrait = format === "portrait";

  const t = (en, vi) => language === "vi" ? vi : en;

  const dateStr = new Date(match.date).toLocaleDateString(language === "vi" ? "vi-VN" : "en-GB", {
    day: "numeric", month: "short", year: "numeric"
  });

  const resultBadge = isWin
    ? { text: t("VICTORY", "CHIẾN THẮNG"), bg: "bg-green-600" }
    : isDraw
    ? { text: t("DRAW", "HÒA"), bg: "bg-yellow-500" }
    : { text: t("LOSS", "THUA"), bg: "bg-red-600" };

  // Score arrangement: home team always on left in display
  // FOX team is labeled with the actual team name (e.g., "FOX U14B")
  const foxLabel = `FOX ${match.team || ""}`.trim();
  const homeLabel = match.isHome ? foxLabel : (match.opponent || "OPP");
  const awayLabel = match.isHome ? (match.opponent || "OPP") : foxLabel;
  const homeScore = match.isHome ? ourScore : oppScore;
  const awayScore = match.isHome ? oppScore : ourScore;

  // Helper to determine if a team label is the Fox team (for highlighting)
  const isHomeOurs = match.isHome;

  return (
    <div className="absolute inset-0 flex flex-col bg-gradient-to-br from-neutral-50 via-orange-50/20 to-neutral-100 overflow-hidden">
      {/* Decorative background grain */}
      <div className="absolute inset-0 opacity-[0.05] pointer-events-none mix-blend-multiply"
           style={{
             backgroundImage: `url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='3'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E")`
           }} />

      {/* Diagonal accent stripes */}
      <div className="absolute -top-12 -right-12 w-48 h-48 bg-gradient-to-br from-red-600/10 to-orange-500/10 rotate-12 pointer-events-none" />
      <div className="absolute -bottom-12 -left-12 w-48 h-48 bg-gradient-to-tr from-red-600/5 to-orange-500/10 -rotate-12 pointer-events-none" />

      {/* ============ HEADER STRIP ============ */}
      <div className="relative bg-neutral-950 px-5 py-3 flex-shrink-0">
        <div className="absolute inset-x-0 bottom-0 h-1 bg-gradient-to-r from-red-600 via-orange-500 to-red-600" />
        <div className="flex items-center justify-between gap-3 relative">
          <div className="flex items-center gap-2.5">
            <div className="w-10 h-10 rounded-full bg-black flex items-center justify-center flex-shrink-0 ring-2 ring-orange-500/40 overflow-hidden">
              <FoxLogoMark className="w-9 h-9" />
            </div>
            <div className="leading-none">
              <h1 className="font-display text-white text-base tracking-wider leading-none">FOX FOOTBALL</h1>
              <p className="text-orange-500 text-[8px] uppercase tracking-[0.3em] font-bold mt-0.5">VIETNAM · {t("MATCH REPORT", "BÁO CÁO TRẬN ĐẤU")}</p>
            </div>
          </div>
          <div className="text-right leading-none">
            <p className="text-[7px] text-neutral-400 uppercase tracking-widest font-bold mb-0.5">{match.matchType?.toUpperCase()}</p>
            <p className="font-display text-orange-500 text-sm tracking-wider">MATCH #{match.matchNumber}</p>
          </div>
        </div>
      </div>

      {/* ============ SCOREBOARD ============ */}
      <div className="relative px-4 py-4 bg-gradient-to-b from-neutral-900 to-neutral-950 text-white flex-shrink-0">
        <div className="grid grid-cols-7 items-center gap-2">
          {/* Home team */}
          <div className="col-span-3 text-right">
            <p className="text-[7px] uppercase tracking-[0.25em] text-neutral-400 font-bold mb-0.5">
              {match.isHome ? t("HOME", "SÂN NHÀ") : t("AWAY", "SÂN KHÁCH")}
            </p>
            <h2 className={`font-display tracking-wider leading-none break-words ${homeLabel.length > 12 ? 'text-base' : 'text-xl'} ${isHomeOurs ? 'text-orange-400' : 'text-white'}`}>
              {homeLabel}
            </h2>
          </div>
          {/* Score */}
          <div className="col-span-1 text-center">
            <div className="font-display text-4xl text-white leading-none flex items-center justify-center gap-1">
              <span className={isHomeOurs ? "text-orange-500" : ""}>{homeScore}</span>
              <span className="text-neutral-700 text-2xl">·</span>
              <span className={!isHomeOurs ? "text-orange-500" : ""}>{awayScore}</span>
            </div>
          </div>
          {/* Away team */}
          <div className="col-span-3 text-left">
            <p className="text-[7px] uppercase tracking-[0.25em] text-neutral-400 font-bold mb-0.5">
              {match.isHome ? t("AWAY", "SÂN KHÁCH") : t("HOME", "SÂN NHÀ")}
            </p>
            <h2 className={`font-display tracking-wider leading-none break-words ${awayLabel.length > 12 ? 'text-base' : 'text-xl'} ${!isHomeOurs ? 'text-orange-400' : 'text-white'}`}>
              {awayLabel}
            </h2>
          </div>
        </div>

        {/* Result badges */}
        <div className="flex items-center justify-center gap-1.5 mt-2.5 flex-wrap">
          <span className={`${resultBadge.bg} text-white px-2 py-0.5 text-[8px] font-bold uppercase tracking-[0.2em] rounded`}>
            {resultBadge.text}
          </span>
          {isCleanSheet && (
            <span className="bg-blue-600 text-white px-2 py-0.5 text-[8px] font-bold uppercase tracking-[0.2em] rounded">
              🧤 {t("CLEAN SHEET", "GIỮ SẠCH LƯỚI")}
            </span>
          )}
        </div>

        {/* Match meta */}
        <div className="flex items-center justify-center gap-3 mt-2 text-[8px] uppercase tracking-widest text-neutral-400 font-bold flex-wrap">
          <span className="flex items-center gap-1"><Calendar className="w-2.5 h-2.5" />{dateStr}</span>
          {match.venue && <span className="flex items-center gap-1"><MapPin className="w-2.5 h-2.5" />{match.venue}</span>}
        </div>
      </div>

      {/* ============ MAIN CONTENT (flexible) ============ */}
      <div className="flex-1 grid grid-cols-5 min-h-0 overflow-hidden">
        {/* LEFT: Summary block */}
        <div className="col-span-3 bg-gradient-to-br from-orange-500 to-red-500 p-4 text-white relative overflow-hidden">
          <div className="absolute top-2 right-2 opacity-10">
            <FoxLogoMark className="w-16 h-16" />
          </div>

          <div className="relative h-full flex flex-col">
            <h3 className="font-display text-base tracking-wider mb-1.5 flex items-center gap-1.5 flex-shrink-0">
              <FileText className="w-3.5 h-3.5" />
              {t("MATCH RECAP", "TÓM TẮT TRẬN ĐẤU")}
            </h3>
            <p className={`font-serif-italic ${isPortrait ? 'text-[11px]' : 'text-[10px]'} leading-snug text-white/95 mb-2 whitespace-pre-wrap flex-shrink overflow-hidden`}
               style={{
                 display: '-webkit-box',
                 WebkitLineClamp: isPortrait ? 12 : 8,
                 WebkitBoxOrient: 'vertical'
               }}>
              {match.summary}
            </p>

            {match.areasOfImprovement && (
              <div className="mb-1.5 pb-1.5 border-b border-white/20 flex-shrink-0">
                <p className="text-[7px] uppercase tracking-[0.2em] font-bold mb-0.5 text-white/70">
                  {t("AREAS OF IMPROVEMENT", "ĐIỂM CẦN CẢI THIỆN")}
                </p>
                <p className="text-[9px] text-white/90 leading-tight"
                   style={{
                     display: '-webkit-box',
                     WebkitLineClamp: 2,
                     WebkitBoxOrient: 'vertical',
                     overflow: 'hidden'
                   }}>{match.areasOfImprovement}</p>
              </div>
            )}

            {match.nextMatchPreview && (
              <div className="mb-1.5 pb-1.5 border-b border-white/20 flex-shrink-0">
                <p className="text-[7px] uppercase tracking-[0.2em] font-bold mb-0.5 text-white/70 flex items-center gap-1">
                  <Calendar className="w-2 h-2" /> {t("NEXT MATCH", "TRẬN TIẾP THEO")}
                </p>
                <p className="text-[9px] text-white/90 leading-tight"
                   style={{
                     display: '-webkit-box',
                     WebkitLineClamp: 2,
                     WebkitBoxOrient: 'vertical',
                     overflow: 'hidden'
                   }}>{match.nextMatchPreview}</p>
              </div>
            )}

            {/* Spacer to push footer down */}
            <div className="flex-1" />

            <div className="flex items-end justify-between mt-2 flex-shrink-0">
              <p className="font-display text-sm tracking-widest text-white leading-none">
                1, 2, 3...<br/><span className="text-yellow-300 text-base">FOX!</span> 🦊
              </p>
              <div className="text-right">
                <p className="text-[7px] uppercase tracking-[0.2em] text-white/70 leading-none mb-0.5">{t("COACH", "HUẤN LUYỆN VIÊN")}</p>
                <p className="font-display text-xs tracking-widest text-white leading-none">
                  {match.coach?.toUpperCase()}
                </p>
              </div>
            </div>
          </div>
        </div>

        {/* RIGHT: Stats cards */}
        <div className="col-span-2 bg-neutral-50 p-2.5 space-y-2 overflow-hidden">
          {/* MVP */}
          {match.mvps && match.mvps.length > 0 && (
            <div className="bg-gradient-to-br from-yellow-300 to-yellow-400 rounded-md p-2 shadow-sm">
              <div className="flex items-center gap-1 mb-0.5">
                <Star className="w-2.5 h-2.5 text-neutral-900 fill-neutral-900" />
                <h4 className="font-display text-[11px] tracking-wider text-neutral-900 leading-none">{t("MVP", "MVP")}</h4>
              </div>
              <div className="space-y-0.5">
                {match.mvps.slice(0, isPortrait ? 3 : 2).map((mvp, i) => (
                  <div key={i}>
                    <p className="font-bold text-neutral-900 text-[10px] leading-tight">{mvp.name}</p>
                    {mvp.comment && isPortrait && (
                      <p className="text-[8px] text-neutral-800 italic leading-tight"
                         style={{ display: '-webkit-box', WebkitLineClamp: 2, WebkitBoxOrient: 'vertical', overflow: 'hidden' }}>
                        {mvp.comment}
                      </p>
                    )}
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* Goalscorers */}
          {match.goalscorers && match.goalscorers.length > 0 && (
            <div className="bg-white border border-orange-300 rounded-md p-2 shadow-sm">
              <div className="flex items-center gap-1 mb-0.5">
                <Target className="w-2.5 h-2.5 text-orange-600" />
                <h4 className="font-display text-[11px] tracking-wider text-neutral-900 leading-none">{t("GOALSCORERS", "GHI BÀN")}</h4>
              </div>
              <div className="space-y-0">
                {match.goalscorers.slice(0, isPortrait ? 6 : 4).map((g, i) => (
                  <p key={i} className="text-[10px] text-neutral-900 font-medium leading-tight">
                    {g.name} {"⚽".repeat(Math.min(parseInt(g.goals) || 1, 4))}
                  </p>
                ))}
              </div>
            </div>
          )}

          {/* Assists */}
          {match.assists && match.assists.length > 0 && (
            <div className="bg-white border border-blue-300 rounded-md p-2 shadow-sm">
              <div className="flex items-center gap-1 mb-0.5">
                <Sparkles className="w-2.5 h-2.5 text-blue-600" />
                <h4 className="font-display text-[10px] tracking-wider text-neutral-900 leading-none">{t("ASSISTS", "KIẾN TẠO")}</h4>
              </div>
              <p className="text-[9px] text-neutral-900 leading-tight"
                 style={{ display: '-webkit-box', WebkitLineClamp: isPortrait ? 3 : 2, WebkitBoxOrient: 'vertical', overflow: 'hidden' }}>
                {match.assists.map(a => a.name).join(", ")} 🎯
              </p>
            </div>
          )}

          {/* Clean sheet */}
          {isCleanSheet && match.goalkeepers && match.goalkeepers.length > 0 && (
            <div className="bg-blue-600 text-white rounded-md p-2 shadow-sm">
              <div className="flex items-center gap-1 mb-0.5">
                <Shield className="w-2.5 h-2.5" />
                <h4 className="font-display text-[10px] tracking-wider leading-none">🧤 {t("CLEAN SHEET", "GIỮ SẠCH LƯỚI")}</h4>
              </div>
              <p className="text-[10px] font-bold leading-tight">
                {match.goalkeepers.map(gk => gk.name).join(", ")}
              </p>
            </div>
          )}

          {/* Captain */}
          {match.captain && isPortrait && (
            <div className="bg-neutral-900 text-white rounded-md p-1.5 shadow-sm">
              <p className="text-[7px] uppercase tracking-[0.2em] text-neutral-400 font-bold mb-0">
                © {t("CAPTAIN", "ĐỘI TRƯỞNG")}
              </p>
              <p className="font-bold text-[10px] leading-tight">{match.captain}</p>
            </div>
          )}

          {/* Special mentions */}
          {match.specialMentions && match.specialMentions.length > 0 && isPortrait && (
            <div className="bg-white border border-purple-300 rounded-md p-2 shadow-sm">
              <div className="flex items-center gap-1 mb-0.5">
                <Award className="w-2.5 h-2.5 text-purple-600" />
                <h4 className="font-display text-[10px] tracking-wider text-neutral-900 leading-none">{t("MENTIONS", "GHI NHẬN")}</h4>
              </div>
              <div className="space-y-0">
                {match.specialMentions.slice(0, 3).map((sm, i) => (
                  <p key={i} className="text-[9px] text-neutral-900 leading-tight">
                    👏 {sm.name}
                  </p>
                ))}
              </div>
            </div>
          )}
        </div>
      </div>

      {/* ============ TEAM PHOTO ============ */}
      {match.teamPhoto && (
        <div
          className="relative flex-shrink-0 overflow-hidden bg-neutral-900"
          style={{ height: isPortrait ? 140 : 100 }}
        >
          <img
            src={match.teamPhoto}
            alt="Team"
            crossOrigin="anonymous"
            style={{ width: "100%", height: "100%", objectFit: "cover", display: "block" }}
          />
        </div>
      )}

      {/* ============ FOOTER ============ */}
      <div className="bg-neutral-950 px-4 py-1.5 flex items-center justify-between flex-shrink-0">
        <p className="text-[7px] text-neutral-500 uppercase tracking-[0.3em] font-bold">
          @foxfootballvietnam
        </p>
        <p className="text-[7px] text-orange-500 uppercase tracking-[0.3em] font-bold">
          {t("BUILDING CHAMPIONS", "ĐÀO TẠO NHÀ VÔ ĐỊCH")} 🦊
        </p>
      </div>
    </div>
  );
}

