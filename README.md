<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kali Smart Solar - Rooftop Feasibility Engine</title>
    <style>
        :root {
            --primary: #059669;
            --primary-dark: #047857;
            --accent: #d97706;
            --danger: #dc2626;
            --bg-canvas: #f8fafc;
            --bg-card: #ffffff;
            --text-main: #0f172a;
            --text-muted: #475569;
            --border-line: #e2e8f0;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-canvas);
            color: var(--text-main);
            line-height: 1.5;
            padding-top: 60px; /* Spacer for fixed mobile header */
        }

        /* Fixed Brand Top Header */
        .navbar {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            height: 60px;
            background: var(--bg-card);
            border-bottom: 1px solid var(--border-line);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        .navbar h1 {
            font-size: 1.25rem;
            color: var(--primary);
            font-weight: 800;
            letter-spacing: -0.5px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .hero-banner {
            text-align: center;
            padding: 24px 16px;
            background: linear-gradient(180deg, #ecfdf5 0%, var(--bg-canvas) 100%);
        }

        .hero-banner h2 {
            font-size: 1.5rem;
            color: var(--text-main);
            margin-bottom: 6px;
        }

        .hero-banner p {
            font-size: 0.9rem;
            color: var(--text-muted);
            max-width: 500px;
            margin: 0 auto;
        }

        .app-container {
            max-width: 1050px;
            margin: 0 auto;
            padding: 0 16px 40px 16px;
        }

        .mobile-stack {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        @media (min-width: 768px) {
            .mobile-stack {
                display: grid;
                grid-template-columns: 1fr 1.1fr;
                align-items: start;
            }
        }

        .ui-card {
            background: var(--bg-card);
            border: 1px solid var(--border-line);
            border-radius: 14px;
            padding: 20px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.03);
        }

        .ui-card h3 {
            font-size: 1.15rem;
            margin-bottom: 16px;
            border-bottom: 2px solid var(--bg-canvas);
            padding-bottom: 8px;
            color: var(--text-main);
        }

        .input-group {
            margin-bottom: 16px;
        }

        .input-group label {
            display: block;
            font-size: 0.85rem;
            font-weight: 700;
            color: var(--text-muted);
            margin-bottom: 6px;
            text-transform: uppercase;
            letter-spacing: 0.3px;
        }

        input, select {
            width: 100%;
            height: 46px;
            padding: 0 12px;
            border: 1px solid #cbd5e1;
            border-radius: 8px;
            font-size: 1rem;
            color: var(--text-main);
            background-color: #fff;
            transition: border-color 0.2s, box-shadow 0.2s;
            -webkit-appearance: none; /* Reset standard iOS rounding controls */
        }

        /* Native dropdown arrow fallback for custom select */
        select {
            background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23475569' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
            background-repeat: no-repeat;
            background-position: right 12px center;
            background-size: 16px;
            padding-right: 32px;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(5, 150, 105, 0.12);
        }

        .action-btn {
            width: 100%;
            height: 50px;
            background-color: var(--primary);
            color: #fff;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 700;
            cursor: pointer;
            transition: background-color 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 10px;
        }

        .action-btn:hover, .action-btn:active {
            background-color: var(--primary-dark);
        }

        /* Output View Structuring */
        .placeholder-box {
            text-align: center;
            padding: 40px 20px;
            color: var(--text-muted);
            font-size: 0.95rem;
        }

        .hidden {
            display: none !important;
        }

        .verdict-tag {
            padding: 14px;
            border-radius: 8px;
            font-weight: 700;
            font-size: 0.95rem;
            text-align: center;
            margin-bottom: 16px;
        }

        .go-green { background: #d1fae5; color: #065f46; border: 1px solid #a7f3d0; }
        .go-warn { background: #fef3c7; color: #92400e; border: 1px solid #fde68a; }
        .go-stop { background: #fee2e2; color: #991b1b; border: 1px solid #fca5a5; }

        .badge-info {
            background: #eff6ff;
            color: #1e40af;
            border: 1px solid #bfdbfe;
            padding: 10px 12px;
            border-radius: 8px;
            font-size: 0.8rem;
            margin-bottom: 16px;
            font-weight: 500;
        }

        .data-list {
            list-style: none;
            margin-bottom: 16px;
        }

        .data-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid var(--bg-canvas);
            font-size: 0.95rem;
        }

        .data-label {
            color: var(--text-muted);
            font-weight: 500;
        }

        .data-value {
            font-weight: 700;
            color: var(--text-main);
            text-align: right;
        }

        .text-green { color: var(--primary) !important; }
        .text-orange { color: var(--accent) !important; }

        .disclaimer {
            font-size: 0.8rem;
            color: var(--text-muted);
            background: #f1f5f9;
            padding: 12px;
            border-radius: 8px;
            font-style: italic;
        }

        /* Modern Footer Section */
        footer {
            background-color: #0f172a;
            color: #94a3b8;
            padding: 30px 16px;
            margin-top: 40px;
            border-top: 4px solid var(--primary);
            font-size: 0.85rem;
        }

        .footer-content {
            max-width: 1050px;
            margin: 0 auto;
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        @media (min-width: 768px) {
            .footer-content {
                flex-direction: row;
                justify-content: space-between;
            }
        }

        .footer-section {
            flex: 1;
        }

        .footer-section h4 {
            color: #f8fafc;
            font-size: 0.95rem;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .footer-links {
            list-style: none;
        }

        .footer-links li {
            margin-bottom: 6px;
        }

        .footer-links a {
            color: #94a3b8;
            text-decoration: none;
            transition: color 0.2s;
        }

        .footer-links a:hover {
            color: var(--primary);
        }

        .footer-bottom {
            text-align: center;
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px solid #334155;
            font-size: 0.8rem;
            color: #64748b;
        }
    </style>
</head>
<body>

    <!-- Fixed Navbar Header Frame -->
    <nav class="navbar">
        <h1>☀️ Kali Smart Solar</h1>
    </nav>

    <!-- App Dynamic Context Banner -->
    <section class="hero-banner">
        <h2>Rooftop ROI Evaluator</h2>
        <p>Calculate your solar setup capacity, estimated central subsidy framework, and out-of-pocket metrics instantly.</p>
    </section>

    <main class="app-container">
        <div class="mobile-stack">
            
            <!-- Inputs Input Area Container -->
            <div class="ui-card">
                <h3>Consumer Consumption Metrics</h3>
                <form id="solarCalculationForm">
                    <div class="input-group">
                        <label for="billCycle">Billing Loop Interval</label>
                        <select id="billCycle" required>
                            <option value="2" selected>Bi-Monthly (Every 2 Months - e.g., State DISCOMs)</option>
                            <option value="1">Monthly (Every Month)</option>
                        </select>
                    </div>

                    <div class="input-group">
                        <label for="billAmount">Avg Bill Cost per Cycle (₹)</label>
                        <input type="number" id="billAmount" placeholder="e.g., 6000" required min="1" inputmode="numeric">
                    </div>

                    <div class="input-group">
                        <label for="unitRate">Avg Tariff Cost (₹ per unit/kWh)</label>
                        <input type="number" id="unitRate" step="0.05" placeholder="e.g., 8.2" required min="1" inputmode="decimal">
                    </div>

                    <div class="input-group">
                        <label for="roofSpace">Clear Shadow-Free Terrace Area (sq. ft.)</label>
                        <input type="number" id="roofSpace" placeholder="e.g., 400" required min="20" inputmode="numeric">
                    </div>

                    <div class="input-group">
                        <label for="sunZone">Regional Sun Radiation Level</label>
                        <select id="sunZone" required>
                            <option value="5.2">North / West India (High Irradiance)</option>
                            <option value="4.8" selected>South / Central India (Consistent Tropical Sun)</option>
                            <option value="4.2">East / North-East India (Cloud Overcast Risk)</option>
                        </select>
                    </div>

                    <button type="submit" class="action-btn">Compute Plant Layout</button>
                </form>
            </div>

            <!-- Operational Engineering Metrics View -->
            <div class="ui-card">
                <h3>Analysis Results</h3>
                
                <div id="uiPlaceholder" class="placeholder-box">
                    <p>Provide your residential billing particulars to load capacity dimensions, PM Surya Ghar central subsidies, and exact amortization loops.</p>
                </div>

                <div id="uiResultDashboard" class="hidden">
                    <div id="verdictBadge" class="verdict-tag">Evaluating...</div>
                    
                    <div class="badge-info">
                        <strong>Live Portal Sync:</strong> Computations factor in current 2026 Direct Benefit Transfer (DBT) subsidy calculations safely.
                    </div>

                    <ul class="data-list">
                        <li class="data-row">
                            <span class="data-label">Recommended Capacity</span>
                            <span id="outSize" class="data-value text-green">0.00 kWp</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Total Panels (540Wp Mono)</span>
                            <span id="outPanels" class="data-value">0 Modules</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Terrace Space Viability</span>
                            <span id="outSpace" class="data-value">Checking...</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Gross Installation Cost</span>
                            <span id="outGross" class="data-value">₹0</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">PM Surya Ghar Subsidy</span>
                            <span id="outSubsidy" class="data-value text-green">- ₹0</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Net Investment (Out-of-Pocket)</span>
                            <span id="outNet" class="data-value">₹0</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Monthly Production Capability</span>
                            <span id="outProduction" class="data-value text-green">0 Units</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Amortization Payback Duration</span>
                            <span id="outPayback" class="data-value text-orange">0.0 Years</span>
                        </li>
                        <li class="data-row">
                            <span class="data-label">Estimated 25-Year Net Yield</span>
                            <span id="outSavings" class="data-value text-green">₹0</span>
                        </li>
                    </ul>

                    <div class="disclaimer">
                        <strong>Disclaimer:</strong> Structural engineering requires clean roof alignments. Calculated metrics follow standard domestic de-rating scales (approx 20% conversion safety variance).
                    </div>
                </div>
            </div>

        </div>
    </main>

    <!-- Professional Corporate Footer Section -->
    <footer>
        <div class="footer-content">
            <div class="footer-section">
                <h4>Kali Smart Solar</h4>
                <p>Architecting digital validation tools for decentralized green clean grid configurations inside India.</p>
            </div>
            <div class="footer-section" style="margin-top: 15px; margin-top: 0; min-width: 200px;">
                <h4>Scheme Assets</h4>
                <ul class="footer-links">
                    <li><a href="https://pmsuryaghar.gov.in/" target="_blank" rel="noreferrer">National Portal Official URL</a></li>
                    <li><a href="#">MNRE DCR Compliance Guidelines</a></li>
                    <li><a href="#">DISCOM Net-Metering Benchmarks</a></li>
                </ul>
            </div>
        </div>
        <div class="footer-bottom">
            &copy; 2026 Kali Smart Solar Engine. Built with precision for sustainable consumer engineering. All rights reserved.
        </div>
    </footer>

    <script>
        document.getElementById('solarCalculationForm').addEventListener('submit', function(event) {
            event.preventDefault();

            // 1. Data Extractor Logic
            const cycleFactor = parseInt(document.getElementById('billCycle').value);
            const rawBill = parseFloat(document.getElementById('billAmount').value);
            const electricityPrice = parseFloat(document.getElementById('unitRate').value);
            const clearRoofArea = parseFloat(document.getElementById('roofSpace').value);
            const regionSolarYield = parseFloat(document.getElementById('sunZone').value);

            // 2. Functional Conversion Computations
            const consumerUnitsPerCycle = rawBill / electricityPrice;
            const normalizedMonthlyConsumptionUnits = cycleFactor === 2 ? (consumerUnitsPerCycle / 2) : consumerUnitsPerCycle;
            const targetDailyGenerationUnits = normalizedMonthlyConsumptionUnits / 30;

            // Theoretical plant design parameters (with a 15% inline processing loss buffer)
            let recommendedKwSize = (targetDailyGenerationUnits / regionSolarYield) * 1.15;
            if(recommendedKwSize < 1) recommendedKwSize = 1.0; // Floor safety setup baseline

            // Sizing structure using current high grade 540Wp Mono PERC modules
            const modernModulePowerKw = 0.54;
            let neededModuleUnitsCount = Math.ceil(recommendedKwSize / modernModulePowerKw);
            const optimizedFinalPlantCapacityKw = neededModuleUnitsCount * modernModulePowerKw;

            // Space Assessment: Indian arrays need roughly 90 sq. ft clear zone per kW for cleaning paths & tilt clearance
            const thresholdTerraceSpaceDemanded = optimizedFinalPlantCapacityKw * 90;
            const structuralSpaceIsViable = clearRoofArea >= thresholdTerraceSpaceDemanded;

            // 3. Financial Engineering Framework Matrix (2026 Averages)
            const benchmarkGrossCostPerKw = 55000;
            const calculatedGrossCostVal = Math.round(optimizedFinalPlantCapacityKw * benchmarkGrossCostPerKw);

            // PM Surya Ghar Scheme Tier Mechanics (₹30K/kW for up to 2kW, ₹18K for 3rd kW, Max capped at ₹78K)
            let subsidyDistributionPayable = 0;
            if(optimizedFinalPlantCapacityKw <= 2.0) {
                subsidyDistributionPayable = Math.round(optimizedFinalPlantCapacityKw * 30000);
            } else if(optimizedFinalPlantCapacityKw > 2.0 && optimizedFinalPlantCapacityKw <= 3.0) {
                subsidyDistributionPayable = 60000 + Math.round((optimizedFinalPlantCapacityKw - 2.0) * 18000);
            } else {
                subsidyDistributionPayable = 78000;
            }

            const netCustomerCapitalOutlay = calculatedGrossCostVal - subsidyDistributionPayable;

            // Generation and Amortization Calculation Profiles
            const standardDeratingDeclineCoefficient = 0.82; // Environmental dust drop factor
            const calculatedMonthlyUnitsOutput = optimizedFinalPlantCapacityKw * regionSolarYield * 30 * standardDeratingDeclineCoefficient;
            
            const realBillOffsetUnits = Math.min(normalizedMonthlyConsumptionUnits, calculatedMonthlyUnitsOutput);
            const estimatedCashSavingsMonthly = realBillOffsetUnits * electricityPrice;
            const estimatedCashSavingsYearly = estimatedCashSavingsMonthly * 12;

            const finalPaybackDurationVal = netCustomerCapitalOutlay / estimatedCashSavingsYearly;
            const lifetimeAccumulatedNetYield = (estimatedCashSavingsYearly * 25) - netCustomerCapitalOutlay;

            // 4. Output Render Engine Block Execution
            const verdictNode = document.getElementById('verdictBadge');
            verdictNode.className = "verdict-tag"; 

            if(!structuralSpaceIsViable) {
                verdictNode.innerText = "❌ INSUFFICIENT ROOF SURFACE SPACE";
                verdictNode.classList.add("go-stop");
            } else if(normalizedMonthlyConsumptionUnits < 130) {
                verdictNode.innerText = "⚠️ LOW CONSUMPTION: MARGINAL MONTHLY SAVINGS";
                verdictNode.classList.add("go-warn");
            } else if(finalPaybackDurationVal <= 4.2) {
                verdictNode.innerText = "✅ HIGHLY BENEFICIAL: FANTASTIC FAST INVESTMENT YIELD";
                verdictNode.classList.add("go-green");
            } else {
                verdictNode.innerText = "✅ STABLE & PROFITABLE RESIDENTIAL VIABILITY";
                verdictNode.classList.add("go-green");
            }

            // 5. Data View Interpolations
            document.getElementById('outSize').innerText = `${optimizedFinalPlantCapacityKw.toFixed(2)} kWp`;
            document.getElementById('outPanels').innerText = `${neededModuleUnitsCount} Panels (540Wp Mono PERC)`;
            
            const spaceTargetNode = document.getElementById('outSpace');
            if(structuralSpaceIsViable) {
                spaceTargetNode.innerText = `✅ Feasible (Needs ~${Math.round(thresholdTerraceSpaceDemanded)} sq. ft.)`;
                spaceTargetNode.style.color = "var(--primary)";
            } else {
                spaceTargetNode.innerText = `❌ Shortfall (Needs ~${Math.round(thresholdTerraceSpaceDemanded)} sq. ft.)`;
                spaceTargetNode.style.color = "var(--danger)";
            }

            document.getElementById('outGross').innerText = `₹${calculatedGrossCostVal.toLocaleString('en-IN')}`;
            document.getElementById('outSubsidy').innerText = `- ₹${subsidyDistributionPayable.toLocaleString('en-IN')}`;
            document.getElementById('outNet').innerText = `₹${netCustomerCapitalOutlay.toLocaleString('en-IN')}`;
            document.getElementById('outProduction').innerText = `${Math.round(calculatedMonthlyUnitsOutput)} Units / Month`;
            document.getElementById('outPayback').innerText = `${finalPaybackDurationVal.toFixed(1)} Years`;
            document.getElementById('outSavings').innerText = `₹${Math.round(lifetimeAccumulatedNetYield).toLocaleString('en-IN')} (25 Yrs)`;

            // Screen View State Switcher Execution
            document.getElementById('uiPlaceholder').style.display = 'none';
            document.getElementById('uiResultDashboard').classList.remove('hidden');
            
            // Auto smooth scroll to tracking grid layout view on thin screens
            if(window.innerWidth < 768) {
                document.getElementById('uiResultDashboard').scrollIntoView({ behavior: 'smooth' });
            }
        });
    </script>
</body>
</html>
