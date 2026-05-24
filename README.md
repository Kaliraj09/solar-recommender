<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Indian Rooftop Solar ROI & Recommendation Engine</title>
    <style>
        :root {
            --primary: #059669;
            --primary-hover: #047857;
            --accent: #d97706;
            --danger: #dc2626;
            --background: #f0fdf4;
            --text: #1f2937;
            --card-bg: #ffffff;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--background);
            color: var(--text);
            line-height: 1.6;
            padding: 20px;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            padding: 30px 0;
        }

        header h1 {
            color: var(--primary);
            margin-bottom: 10px;
            font-size: 2.2rem;
        }

        header p {
            color: #4b5563;
        }

        .grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 24px;
        }

        @media (min-width: 768px) {
            .grid {
                grid-template-columns: 1fr 1.2fr;
            }
        }

        .card {
            background: var(--card-bg);
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
            height: fit-content;
        }

        h2 {
            margin-bottom: 20px;
            color: #111827;
            border-bottom: 2px solid #e5e7eb;
            padding-bottom: 10px;
            font-size: 1.4rem;
        }

        .form-group {
            margin-bottom: 18px;
        }

        label {
            display: block;
            margin-bottom: 6px;
            font-weight: 600;
            color: #374151;
            font-size: 0.95rem;
        }

        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #d1d5db;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s, box-shadow 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(5, 150, 105, 0.15);
        }

        button {
            width: 100%;
            background-color: var(--primary);
            color: white;
            padding: 14px;
            border: none;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.3s;
            margin-top: 10px;
        }

        button:hover {
            background-color: var(--primary-hover);
        }

        .results-hidden {
            display: none;
        }

        /* Recommendation Layout Styles */
        .verdict-box {
            padding: 18px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-weight: bold;
            font-size: 1.1rem;
            text-align: center;
        }

        .verdict-go { background-color: #d1fae5; color: #065f46; border: 1px solid #a7f3d0; }
        .verdict-warn { background-color: #fef3c7; color: #92400e; border: 1px solid #fde68a; }
        .verdict-stop { background-color: #fee2e2; color: #991b1b; border: 1px solid #fca5a5; }

        .metrics-table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }

        .metrics-table th, .metrics-table td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #e5e7eb;
        }

        .metrics-table th {
            color: #6b7280;
            font-size: 0.85rem;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .metrics-table td {
            font-weight: 600;
            font-size: 1.1rem;
        }

        .highlight-green { color: var(--primary); }
        .highlight-orange { color: var(--accent); }

        .scheme-badge {
            background-color: #eff6ff;
            color: #1e40af;
            border: 1px solid #bfdbfe;
            padding: 10px;
            border-radius: 8px;
            font-size: 0.85rem;
            margin-bottom: 15px;
        }

        .note {
            font-size: 0.85rem;
            color: #6b7280;
            margin-top: 15px;
            font-style: italic;
            background: #f3f4f6;
            padding: 12px;
            border-radius: 8px;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>☀️ Smart Solar Consultation Engine</h1>
        <p>Evaluate financial health, exact government subsidies, real payback timelines, and an instant viability verdict.</p>
    </header>

    <div class="grid">
        <div class="card">
            <h2>Property & Power Data</h2>
            <form id="solarForm">
                <div class="form-group">
                    <label for="billCycle">Billing Cycle Frequency</label>
                    <select id="billCycle" required>
                        <option value="2" selected>Bi-Monthly (Every 2 Months - e.g., TANGEDCO)</option>
                        <option value="1">Monthly (Every Month)</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="billAmount">Average Bill Amount per Cycle (₹)</label>
                    <input type="number" id="billAmount" placeholder="e.g., 5000" required min="1">
                </div>

                <div class="form-group">
                    <label for="electricityRate">Average Unit Slab Rate (₹ per kWh)</label>
                    <input type="number" id="electricityRate" step="0.1" placeholder="e.g., 8.0" required min="1">
                </div>

                <div class="form-group">
                    <label for="roofSpace">Shadow-Free Terrace/Roof Area (sq. ft.)</label>
                    <input type="number" id="roofSpace" placeholder="e.g., 350" required min="30">
                </div>

                <div class="form-group">
                    <label for="region">Geographic Sunlight Multiplier</label>
                    <select id="region" required>
                        <option value="5.2">North / West India (High Irradiance - Desert/Plains)</option>
                        <option value="4.8" selected>South / Central India (Consistent Tropical Sun)</option>
                        <option value="4.2">East / North-East India (Heavy Monsoon/Cloud Cover)</option>
                    </select>
                </div>

                <button type="submit">Analyze Viability & Investment</button>
            </form>
        </div>

        <div class="card">
            <h2>Feasibility & Financial Breakdown</h2>
            <div id="resultsPlaceholder" style="text-align: center; color: #6b7280; padding-top: 100px;">
                <p>Submit your consumption figures to run structural metrics, net cost mappings, and custom ROI forecasts.</p>
            </div>
            
            <div id="resultsDisplay" class="results-hidden">
                <div id="verdictBlock" class="verdict-box">
                    Calculating Verdict...
                </div>

                <div class="scheme-badge">
                    <strong>📢 Central Scheme Connected:</strong> Subsidies are automatically calculated dynamically based on the current 2026 <em>PM Surya Ghar: Muft Bijli Yojana</em> structural models.
                </div>

                <table class="metrics-table">
                    <thead>
                        <tr><th colspan="2">System & Space Architecture</th></tr>
                    </thead>
                    <tbody>
                        <tr><td>Suggested Capacity</td><td id="cellCapacity" class="highlight-green">0.0 kWp</td></tr>
                        <tr><td>Required Panel Fleet</td><td id="cellPanels">0 Panels</td></tr>
                        <tr><td>Physical Area Check</td><td id="cellSpace">Checking Area...</td></tr>
                    </tbody>
                </table>

                <table class="metrics-table">
                    <thead>
                        <tr><th colspan="2">Financial Viability Metrics (₹)</th></tr>
                    </thead>
                    <tbody>
                        <tr><td>Est. Gross Market Cost</td><td id="cellGrossCost">₹0</td></tr>
                        <tr><td>Govt. Subsidy (DBT Credit)</td><td id="cellSubsidy" class="highlight-green">- ₹0</td></tr>
                        <tr><td><strong>Your Net Investment</strong></td><td id="cellNetCost"><strong>₹0</strong></td></tr>
                        <tr><td>Estimated Annual Savings</td><td id="cellAnnualSavings" class="highlight-green">₹0 / year</td></tr>
                        <tr><td><strong>Payback Period</strong></td><td id="cellPayback" class="highlight-orange">0 Years</td></tr>
                        <tr><td>Net 25-Year Life Savings</td><td id="cellLifeSavings" class="highlight-green">₹0</td></tr>
                    </tbody>
                </table>

                <div class="note">
                    <strong>Strategic Investment Logic:</strong> Residential systems hold an architecture lifetime of 25 years. If your custom payback timeline clocks under 5 years, transitioning to clean rooftop generation functions mathematically better than a standard fixed deposit.
                </div>
            </div>
        </div>
    </div>
</div>

<script>
    document.getElementById('solarForm').addEventListener('submit', function(e) {
        e.preventDefault();

        // 1. Inputs Acquisition
        const billCycle = parseInt(document.getElementById('billCycle').value);
        const billAmount = parseFloat(document.getElementById('billAmount').value);
        const rate = parseFloat(document.getElementById('electricityRate').value);
        const roofSpace = parseFloat(document.getElementById('roofSpace').value);
        const sunHours = parseFloat(document.getElementById('region').value);

        // 2. Technical Conversion Calculations
        const totalUnitsPerCycle = billAmount / rate;
        const monthlyUnitsConsumed = billCycle === 2 ? (totalUnitsPerCycle / 2) : totalUnitsPerCycle;
        const dailyUnitsTarget = monthlyUnitsConsumed / 30;
        
        // System Capacity requirement calculation factoring safety engineering variables (15% plant dropout)
        let neededKw = (dailyUnitsTarget / sunHours) * 1.15;
        if (neededKw < 1) neededKw = 1; // Base micro floor

        // Scale against high efficiency modern 540Wp modules (0.54 kWp per module)
        const panelPowerKw = 0.54;
        let panelsCount = Math.ceil(neededKw / panelPowerKw);
        const targetedPlantCapacity = panelsCount * panelPowerKw;

        // Space Engineering Metrics: Standard Indian installations require ~90 sq.ft per kWp for safe structural clearance
        const structuralSpaceRequired = targetedPlantCapacity * 90;
        let areaFeasible = roofSpace >= structuralSpaceRequired;

        // 3. Financial Engineering Framework (2026 Market Rates)
        // Average benchmark setup rate for high grade structural engineering in India: ₹55,000 per kWp gross
        const grossInvestmentCost = Math.round(targetedPlantCapacity * 55000);

        // Official PM Surya Ghar Subsidy Engine Mechanics
        let calculatedSubsidy = 0;
        if (targetedPlantCapacity <= 2) {
            calculatedSubsidy = Math.round(targetedPlantCapacity * 30000);
        } else if (targetedPlantCapacity > 2 && targetedPlantCapacity <= 3) {
            calculatedSubsidy = 60000 + Math.round((targetedPlantCapacity - 2) * 18000);
        } else {
            calculatedSubsidy = 78000; // Hard national ceiling for systems above 3kWp
        }

        const netOutofPocketCost = grossInvestmentCost - calculatedSubsidy;

        // Savings & Amortization Yield Forecasts
        const environmentalDeratingFactor = 0.80; // Accounting for seasonal clouds, local dust, wiring drops
        const monthlyGenerationUnits = targetedPlantCapacity * sunHours * 30 * environmentalDeratingFactor;
        
        // Ensure household savings don't exceed their true ongoing utility expense capacity
        const offsetUnitsMonthly = Math.min(monthlyUnitsConsumed, monthlyGenerationUnits);
        const monthlySavingsCash = offsetUnitsMonthly * rate;
        const yearlySavingsCash = monthlySavingsCash * 12;

        const paybackTimelineYears = netOutofPocketCost / yearlySavingsCash;
        const lifetimeTotalSavings = (yearlySavingsCash * 25) - netOutofPocketCost;

        // 4. Automated Verdict Evaluation Engine
        const verdictElement = document.getElementById('verdictBlock');
        verdictElement.className = "verdict-box"; // reset styles

        if (!areaFeasible) {
            verdictElement.innerText = "❌ STRONGLY NOT RECOMMENDED: Insufficient Space";
            verdictElement.classList.add("verdict-stop");
        } else if (monthlyUnitsConsumed < 120) {
            verdictElement.innerText = "⚠️ MARGINAL BENEFIT: Your Current Bill is Too Low to Justify Setup Costs";
            verdictElement.classList.add("verdict-warn");
        } else if (paybackTimelineYears <= 4.5) {
            verdictElement.innerText = "✅ HIGHLY RECOMMENDED: Phenomenal Returns & Fast Payback Financial Loop!";
            verdictElement.classList.add("verdict-go");
        } else {
            verdictElement.innerText = "✅ VIABLE INVESTMENT: Highly Profitable Long-Term Financial Option.";
            verdictElement.classList.add("verdict-go");
        }

        // 5. Data Rendering to View Elements
        document.getElementById('cellCapacity').innerText = `${targetedPlantCapacity.toFixed(2)} kWp`;
        document.getElementById('cellPanels').innerText = `${panelsCount} Units (540Wp Mono-PERC Half-Cut)`;
        
        document.getElementById('cellSpace').innerText = areaFeasible 
            ? `✅ Safe (Needs ~${Math.round(structuralSpaceRequired)} sq. ft.)` 
            : `❌ Shortfall (Needs ~${Math.round(structuralSpaceRequired)} sq. ft.)`;
        if(!areaFeasible) document.getElementById('cellSpace').style.color = 'var(--danger)';
        else document.getElementById('cellSpace').style.color = 'var(--text)';

        document.getElementById('cellGrossCost').innerText = `₹${grossInvestmentCost.toLocaleString('en-IN')}`;
        document.getElementById('cellSubsidy').innerText = `- ₹${calculatedSubsidy.toLocaleString('en-IN')}`;
        document.getElementById('cellNetCost').innerText = `₹${netOutofPocketCost.toLocaleString('en-IN')}`;
        document.getElementById('cellAnnualSavings').innerText = `₹${Math.round(yearlySavingsCash).toLocaleString('en-IN')} / year`;
        
        document.getElementById('cellPayback').innerText = `${paybackTimelineYears.toFixed(1)} Years`;
        document.getElementById('cellLifeSavings').innerText = `₹${Math.round(lifetimeTotalSavings).toLocaleString('en-IN')} across 25 Yrs`;

        // UI Reveal Execution Toggle
        document.getElementById('resultsPlaceholder').style.display = 'none';
        document.getElementById('resultsDisplay').classList.remove('results-hidden');
    });
</script>

</body>
</html>
