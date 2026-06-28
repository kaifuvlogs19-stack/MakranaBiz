import { useState, useEffect, useRef } from "react";

// ─── DESIGN TOKENS ───────────────────────────────────────────────────────────
// Palette rooted in Makrana marble: white stone, warm sand, deep charcoal, gold vein
// #F7F4EF — marble white (bg)
// #1A1714 — deep charcoal (text)
// #C9A84C — gold vein (accent / CTAs)
// #E8E0D0 — warm sand (cards, borders)
// #5C4A2A — deep amber (secondary text, icons)
// #2D5016 — forest green (verified badge)
// Signature: marble-grain hero texture via CSS + gold vein dividers

const COLORS = {
  bg: "#F7F4EF",
  dark: "#1A1714",
  gold: "#C9A84C",
  sand: "#E8E0D0",
  amber: "#5C4A2A",
  green: "#2D5016",
  white: "#FFFFFF",
  gray: "#8B7E6E",
};

// ─── DATA ────────────────────────────────────────────────────────────────────
const CATEGORIES = [
  { id: 1, name: "Marble Factories", slug: "marble-factories", icon: "🏭", count: 240 },
  { id: 2, name: "Marble Exporters", slug: "marble-exporters", icon: "🚢", count: 87 },
  { id: 3, name: "Marble Showrooms", slug: "marble-showrooms", icon: "🏛️", count: 134 },
  { id: 4, name: "Tile Shops", slug: "tile-shops", icon: "🔲", count: 56 },
  { id: 5, name: "Hotels", slug: "hotels", icon: "🏨", count: 43 },
  { id: 6, name: "Restaurants", slug: "restaurants", icon: "🍽️", count: 78 },
  { id: 7, name: "Hospitals", slug: "hospitals", icon: "🏥", count: 19 },
  { id: 8, name: "Schools & Colleges", slug: "education", icon: "🎓", count: 31 },
  { id: 9, name: "Clothing Shops", slug: "clothing", icon: "👗", count: 92 },
  { id: 10, name: "Shoe Stores", slug: "shoes", icon: "👟", count: 44 },
  { id: 11, name: "Salons & Beauty", slug: "salons", icon: "💇", count: 67 },
  { id: 12, name: "Cafes", slug: "cafes", icon: "☕", count: 28 },
];

const BUSINESSES = [
  {
    id: 1,
    name: "Raj Marble Industries",
    slug: "raj-marble-industries",
    category: "Marble Factories",
    categorySlug: "marble-factories",
    description: "Premium white Makrana marble manufacturer since 1978. Supplier to Taj Mahal restoration projects and global exporters.",
    address: "Industrial Area, Makrana, Rajasthan 341505",
    phone: "+91 98290 12345",
    whatsapp: "9829012345",
    rating: 4.8,
    reviewCount: 124,
    verified: true,
    featured: true,
    tags: ["White Marble", "Export Quality", "Wholesale"],
    image: null,
    hours: "Mon–Sat 9AM–6PM",
    established: 1978,
  },
  {
    id: 2,
    name: "Sharma Stone Exports",
    slug: "sharma-stone-exports",
    category: "Marble Exporters",
    categorySlug: "marble-exporters",
    description: "ISO-certified marble exporter with clients in USA, UAE, Italy, and Australia. Specializing in custom cuts.",
    address: "Export Zone, Makrana, Rajasthan",
    phone: "+91 94140 67890",
    whatsapp: "9414067890",
    rating: 4.6,
    reviewCount: 89,
    verified: true,
    featured: true,
    tags: ["ISO Certified", "International", "Custom Cuts"],
    image: null,
    hours: "Mon–Fri 10AM–5PM",
    established: 1995,
  },
  {
    id: 3,
    name: "Hotel White Palace",
    slug: "hotel-white-palace",
    category: "Hotels",
    categorySlug: "hotels",
    description: "3-star business hotel in the heart of Makrana. Marble-themed interiors, 40 rooms, conference hall.",
    address: "Main Market, Makrana, Rajasthan",
    phone: "+91 01586 224455",
    whatsapp: "9829099999",
    rating: 4.3,
    reviewCount: 201,
    verified: true,
    featured: false,
    tags: ["AC Rooms", "Conference Hall", "WiFi"],
    image: null,
    hours: "24 Hours",
    established: 2008,
  },
  {
    id: 4,
    name: "Makrana Marble Showroom",
    slug: "makrana-marble-showroom",
    category: "Marble Showrooms",
    categorySlug: "marble-showrooms",
    description: "Largest marble showroom in Makrana. 5000+ sq ft display area with 200+ varieties including Italian, Kishangarh, and local white.",
    address: "NH-58, Makrana Bypass, Rajasthan",
    phone: "+91 94600 11223",
    whatsapp: "9460011223",
    rating: 4.7,
    reviewCount: 67,
    verified: true,
    featured: true,
    tags: ["200+ Varieties", "Home Delivery", "EMI Available"],
    image: null,
    hours: "Mon–Sun 8AM–8PM",
    established: 2001,
  },
  {
    id: 5,
    name: "Spice Garden Restaurant",
    slug: "spice-garden-restaurant",
    category: "Restaurants",
    categorySlug: "restaurants",
    description: "Authentic Rajasthani cuisine with live folk music on weekends. Dal Baati Churma specialists.",
    address: "Near Bus Stand, Makrana",
    phone: "+91 94141 55678",
    whatsapp: "9414155678",
    rating: 4.4,
    reviewCount: 312,
    verified: false,
    featured: false,
    tags: ["Rajasthani Food", "Pure Veg", "Live Music"],
    image: null,
    hours: "Daily 8AM–10PM",
    established: 2012,
  },
  {
    id: 6,
    name: "City Hospital & Research Centre",
    slug: "city-hospital",
    category: "Hospitals",
    categorySlug: "hospitals",
    description: "Multi-specialty hospital with 24/7 emergency, ICU, radiology, and specialist OPD. 80-bed facility.",
    address: "Station Road, Makrana, Rajasthan",
    phone: "+91 01586 220100",
    whatsapp: "9829033333",
    rating: 4.5,
    reviewCount: 445,
    verified: true,
    featured: false,
    tags: ["24/7 Emergency", "ICU", "Multi-Specialty"],
    image: null,
    hours: "24 Hours",
    established: 2003,
  },
];

