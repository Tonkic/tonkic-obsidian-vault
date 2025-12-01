<svg width="600" height="340" viewBox="0 0 600 340" xmlns="http://www.w3.org/2000/svg" font-family="Consolas, Monaco, 'Andale Mono', 'Ubuntu Mono', monospace">
    <defs>
        <filter id="shadow" x="-10%" y="-10%" width="120%" height="120%">
            <feDropShadow dx="0" dy="2" stdDeviation="4" flood-color="#000000" flood-opacity="0.1"/>
        </filter>
    </defs>

    <rect x="10" y="10" width="580" height="300" rx="8" fill="#ffffff" stroke="#e5e7eb" stroke-width="1" filter="url(#shadow)"/>

    <g transform="translate(30, 45)">
        <rect x="0" y="0" width="70" height="24" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
        <text x="35" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#991b1b">ERROR</text>
        
        <text x="85" y="16" font-size="13" font-weight="bold" fill="#111827">wrong_concept</text>
        
        <g transform="translate(520, 0)">
            <text x="0" y="16" text-anchor="end" font-size="13" font-weight="bold" fill="#111827">Score: <tspan fill="#991b1b">4</tspan>/10</text>
        </g>
    </g>

    <g transform="translate(30, 90)">
        <text x="0" y="0" font-size="11" font-weight="bold" fill="#6b7280" letter-spacing="0.5">CRITIQUE:</text>
        <text x="70" y="0" font-size="12" fill="#111827">The image depicts an Airbus A320, not a Boeing 737-500. Engine/wing mismatch.</text>
    </g>

    <g transform="translate(30, 155)">
        <g>
            <rect x="0" y="0" width="260" height="150" rx="6" fill="#d1fae5" fill-opacity="0.25"/>
            <path d="M0 6 a6 6 0 0 1 6 -6 h248 a6 6 0 0 1 6 6 v24 h-260 z" fill="#d1fae5" fill-opacity="0.6"/>
            <text x="10" y="20" font-size="12" font-weight="bold" fill="#065f46">✓ Correct Features</text>
            
            <g transform="translate(10, 45)">
                <text x="0" y="0" font-size="11" fill="#111827">• aircraft on ground</text><text x="0" y="24" font-size="11" fill="#111827">• two engines</text><text x="0" y="48" font-size="11" fill="#111827">• passenger windows</text><text x="0" y="72" font-size="11" fill="#111827">• tail design</text>
            </g>
        </g>

        <g transform="translate(280, 0)">
            <rect x="0" y="0" width="260" height="150" rx="6" fill="#fef3c7" fill-opacity="0.25"/>
            <path d="M0 6 a6 6 0 0 1 6 -6 h248 a6 6 0 0 1 6 6 v24 h-260 z" fill="#fef3c7" fill-opacity="0.6"/>
            <text x="10" y="20" font-size="12" font-weight="bold" fill="#92400e">🔧 Modifications</text>
            
            <g transform="translate(10, 45)">
                <text x="0" y="0" font-size="11" fill="#111827">• change model to Boeing 737-500</text><text x="0" y="24" font-size="11" fill="#111827">• adjust engine placement</text><text x="0" y="48" font-size="11" fill="#111827">• modify wing shape</text><text x="0" y="72" font-size="11" fill="#111827">• update fuselage design</text>
            </g>
        </g>
    </g>
    </svg>
    