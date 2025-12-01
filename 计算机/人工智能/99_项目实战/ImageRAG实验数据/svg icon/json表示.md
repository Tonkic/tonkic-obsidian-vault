<svg width="420" height="420" viewBox="0 0 420 420" xmlns="http://www.w3.org/2000/svg" font-family="Consolas, Monaco, 'Andale Mono', 'Ubuntu Mono', monospace">
    <defs>
        <filter id="shadow" x="-10%" y="-10%" width="120%" height="120%">
            <feDropShadow dx="0" dy="2" stdDeviation="4" flood-color="#000000" flood-opacity="0.1"/>
        </filter>
    </defs>

    <rect x="10" y="10" width="400" height="400" rx="12" fill="#ffffff" stroke="#e5e7eb" stroke-width="1" filter="url(#shadow)"/>

    <g transform="translate(30, 45)">
        <rect x="0" y="0" width="70" height="24" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
        <text x="35" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#991b1b">ERROR</text>
        
        <text x="80" y="16" font-size="13" font-weight="bold" fill="#111827">wrong_concept</text>
        
        <g transform="translate(360, 0)">
            <text x="0" y="16" text-anchor="end" font-size="13" font-weight="bold" fill="#111827">Score: <tspan fill="#991b1b">4</tspan>/10</text>
        </g>
    </g>

    <g transform="translate(30, 90)">
        <text x="0" y="0" font-size="11" font-weight="bold" fill="#6b7280" letter-spacing="0.5">CRITIQUE:</text>
        <foreignObject x="0" y="5" width="360" height="45">
            <div xmlns="http://www.w3.org/1999/xhtml" style="font-size: 12px; color: #111827; line-height: 1.4;">
                The image depicts an Airbus A320, not a Boeing 737-500. Engine/wing mismatch.
            </div>
        </foreignObject>
    </g>

    <g transform="translate(30, 150)">
        <rect x="0" y="0" width="360" height="123" rx="6" fill="#d1fae5" fill-opacity="0.25"/>
        <path d="M0 6 a6 6 0 0 1 6 -6 h348 a6 6 0 0 1 6 6 v20 h-360 z" fill="#d1fae5" fill-opacity="0.6"/>
        
        <text x="10" y="18" font-size="12" font-weight="bold" fill="#065f46">✓ Correct Features</text>
        
        <g transform="translate(10, 40)">
            <text x="0" y="0" font-size="11" fill="#111827">• aircraft on ground</text><text x="0" y="22" font-size="11" fill="#111827">• two engines</text><text x="0" y="44" font-size="11" fill="#111827">• passenger windows</text><text x="0" y="66" font-size="11" fill="#111827">• tail design</text>
        </g>
    </g>

    <g transform="translate(30, 288)">
        <rect x="0" y="0" width="360" height="123" rx="6" fill="#fef3c7" fill-opacity="0.25"/>
        <path d="M0 6 a6 6 0 0 1 6 -6 h348 a6 6 0 0 1 6 6 v20 h-360 z" fill="#fef3c7" fill-opacity="0.6"/>
        
        <text x="10" y="18" font-size="12" font-weight="bold" fill="#92400e">🔧 Modifications</text>
        
        <g transform="translate(10, 40)">
            <text x="0" y="0" font-size="11" fill="#111827">• change model to Boeing 737-500</text><text x="0" y="22" font-size="11" fill="#111827">• adjust engine placement</text><text x="0" y="44" font-size="11" fill="#111827">• modify wing shape</text><text x="0" y="66" font-size="11" fill="#111827">• update fuselage design</text>
        </g>
    </g>

    </svg>
    