const STATS = [
  { label: "Businesses Listed", value: "1,200+", icon: "🏢" },
  { label: "Marble Industries", value: "400+", icon: "⛏️" },
  { label: "Monthly Visitors", value: "50,000+", icon: "👥" },
  { label: "Cities (Soon)", value: "All India", icon: "🗺️" },
];

// ─── COMPONENTS ──────────────────────────────────────────────────────────────

function StarRating({ rating, size = 14 }) {
  return (
    <span style={{ display: "inline-flex", gap: 2, alignItems: "center" }}>
      {[1, 2, 3, 4, 5].map((s) => (
        <span key={s} style={{ fontSize: size, color: s <= Math.round(rating) ? COLORS.gold : COLORS.sand }}>★</span>
      ))}
    </span>
  );
}

function Badge({ children, variant = "default" }) {
  const styles = {
    default: { background: COLORS.sand, color: COLORS.amber },
    verified: { background: "#E8F5E9", color: COLORS.green },
    featured: { background: "#FFF8E1", color: "#B8860B" },
    category: { background: COLORS.dark, color: COLORS.gold },
  };
  return (
    <span style={{
      ...styles[variant],
      padding: "2px 10px",
      borderRadius: 20,
      fontSize: 11,
      fontWeight: 700,
      letterSpacing: "0.04em",
      textTransform: "uppercase",
    }}>{children}</span>
  );
}

function BusinessCard({ biz, onClick }) {
  const [hovered, setHovered] = useState(false);
  return (
    <div
      onClick={() => onClick(biz)}
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      style={{
        background: COLORS.white,
        border: `1.5px solid ${hovered ? COLORS.gold : COLORS.sand}`,
        borderRadius: 16,
        overflow: "hidden",
        cursor: "pointer",
        transition: "all 0.22s ease",
        transform: hovered ? "translateY(-4px)" : "translateY(0)",
        boxShadow: hovered ? "0 12px 40px rgba(201,168,76,0.18)" : "0 2px 12px rgba(0,0,0,0.06)",
      }}
    >
      {/* Image area */}
      <div style={{
        height: 140,
        background: `linear-gradient(135deg, #e8e0d0 0%, #d4c9b0 50%, #c8bda0 100%)`,
        position: "relative",
        display: "flex",
        alignItems: "center",
        justifyContent: "center",
        overflow: "hidden",
      }}>
        <span style={{ fontSize: 48, opacity: 0.5 }}>
          {CATEGORIES.find(c => c.slug === biz.categorySlug)?.icon || "🏢"}
        </span>
        <div style={{ position: "absolute", top: 12, left: 12, display: "flex", gap: 6 }}>
          {biz.verified && <Badge variant="verified">✓ Verified</Badge>}
          {biz.featured && <Badge variant="featured">⭐ Featured</Badge>}
        </div>
      </div>

      {/* Content */}
      <div style={{ padding: "16px 18px 18px" }}>
        <div style={{ marginBottom: 4 }}>
          <Badge variant="category">{biz.category}</Badge>
        </div>
        <h3 style={{
          fontSize: 16,
          fontWeight: 700,
          color: COLORS.dark,
          margin: "8px 0 4px",
          fontFamily: "'Georgia', serif",
          lineHeight: 1.3,
        }}>{biz.name}</h3>
        <p style={{
          fontSize: 13,
          color: COLORS.gray,
          margin: "0 0 10px",
          lineHeight: 1.5,
          display: "-webkit-box",
          WebkitLineClamp: 2,
          WebkitBoxOrient: "vertical",
          overflow: "hidden",
        }}>{biz.description}</p>

        <div style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 10 }}>
          <StarRating rating={biz.rating} />
          <span style={{ fontSize: 13, color: COLORS.dark, fontWeight: 700 }}>{biz.rating}</span>
          <span style={{ fontSize: 12, color: COLORS.gray }}>({biz.reviewCount} reviews)</span>
        </div>

        <div style={{ display: "flex", gap: 6, flexWrap: "wrap", marginBottom: 12 }}>
          {biz.tags.slice(0, 2).map(t => (
            <span key={t} style={{
              background: COLORS.bg,
              border: `1px solid ${COLORS.sand}`,
              color: COLORS.amber,
              fontSize: 11,
              padding: "3px 8px",
              borderRadius: 6,
              fontWeight: 600,
            }}>{t}</span>
          ))}
        </div>

        <div style={{ display: "flex", alignItems: "center", gap: 6, marginBottom: 12 }}>
          <span style={{ fontSize: 12 }}>📍</span>
          <span style={{ fontSize: 12, color: COLORS.gray, flex: 1, overflow: "hidden", textOverflow: "ellipsis", whiteSpace: "nowrap" }}>
            {biz.address}
          </span>
        </div>

        <div style={{ display: "flex", gap: 8 }}>
          <button
            onClick={e => { e.stopPropagation(); }}
            style={{
              flex: 1,
              background: COLORS.gold,
              color: COLORS.dark,
              border: "none",
              borderRadius: 8,
              padding: "9px 0",
              fontSize: 13,
              fontWeight: 700,
              cursor: "pointer",
            }}
          >📞 Call</button>
          <button
            onClick={e => { e.stopPropagation(); }}
            style={{
              flex: 1,
              background: "#25D366",
              color: COLORS.white,
              border: "none",
              borderRadius: 8,
              padding: "9px 0",
              fontSize: 13,
              fontWeight: 700,
              cursor: "pointer",
            }}
          >💬 WhatsApp</button>
        </div>
      </div>
    </div>
  );
}

function BusinessModal({ biz, onClose }) {
  const [activeTab, setActiveTab] = useState("overview");
  const [enquiryForm, setEnquiryForm] = useState({ name: "", phone: "", message: "" });
  const [submitted, setSubmitted] = useState(false);

  const tabs = ["overview", "products", "reviews", "enquiry"];

  return (
    <div style={{
      position: "fixed", inset: 0,
      background: "rgba(26,23,20,0.7)",
      zIndex: 1000,
      display: "flex",
      alignItems: "flex-end",
      justifyContent: "center",
      padding: "0",
      backdropFilter: "blur(4px)",
    }} onClick={onClose}>
      <div
        onClick={e => e.stopPropagation()}
        style={{
          background: COLORS.white,
          width: "100%",
          maxWidth: 720,
          maxHeight: "92vh",
          borderRadius: "20px 20px 0 0",
          overflow: "hidden",
          display: "flex",
          flexDirection: "column",
        }}
      >
        {/* Header */}
        <div style={{
          background: `linear-gradient(135deg, #1A1714 0%, #2C2420 100%)`,
          padding: "24px 24px 20px",
          position: "relative",
        }}>
          <button onClick={onClose} style={{
            position: "absolute", top: 16, right: 16,
            background: "rgba(255,255,255,0.15)", border: "none",
            color: "#fff", borderRadius: "50%", width: 32, height: 32,
            fontSize: 18, cursor: "pointer", display: "flex", alignItems: "center", justifyContent: "center",
          }}>×</button>

          <div style={{ display: "flex", gap: 16, alignItems: "flex-start" }}>
            <div style={{
              width: 64, height: 64, borderRadius: 14,
              background: "linear-gradient(135deg, #C9A84C, #8B6914)",
              display: "flex", alignItems: "center", justifyContent: "center",
              fontSize: 28, flexShrink: 0,
            }}>
              {CATEGORIES.find(c => c.slug === biz.categorySlug)?.icon || "🏢"}
            </div>
            <div style={{ flex: 1 }}>
              <div style={{ display: "flex", gap: 6, marginBottom: 6, flexWrap: "wrap" }}>
                {biz.verified && <Badge variant="verified">✓ Verified</Badge>}
                {biz.featured && <Badge variant="featured">⭐ Featured</Badge>}
                <Badge variant="category">{biz.category}</Badge>
              </div>
              <h2 style={{ color: "#fff", fontSize: 20, fontWeight: 800, margin: "0 0 4px", fontFamily: "'Georgia', serif" }}>
                {biz.name}
              </h2>
              <div style={{ display: "flex", alignItems: "center", gap: 8 }}>
                <StarRating rating={biz.rating} size={13} />
                <span style={{ color: COLORS.gold, fontWeight: 700, fontSize: 14 }}>{biz.rating}</span>
                <span style={{ color: "rgba(255,255,255,0.5)", fontSize: 13 }}>({biz.reviewCount} reviews)</span>
              </div>
            </div>
          </div>

          {/* Quick action buttons */}
          <div style={{ display: "flex", gap: 8, marginTop: 16 }}>
            {[
              { label: "📞 Call", bg: COLORS.gold, color: COLORS.dark },
              { label: "💬 WhatsApp", bg: "#25D366", color: "#fff" },
              { label: "🌐 Website", bg: "rgba(255,255,255,0.12)", color: "#fff" },
              { label: "📍 Maps", bg: "rgba(255,255,255,0.12)", color: "#fff" },
            ].map(btn => (
              <button key={btn.label} style={{
                background: btn.bg, color: btn.color,
                border: "none", borderRadius: 8,
                padding: "8px 12px", fontSize: 12,
                fontWeight: 700, cursor: "pointer", flex: 1,
              }}>{btn.label}</button>
            ))}
          </div>
        </div>

        {/* Tabs */}
        <div style={{
          display: "flex", borderBottom: `2px solid ${COLORS.sand}`,
          background: COLORS.white,
        }}>
          {tabs.map(tab => (
            <button key={tab} onClick={() => setActiveTab(tab)} style={{
              flex: 1, padding: "12px 8px",
              background: "none", border: "none",
              borderBottom: activeTab === tab ? `2px solid ${COLORS.gold}` : "2px solid transparent",
              color: activeTab === tab ? COLORS.gold : COLORS.gray,
              fontWeight: 700, fontSize: 13,
              cursor: "pointer", textTransform: "capitalize",
              marginBottom: -2,
            }}>{tab}</button>
          ))}
        </div>

        {/* Tab content */}
        <div style={{ flex: 1, overflow: "auto", padding: "20px 24px" }}>
          {activeTab === "overview" && (
            <div>
              <p style={{ color: COLORS.dark, lineHeight: 1.7, marginBottom: 20, fontSize: 14 }}>{biz.description}</p>

              <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12, marginBottom: 20 }}>
                {[
                  { icon: "📞", label: "Phone", value: biz.phone },
                  { icon: "🕐", label: "Hours", value: biz.hours },
                  { icon: "📅", label: "Est.", value: biz.established },
                  { icon: "📍", label: "Location", value: "Makrana, RJ" },
                ].map(item => (
                  <div key={item.label} style={{
                    background: COLORS.bg, borderRadius: 10, padding: "12px 14px",
                    border: `1px solid ${COLORS.sand}`,
                  }}>
                    <div style={{ fontSize: 11, color: COLORS.gray, marginBottom: 2 }}>{item.icon} {item.label}</div>
                    <div style={{ fontSize: 13, fontWeight: 700, color: COLORS.dark }}>{item.value}</div>
                  </div>
                ))}
              </div>

              <div style={{ marginBottom: 16 }}>
                <div style={{ fontSize: 13, fontWeight: 700, color: COLORS.dark, marginBottom: 8 }}>Tags</div>
                <div style={{ display: "flex", gap: 6, flexWrap: "wrap" }}>
                  {biz.tags.map(t => (
                    <span key={t} style={{
                      background: COLORS.sand, color: COLORS.amber,
                      padding: "5px 12px", borderRadius: 20, fontSize: 12, fontWeight: 600,
                    }}>{t}</span>
                  ))}
                </div>
              </div>

              {/* Fake Map */}
              <div style={{
                height: 160, background: "linear-gradient(135deg, #c8d8c0 0%, #a8c0a0 100%)",
                borderRadius: 12, display: "flex", alignItems: "center", justifyContent: "center",
                border: `1px solid ${COLORS.sand}`,
              }}>
                <div style={{ textAlign: "center" }}>
                  <div style={{ fontSize: 32 }}>📍</div>
                  <div style={{ fontSize: 13, color: COLORS.dark, fontWeight: 700 }}>Google Maps Embed</div>
                  <div style={{ fontSize: 11, color: COLORS.gray }}>Live map with real listing</div>
                </div>
              </div>
            </div>
          )}

          {activeTab === "products" && (
            <div>
              <p style={{ color: COLORS.gray, fontSize: 13, marginBottom: 16 }}>Products & services offered by {biz.name}</p>
              {[
                { name: "White Makrana Marble", price: "₹180/sq ft", unit: "per sq ft" },
                { name: "Pink Marble Slabs", price: "₹220/sq ft", unit: "per sq ft" },
                { name: "Custom Cutting & Polishing", price: "₹45/sq ft", unit: "service" },
              ].map((p, i) => (
                <div key={i} style={{
                  display: "flex", justifyContent: "space-between", alignItems: "center",
                  padding: "14px 16px", background: COLORS.bg, borderRadius: 10,
                  border: `1px solid ${COLORS.sand}`, marginBottom: 10,
                }}>
                  <div>
                    <div style={{ fontWeight: 700, color: COLORS.dark, fontSize: 14 }}>{p.name}</div>
                    <div style={{ fontSize: 11, color: COLORS.gray }}>{p.unit}</div>
                  </div>
                  <div style={{ fontWeight: 800, color: COLORS.gold, fontSize: 16 }}>{p.price}</div>
                </div>
              ))}
            </div>
          )}

          {activeTab === "reviews" && (
            <div>
              <div style={{
                background: COLORS.bg, borderRadius: 12, padding: 16,
                border: `1px solid ${COLORS.sand}`, marginBottom: 16,
                display: "flex", alignItems: "center", gap: 20,
              }}>
                <div style={{ textAlign: "center" }}>
                  <div style={{ fontSize: 40, fontWeight: 900, color: COLORS.dark }}>{biz.rating}</div>
                  <StarRating rating={biz.rating} size={16} />
                  <div style={{ fontSize: 12, color: COLORS.gray, marginTop: 4 }}>{biz.reviewCount} reviews</div>
                </div>
                <div style={{ flex: 1 }}>
                  {[5, 4, 3, 2, 1].map(s => (
                    <div key={s} style={{ display: "flex", alignItems: "center", gap: 8, marginBottom: 4 }}>
                      <span style={{ fontSize: 11, color: COLORS.gray, width: 8 }}>{s}</span>
                      <div style={{ flex: 1, height: 6, background: COLORS.sand, borderRadius: 3 }}>
                        <div style={{
                          height: "100%", borderRadius: 3, background: COLORS.gold,
                          width: s === 5 ? "68%" : s === 4 ? "22%" : s === 3 ? "7%" : "2%",
                        }} />
                      </div>
                    </div>
                  ))}
                </div>
              </div>

              {[
                { name: "Arjun Sharma", rating: 5, date: "2 weeks ago", text: "Excellent quality marble. Best supplier in Makrana. Highly recommended for bulk orders!" },
                { name: "Priya Gupta", rating: 4, date: "1 month ago", text: "Good quality, timely delivery. Pricing is fair for the quality they provide." },
              ].map((r, i) => (
                <div key={i} style={{
                  padding: "14px 16px", background: COLORS.bg, borderRadius: 10,
                  border: `1px solid ${COLORS.sand}`, marginBottom: 10,
                }}>
                  <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 6 }}>
                    <div style={{ fontWeight: 700, color: COLORS.dark, fontSize: 14 }}>👤 {r.name}</div>
                    <div style={{ fontSize: 11, color: COLORS.gray }}>{r.date}</div>
                  </div>
                  <StarRating rating={r.rating} size={12} />
                  <p style={{ fontSize: 13, color: COLORS.gray, marginTop: 6, lineHeight: 1.5 }}>{r.text}</p>
                </div>
              ))}
            </div>
          )}

          {activeTab === "enquiry" && (
            <div>
              {submitted ? (
                <div style={{
                  textAlign: "center", padding: "40px 20px",
                  background: "#E8F5E9", borderRadius: 16, border: "1px solid #A5D6A7",
                }}>
                  <div style={{ fontSize: 48 }}>✅</div>
                  <h3 style={{ color: COLORS.green, marginTop: 12 }}>Enquiry Sent!</h3>
                  <p style={{ color: COLORS.dark, fontSize: 14 }}>The business will contact you shortly.</p>
                </div>
              ) : (
                <div>
                  <p style={{ color: COLORS.gray, fontSize: 13, marginBottom: 16 }}>Send your enquiry directly to {biz.name}</p>
                  {[
                    { label: "Your Name", key: "name", type: "text", placeholder: "Enter your full name" },
                    { label: "Phone Number", key: "phone", type: "tel", placeholder: "+91 XXXXX XXXXX" },
                  ].map(f => (
                    <div key={f.key} style={{ marginBottom: 14 }}>
                      <label style={{ fontSize: 12, fontWeight: 700, color: COLORS.dark, display: "block", marginBottom: 6 }}>{f.label}</label>
                      <input
                        type={f.type}
                        placeholder={f.placeholder}
                        value={enquiryForm[f.key]}
                        onChange={e => setEnquiryForm({ ...enquiryForm, [f.key]: e.target.value })}
                        style={{
                          width: "100%", padding: "12px 14px", borderRadius: 10,
                          border: `1.5px solid ${COLORS.sand}`, fontSize: 14,
                          background: COLORS.bg, color: COLORS.dark,
                          outline: "none", boxSizing: "border-box",
                        }}
                      />
                    </div>
                  ))}
                  <div style={{ marginBottom: 18 }}>
                    <label style={{ fontSize: 12, fontWeight: 700, color: COLORS.dark, display: "block", marginBottom: 6 }}>Message</label>
                    <textarea
                      placeholder="Describe what you need..."
                      value={enquiryForm.message}
                      onChange={e => setEnquiryForm({ ...enquiryForm, message: e.target.value })}
                      rows={4}
                      style={{
                        width: "100%", padding: "12px 14px", borderRadius: 10,
                        border: `1.5px solid ${COLORS.sand}`, fontSize: 14,
                        background: COLORS.bg, color: COLORS.dark,
                        outline: "none", resize: "vertical", boxSizing: "border-box",
                      }}
                    />
                  </div>
                  <button
                    onClick={() => setSubmitted(true)}
                    style={{
                      width: "100%", background: COLORS.gold, color: COLORS.dark,
                      border: "none", borderRadius: 12, padding: "14px",
                      fontSize: 15, fontWeight: 800, cursor: "pointer",
                    }}
                  >Send Enquiry →</button>
                </div>
              )}
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

// ─── PAGES ───────────────────────────────────────────────────────────────────

function HomePage({ onNavigate }) {
  const [searchQuery, setSearchQuery] = useState("");
  const [selectedCategory, setSelectedCategory] = useState(null);
  const [selectedBiz, setSelectedBiz] = useState(null);
  const [filteredBiz, setFilteredBiz] = useState(BUSINESSES);
  const [activeFilter, setActiveFilter] = useState("all");

  useEffect(() => {
    let results = BUSINESSES;
    if (selectedCategory) results = results.filter(b => b.categorySlug === selectedCategory);
    if (searchQuery) {
      const q = searchQuery.toLowerCase();
      results = results.filter(b =>
        b.name.toLowerCase().includes(q) ||
        b.category.toLowerCase().includes(q) ||
        b.description.toLowerCase().includes(q) ||
        b.tags.some(t => t.toLowerCase().includes(q))
      );
    }
    if (activeFilter === "verified") results = results.filter(b => b.verified);
    if (activeFilter === "featured") results = results.filter(b => b.featured);
    if (activeFilter === "top-rated") results = results.sort((a, b) => b.rating - a.rating);
    setFilteredBiz(results);
  }, [searchQuery, selectedCategory, activeFilter]);

  return (
    <div style={{ background: COLORS.bg, minHeight: "100vh", fontFamily: "'Inter', system-ui, sans-serif" }}>
      {/* NAVBAR */}
      <nav style={{
        background: COLORS.dark,
        padding: "0 24px",
        display: "flex", alignItems: "center", justifyContent: "space-between",
        height: 60, position: "sticky", top: 0, zIndex: 100,
        borderBottom: `2px solid ${COLORS.gold}`,
      }}>
        <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
          <div style={{
            width: 36, height: 36, borderRadius: 8,
            background: `linear-gradient(135deg, ${COLORS.gold}, #8B6914)`,
            display: "flex", alignItems: "center", justifyContent: "center",
            fontSize: 18,
          }}>⛏️</div>
          <div>
            <div style={{ color: COLORS.white, fontWeight: 900, fontSize: 16, fontFamily: "'Georgia', serif", letterSpacing: "-0.01em" }}>
              Makrana<span style={{ color: COLORS.gold }}>Biz</span>
            </div>
            <div style={{ color: "rgba(255,255,255,0.4)", fontSize: 9, letterSpacing: "0.15em", textTransform: "uppercase" }}>
              Business Directory
            </div>
          </div>
        </div>
        <div style={{ display: "flex", gap: 8, alignItems: "center" }}>
          <button onClick={() => onNavigate("admin")} style={{
            background: "rgba(201,168,76,0.15)", color: COLORS.gold,
            border: `1px solid rgba(201,168,76,0.3)`, borderRadius: 8,
            padding: "6px 14px", fontSize: 12, fontWeight: 700, cursor: "pointer",
          }}>Admin Panel</button>
          <button style={{
            background: COLORS.gold, color: COLORS.dark,
            border: "none", borderRadius: 8,
            padding: "6px 14px", fontSize: 12, fontWeight: 800, cursor: "pointer",
          }}>+ List Business</button>
        </div>
      </nav>

      {/* HERO */}
      <div style={{
        background: `linear-gradient(160deg, #1A1714 0%, #2C2015 60%, #1A1714 100%)`,
        padding: "60px 24px 80px",
        position: "relative",
        overflow: "hidden",
      }}>
        {/* Marble texture lines */}
        {[...Array(8)].map((_, i) => (
          <div key={i} style={{
            position: "absolute",
            top: `${10 + i * 12}%`,
            left: "-10%", right: "-10%",
            height: 1,
            background: `rgba(201,168,76,${0.03 + i * 0.01})`,
            transform: `rotate(${-8 + i * 2}deg)`,
          }} />
        ))}

        <div style={{ maxWidth: 680, margin: "0 auto", position: "relative" }}>
          <div style={{
            display: "inline-flex", alignItems: "center", gap: 8,
            background: "rgba(201,168,76,0.15)", border: "1px solid rgba(201,168,76,0.3)",
            borderRadius: 20, padding: "5px 14px", marginBottom: 20,
          }}>
            <span style={{ color: COLORS.gold, fontSize: 12, fontWeight: 700, letterSpacing: "0.08em" }}>
              🗺️ MAKRANA, RAJASTHAN
            </span>
          </div>

          <h1 style={{
            color: COLORS.white,
            fontSize: "clamp(28px, 6vw, 52px)",
            fontWeight: 900,
            fontFamily: "'Georgia', serif",
            lineHeight: 1.15,
            margin: "0 0 16px",
            letterSpacing: "-0.02em",
          }}>
            Discover Every Business<br />
            <span style={{
              background: `linear-gradient(90deg, ${COLORS.gold}, #E8C96C)`,
              WebkitBackgroundClip: "text",
              WebkitTextFillColor: "transparent",
            }}>in Makrana</span>
          </h1>

          <p style={{
            color: "rgba(255,255,255,0.6)",
            fontSize: 16, lineHeight: 1.7,
            margin: "0 0 32px",
            maxWidth: 480,
          }}>
            From world-famous Marble factories to local restaurants — find, contact, and review 1,200+ businesses in the Marble Capital of India.
          </p>

          {/* Search bar */}
          <div style={{
            display: "flex", gap: 0,
            background: COLORS.white,
            borderRadius: 14,
            overflow: "hidden",
            boxShadow: "0 8px 40px rgba(0,0,0,0.3)",
            border: `2px solid ${COLORS.gold}`,
          }}>
            <div style={{ padding: "0 16px", display: "flex", alignItems: "center", color: COLORS.gray }}>🔍</div>
            <input
              type="text"
              placeholder="Search marble factories, hotels, restaurants..."
              value={searchQuery}
              onChange={e => setSearchQuery(e.target.value)}
              style={{
                flex: 1, padding: "16px 0", fontSize: 14,
                border: "none", outline: "none",
                color: COLORS.dark, background: "transparent",
              }}
            />
            <button style={{
              background: COLORS.gold, color: COLORS.dark,
              border: "none", padding: "16px 28px",
              fontSize: 14, fontWeight: 800, cursor: "pointer",
              whiteSpace: "nowrap",
            }}>Search</button>
          </div>

          {/* Quick categories in hero */}
          <div style={{ display: "flex", gap: 8, marginTop: 16, flexWrap: "wrap" }}>
            {["Marble Factories", "Hotels", "Restaurants", "Hospitals"].map(cat => (
              <button
                key={cat}
                onClick={() => setSearchQuery(cat)}
                style={{
                  background: "rgba(255,255,255,0.08)",
                  border: "1px solid rgba(255,255,255,0.15)",
                  color: "rgba(255,255,255,0.7)",
                  borderRadius: 20, padding: "5px 14px",
                  fontSize: 12, cursor: "pointer",
                }}
              >{cat}</button>
            ))}
          </div>
        </div>
      </div>

      {/* STATS STRIP */}
      <div style={{
        background: COLORS.gold,
        padding: "0",
      }}>
        <div style={{
          maxWidth: 800, margin: "0 auto",
          display: "grid", gridTemplateColumns: "repeat(4, 1fr)",
        }}>
          {STATS.map((s, i) => (
            <div key={i} style={{
              padding: "18px 16px",
              textAlign: "center",
              borderRight: i < 3 ? `1px solid rgba(26,23,20,0.2)` : "none",
            }}>
              <div style={{ fontSize: 20, marginBottom: 2 }}>{s.icon}</div>
              <div style={{ fontWeight: 900, fontSize: 18, color: COLORS.dark }}>{s.value}</div>
              <div style={{ fontSize: 11, color: "rgba(26,23,20,0.6)", fontWeight: 600 }}>{s.label}</div>
            </div>
          ))}
        </div>
      </div>

      {/* CATEGORIES */}
      <div style={{ padding: "40px 24px", maxWidth: 1100, margin: "0 auto" }}>
        <div style={{ marginBottom: 24, display: "flex", alignItems: "baseline", justifyContent: "space-between" }}>
          <div>
            <div style={{ fontSize: 11, color: COLORS.gold, fontWeight: 800, letterSpacing: "0.12em", textTransform: "uppercase", marginBottom: 4 }}>
              Browse by Category
            </div>
            <h2 style={{ fontSize: 26, fontWeight: 800, color: COLORS.dark, fontFamily: "'Georgia', serif", margin: 0 }}>
              What are you looking for?
            </h2>
          </div>
          {selectedCategory && (
            <button onClick={() => setSelectedCategory(null)} style={{
              background: COLORS.sand, color: COLORS.amber,
              border: "none", borderRadius: 8, padding: "6px 14px",
              fontSize: 12, fontWeight: 700, cursor: "pointer",
            }}>✕ Clear Filter</button>
          )}
        </div>

        <div style={{
          display: "grid",
          gridTemplateColumns: "repeat(auto-fill, minmax(150px, 1fr))",
          gap: 10,
        }}>
          {CATEGORIES.map(cat => (
            <div
              key={cat.id}
              onClick={() => setSelectedCategory(selectedCategory === cat.slug ? null : cat.slug)}
              style={{
                background: selectedCategory === cat.slug ? COLORS.dark : COLORS.white,
                border: `1.5px solid ${selectedCategory === cat.slug ? COLORS.gold : COLORS.sand}`,
                borderRadius: 12, padding: "16px 12px",
                textAlign: "center", cursor: "pointer",
                transition: "all 0.2s",
                boxShadow: selectedCategory === cat.slug ? `0 4px 20px rgba(201,168,76,0.3)` : "none",
              }}
            >
              <div style={{ fontSize: 28, marginBottom: 6 }}>{cat.icon}</div>
              <div style={{ fontSize: 12, fontWeight: 700, color: selectedCategory === cat.slug ? COLORS.gold : COLORS.dark, lineHeight: 1.3 }}>
                {cat.name}
              </div>
              <div style={{ fontSize: 10, color: selectedCategory === cat.slug ? "rgba(255,255,255,0.5)" : COLORS.gray, marginTop: 3 }}>
                {cat.count} listed
              </div>
            </div>
          ))}
        </div>
      </div>

      {/* GOLD DIVIDER */}
      <div style={{ maxWidth: 1100, margin: "0 auto 40px", padding: "0 24px" }}>
        <div style={{ height: 1, background: `linear-gradient(90deg, transparent, ${COLORS.gold}, transparent)` }} />
      </div>

      {/* BUSINESSES */}
      <div style={{ padding: "0 24px 60px", maxWidth: 1100, margin: "0 auto" }}>
        <div style={{ marginBottom: 20, display: "flex", alignItems: "center", justifyContent: "space-between", flexWrap: "wrap", gap: 12 }}>
          <div>
            <div style={{ fontSize: 11, color: COLORS.gold, fontWeight: 800, letterSpacing: "0.12em", textTransform: "uppercase", marginBottom: 4 }}>
              {selectedCategory ? CATEGORIES.find(c => c.slug === selectedCategory)?.name : "All Businesses"}
            </div>
            <h2 style={{ fontSize: 26, fontWeight: 800, color: COLORS.dark, fontFamily: "'Georgia', serif", margin: 0 }}>
              {filteredBiz.length} businesses found
            </h2>
          </div>

          {/* Filters */}
          <div style={{ display: "flex", gap: 6 }}>
            {[
              { key: "all", label: "All" },
              { key: "featured", label: "⭐ Featured" },
              { key: "verified", label: "✓ Verified" },
              { key: "top-rated", label: "🔝 Top Rated" },
            ].map(f => (
              <button
                key={f.key}
                onClick={() => setActiveFilter(f.key)}
                style={{
                  background: activeFilter === f.key ? COLORS.dark : COLORS.white,
                  color: activeFilter === f.key ? COLORS.gold : COLORS.gray,
                  border: `1.5px solid ${activeFilter === f.key ? COLORS.dark : COLORS.sand}`,
                  borderRadius: 8, padding: "6px 12px",
                  fontSize: 12, fontWeight: 700, cursor: "pointer",
                }}
              >{f.label}</button>
            ))}
          </div>
        </div>

        {filteredBiz.length === 0 ? (
          <div style={{ textAlign: "center", padding: "60px 20px" }}>
            <div style={{ fontSize: 48 }}>🔍</div>
            <h3 style={{ color: COLORS.dark, marginTop: 12 }}>No businesses found</h3>
            <p style={{ color: COLORS.gray }}>Try a different search or category</p>
            <button onClick={() => { setSearchQuery(""); setSelectedCategory(null); }} style={{
              background: COLORS.gold, color: COLORS.dark, border: "none",
              borderRadius: 8, padding: "10px 24px", fontWeight: 700, cursor: "pointer", marginTop: 8,
            }}>Clear filters</button>
          </div>
        ) : (
          <div style={{
            display: "grid",
            gridTemplateColumns: "repeat(auto-fill, minmax(300px, 1fr))",
            gap: 20,
          }}>
            {filteredBiz.map(biz => (
              <BusinessCard key={biz.id} biz={biz} onClick={setSelectedBiz} />
            ))}
          </div>
        )}
      </div>

      {/* CTA BANNER */}
      <div style={{
        background: COLORS.dark,
        borderTop: `3px solid ${COLORS.gold}`,
        padding: "50px 24px",
        textAlign: "center",
      }}>
        <div style={{ fontSize: 11, color: COLORS.gold, fontWeight: 800, letterSpacing: "0.15em", textTransform: "uppercase", marginBottom: 12 }}>
          Own a business in Makrana?
        </div>
        <h2 style={{ color: COLORS.white, fontSize: 28, fontFamily: "'Georgia', serif", margin: "0 0 12px" }}>
          Get your business discovered
        </h2>
        <p style={{ color: "rgba(255,255,255,0.5)", fontSize: 14, maxWidth: 400, margin: "0 auto 24px" }}>
          Free listing for all Makrana businesses. Reach thousands of buyers, exporters, and tourists every month.
        </p>
        <div style={{ display: "flex", gap: 12, justifyContent: "center", flexWrap: "wrap" }}>
          <button style={{
            background: COLORS.gold, color: COLORS.dark, border: "none",
            borderRadius: 10, padding: "14px 28px",
            fontSize: 15, fontWeight: 800, cursor: "pointer",
          }}>+ Add Your Business — Free</button>
          <button style={{
            background: "transparent", color: COLORS.white,
            border: "1px solid rgba(255,255,255,0.3)",
            borderRadius: 10, padding: "14px 28px",
            fontSize: 15, fontWeight: 600, cursor: "pointer",
          }}>Claim Existing Listing</button>
        </div>
      </div>

      {/* FOOTER */}
      <footer style={{ background: "#0F0D0C", padding: "30px 24px", textAlign: "center" }}>
        <div style={{ color: COLORS.gold, fontFamily: "'Georgia', serif", fontSize: 18, fontWeight: 800, marginBottom: 8 }}>
          MakranaBiz
        </div>
        <p style={{ color: "rgba(255,255,255,0.3)", fontSize: 12, margin: "0 0 16px" }}>
          The #1 Business Directory for Makrana, Rajasthan — India's Marble Capital
        </p>
        <div style={{ display: "flex", gap: 20, justifyContent: "center", flexWrap: "wrap" }}>
          {["About", "Contact", "Privacy Policy", "List Your Business", "Admin Login"].map(link => (
            <span key={link} style={{ color: "rgba(255,255,255,0.4)", fontSize: 12, cursor: "pointer" }}>{link}</span>
          ))}
        </div>
      </footer>

      {/* BUSINESS MODAL */}
      {selectedBiz && <BusinessModal biz={selectedBiz} onClose={() => setSelectedBiz(null)} />}
    </div>
  );
}

function AdminPanel({ onNavigate }) {
  const [activeSection, setActiveSection] = useState("dashboard");
  const [businesses, setBusinesses] = useState(
    BUSINESSES.map(b => ({ ...b, status: b.verified ? "approved" : "pending" }))
  );

  const pending = businesses.filter(b => b.status === "pending");
  const approved = businesses.filter(b => b.status === "approved");

  const approve = (id) => setBusinesses(bs => bs.map(b => b.id === id ? { ...b, status: "approved", verified: true } : b));
  const reject = (id) => setBusinesses(bs => bs.map(b => b.id === id ? { ...b, status: "rejected" } : b));

  const navItems = [
    { key: "dashboard", label: "📊 Dashboard" },
    { key: "pending", label: `⏳ Pending (${pending.length})` },
    { key: "businesses", label: "🏢 All Businesses" },
    { key: "users", label: "👥 Users" },
    { key: "analytics", label: "📈 Analytics" },
  ];

  return (
    <div style={{ display: "flex", height: "100vh", fontFamily: "'Inter', system-ui, sans-serif", background: "#0F1117" }}>
      {/* Sidebar */}
      <div style={{
        width: 220, background: "#16181E",
        borderRight: "1px solid #2A2D38",
        display: "flex", flexDirection: "column",
        flexShrink: 0,
      }}>
        <div style={{ padding: "20px 16px", borderBottom: "1px solid #2A2D38" }}>
          <div style={{ color: COLORS.gold, fontFamily: "'Georgia', serif", fontSize: 16, fontWeight: 800 }}>MakranaBiz</div>
          <div style={{ color: "#5A6080", fontSize: 10, letterSpacing: "0.1em", textTransform: "uppercase", marginTop: 2 }}>Admin Panel</div>
        </div>
        <nav style={{ flex: 1, padding: "12px 10px" }}>
          {navItems.map(item => (
            <button key={item.key} onClick={() => setActiveSection(item.key)} style={{
              display: "block", width: "100%", textAlign: "left",
              padding: "10px 12px", marginBottom: 2,
              background: activeSection === item.key ? "rgba(201,168,76,0.12)" : "transparent",
              color: activeSection === item.key ? COLORS.gold : "#8892B0",
              border: "none", borderRadius: 8,
              fontSize: 13, fontWeight: activeSection === item.key ? 700 : 400,
              cursor: "pointer",
              borderLeft: activeSection === item.key ? `3px solid ${COLORS.gold}` : "3px solid transparent",
            }}>{item.label}</button>
          ))}
        </nav>
        <div style={{ padding: "12px 10px", borderTop: "1px solid #2A2D38" }}>
          <button onClick={() => onNavigate("home")} style={{
            display: "block", width: "100%", textAlign: "left",
            padding: "10px 12px", background: "transparent",
            color: "#5A6080", border: "none", fontSize: 12, cursor: "pointer",
          }}>← Back to Site</button>
        </div>
      </div>

      {/* Main */}
      <div style={{ flex: 1, overflow: "auto", padding: "24px 28px" }}>
        {activeSection === "dashboard" && (
          <div>
            <h1 style={{ color: "#E2E8F0", fontSize: 24, fontWeight: 800, marginBottom: 24 }}>Dashboard Overview</h1>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 16, marginBottom: 32 }}>
              {[
                { label: "Total Businesses", value: businesses.length, icon: "🏢", color: COLORS.gold },
                { label: "Pending Approval", value: pending.length, icon: "⏳", color: "#F59E0B" },
                { label: "Approved", value: approved.length, icon: "✅", color: "#10B981" },
                { label: "Total Users", value: "1,247", icon: "👥", color: "#6366F1" },
              ].map((stat, i) => (
                <div key={i} style={{
                  background: "#1E2130", borderRadius: 12,
                  padding: "20px", border: "1px solid #2A2D38",
                }}>
                  <div style={{ fontSize: 24, marginBottom: 8 }}>{stat.icon}</div>
                  <div style={{ fontSize: 26, fontWeight: 900, color: stat.color }}>{stat.value}</div>
                  <div style={{ fontSize: 12, color: "#5A6080", marginTop: 2 }}>{stat.label}</div>
                </div>
              ))}
            </div>

            <div style={{ background: "#1E2130", borderRadius: 12, padding: 20, border: "1px solid #2A2D38" }}>
              <h3 style={{ color: "#E2E8F0", marginBottom: 16, fontSize: 15 }}>Recent Submissions</h3>
              {pending.slice(0, 3).map(b => (
                <div key={b.id} style={{
                  display: "flex", alignItems: "center", justifyContent: "space-between",
                  padding: "12px 0", borderBottom: "1px solid #2A2D38",
                }}>
                  <div>
                    <div style={{ color: "#E2E8F0", fontWeight: 700, fontSize: 14 }}>{b.name}</div>
                    <div style={{ color: "#5A6080", fontSize: 12 }}>{b.category} · {b.address}</div>
                  </div>
                  <div style={{ display: "flex", gap: 8 }}>
                    <button onClick={() => approve(b.id)} style={{
                      background: "#10B981", color: "#fff", border: "none",
                      borderRadius: 6, padding: "5px 14px", fontSize: 12, fontWeight: 700, cursor: "pointer",
                    }}>Approve</button>
                    <button onClick={() => reject(b.id)} style={{
                      background: "#EF4444", color: "#fff", border: "none",
                      borderRadius: 6, padding: "5px 14px", fontSize: 12, fontWeight: 700, cursor: "pointer",
                    }}>Reject</button>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {activeSection === "pending" && (
          <div>
            <h1 style={{ color: "#E2E8F0", fontSize: 24, fontWeight: 800, marginBottom: 24 }}>
              Pending Approvals ({pending.length})
            </h1>
            {pending.length === 0 ? (
              <div style={{ textAlign: "center", padding: "60px 20px", color: "#5A6080" }}>
                <div style={{ fontSize: 48 }}>✅</div>
                <p>All caught up! No pending businesses.</p>
              </div>
            ) : pending.map(b => (
              <div key={b.id} style={{
                background: "#1E2130", borderRadius: 12, padding: "20px",
                border: "1px solid #2A2D38", marginBottom: 12,
                display: "flex", alignItems: "flex-start", justifyContent: "space-between", gap: 16,
              }}>
                <div style={{ flex: 1 }}>
                  <div style={{ display: "flex", gap: 8, alignItems: "center", marginBottom: 6 }}>
                    <span style={{ fontSize: 18 }}>{CATEGORIES.find(c => c.slug === b.categorySlug)?.icon}</span>
                    <span style={{ color: "#E2E8F0", fontWeight: 700, fontSize: 16 }}>{b.name}</span>
                    <span style={{ background: "#F59E0B20", color: "#F59E0B", fontSize: 10, fontWeight: 700, padding: "2px 8px", borderRadius: 10 }}>PENDING</span>
                  </div>
                  <div style={{ color: "#5A6080", fontSize: 13, marginBottom: 6 }}>{b.category} · {b.address}</div>
                  <div style={{ color: "#8892B0", fontSize: 13 }}>{b.phone} · {b.hours}</div>
                </div>
                <div style={{ display: "flex", gap: 8, flexShrink: 0 }}>
                  <button onClick={() => approve(b.id)} style={{
                    background: "#10B981", color: "#fff", border: "none",
                    borderRadius: 8, padding: "8px 18px", fontSize: 13, fontWeight: 700, cursor: "pointer",
                  }}>✓ Approve</button>
                  <button onClick={() => reject(b.id)} style={{
                    background: "#EF4444", color: "#fff", border: "none",
                    borderRadius: 8, padding: "8px 18px", fontSize: 13, fontWeight: 700, cursor: "pointer",
                  }}>✕ Reject</button>
                </div>
              </div>
            ))}
          </div>
        )}

        {activeSection === "businesses" && (
          <div>
            <h1 style={{ color: "#E2E8F0", fontSize: 24, fontWeight: 800, marginBottom: 24 }}>
              All Businesses ({businesses.length})
            </h1>
            <div style={{ background: "#1E2130", borderRadius: 12, border: "1px solid #2A2D38", overflow: "hidden" }}>
              <table style={{ width: "100%", borderCollapse: "collapse" }}>
                <thead>
                  <tr style={{ borderBottom: "1px solid #2A2D38" }}>
                    {["Business", "Category", "Phone", "Rating", "Status"].map(h => (
                      <th key={h} style={{ padding: "12px 16px", textAlign: "left", color: "#5A6080", fontSize: 12, fontWeight: 700, textTransform: "uppercase" }}>{h}</th>
                    ))}
                  </tr>
                </thead>
                <tbody>
                  {businesses.map(b => (
                    <tr key={b.id} style={{ borderBottom: "1px solid #2A2D38" }}>
                      <td style={{ padding: "14px 16px", color: "#E2E8F0", fontWeight: 600, fontSize: 13 }}>{b.name}</td>
                      <td style={{ padding: "14px 16px", color: "#8892B0", fontSize: 13 }}>{b.category}</td>
                      <td style={{ padding: "14px 16px", color: "#8892B0", fontSize: 13 }}>{b.phone}</td>
                      <td style={{ padding: "14px 16px" }}>
                        <span style={{ color: COLORS.gold, fontWeight: 700, fontSize: 13 }}>★ {b.rating}</span>
                      </td>
                      <td style={{ padding: "14px 16px" }}>
                        <span style={{
                          padding: "3px 10px", borderRadius: 10, fontSize: 11, fontWeight: 700,
                          background: b.status === "approved" ? "#10B98120" : b.status === "rejected" ? "#EF444420" : "#F59E0B20",
                          color: b.status === "approved" ? "#10B981" : b.status === "rejected" ? "#EF4444" : "#F59E0B",
                        }}>{b.status?.toUpperCase()}</span>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </div>
        )}

        {(activeSection === "users" || activeSection === "analytics") && (
          <div style={{ textAlign: "center", padding: "80px 20px" }}>
            <div style={{ fontSize: 56 }}>{activeSection === "users" ? "👥" : "📈"}</div>
            <h2 style={{ color: "#E2E8F0", marginTop: 16 }}>{activeSection === "users" ? "User Management" : "Analytics"}</h2>
            <p style={{ color: "#5A6080" }}>This section connects to your Supabase database in the full build.</p>
          </div>
        )}
      </div>
    </div>
  );
}

// ─── APP ROOT ────────────────────────────────────────────────────────────────
export default function App() {
  const [page, setPage] = useState("home");

  if (page === "admin") return <AdminPanel onNavigate={setPage} />;
  return <HomePage onNavigate={setPage} />;
}
