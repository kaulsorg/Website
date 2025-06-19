<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basic Hindu Calendar</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the calendar */
        body {
            font-family: 'Inter', sans-serif; /* Using Inter font */
            background-color: #f0f2f5; /* Light gray background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh; /* Full viewport height */
            margin: 0;
            padding: 20px; /* Padding for mobile view */
            box-sizing: border-box;
        }
        .calendar-container {
            background-color: #ffffff; /* White background for the calendar card */
            border-radius: 1.5rem; /* More rounded corners */
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); /* Soft shadow */
            padding: 2rem; /* Ample padding */
            max-width: 500px; /* Max width for better readability on large screens */
            width: 100%; /* Full width on small screens */
            text-align: center;
            border: 1px solid #e2e8f0; /* Light border */
        }
        .header-section {
            background-color: #4f46e5; /* Indigo background for header */
            color: #ffffff;
            padding: 1.5rem 1rem;
            border-radius: 1rem; /* Slightly less rounded than container */
            margin-bottom: 2rem;
            box-shadow: inset 0 -2px 4px rgba(0, 0, 0, 0.1); /* Inner shadow */
        }
        .info-card {
            background-color: #edf2f7; /* Lighter gray for info cards */
            border-radius: 0.75rem;
            padding: 1rem;
            margin-bottom: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 1.1rem;
            color: #2d3748; /* Darker text for info */
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06); /* Subtle shadow */
        }
        .info-card span:first-child {
            font-weight: 600; /* Bold label */
            color: #4a5568; /* Slightly darker for label */
        }
        .approximation-note {
            font-size: 0.9rem;
            color: #718096; /* Gray text for notes */
            margin-top: 1.5rem;
            line-height: 1.5;
        }
        @media (max-width: 640px) {
            .calendar-container {
                padding: 1.5rem;
                border-radius: 1rem;
            }
            .header-section {
                padding: 1rem;
                margin-bottom: 1.5rem;
            }
            .info-card {
                flex-direction: column; /* Stack items on small screens */
                align-items: flex-start;
                padding: 0.75rem;
            }
            .info-card span:first-child {
                margin-bottom: 0.25rem; /* Space between label and value */
            }
        }
    </style>
</head>
<body>
    <div class="calendar-container">
        <div class="header-section">
            <h1 class="text-3xl font-bold mb-2">Hindu Calendar</h1>
            <p class="text-xl" id="gregorian-date"></p>
        </div>

        <div class="space-y-4">
            <div class="info-card">
                <span>Hindu Year:</span>
                <span id="hindu-year" class="font-medium"></span>
            </div>
            <div class="info-card">
                <span>Paksha:</span>
                <span id="paksha" class="font-medium"></span>
            </div>
            <div class="info-card">
                <span>Tithi:</span>
                <span id="tithi" class="font-medium"></span>
            </div>
            <div class="info-card">
                <span>Nakshatra:</span>
                <span id="nakshatra" class="font-medium"></span>
            </div>
            <div class="info-card">
                <span>Yoga:</span>
                <span id="yoga" class="font-medium">N/A (Advanced)</span>
            </div>
            <div class="info-card">
                <span>Karana:</span>
                <span id="karana" class="font-medium">N/A (Advanced)</span>
            </div>
        </div>

        <p class="approximation-note">
            Note: This is a basic demonstration. Accurate Tithi, Nakshatra, Yoga, and Karana calculations require precise astronomical data (planetary positions, sunrise/sunset) and often vary by region and calendar system.
            The values shown here are simplified approximations based on lunar cycles.
        </p>
    </div>

    <script>
        // Function to display the current Gregorian date
        function displayGregorianDate() {
            const today = new Date();
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('gregorian-date').textContent = today.toLocaleDateString('en-US', options);
        }

        // --- Simplified Hindu Calendar Logic ---
        // This is a highly simplified approach and NOT astronomically accurate.
        // It's for demonstration purposes only.

        // Known New Moon reference date (May 8, 2024, 00:00:00 UTC)
        // This is a fixed reference point for approximate lunar cycle calculations.
        const newMoonReference = new Date('2024-05-08T00:00:00Z');
        const synodicMonthDays = 29.530588; // Average length of a synodic month in days

        const nakshatras = [
            "Ashwini", "Bharani", "Krittika", "Rohini", "Mrigashira", "Ardra", "Punarvasu",
            "Pushya", "Ashlesha", "Magha", "Purva Phalguni", "Uttara Phalguni", "Hasta",
            "Chitra", "Swati", "Vishakha", "Anuradha", "Jyeshtha", "Moola", "Purva Ashadha",
            "Uttara Ashadha", "Shravana", "Dhanishta", "Shatabhisha", "Purva Bhadrapada",
            "Uttara Bhadrapada", "Revati"
        ];

        function calculateHinduDateElements() {
            const now = new Date();
            // Calculate total milliseconds since reference New Moon
            const msSinceNewMoon = now.getTime() - newMoonReference.getTime();
            // Convert to days
            const daysSinceNewMoon = msSinceNewMoon / (1000 * 60 * 60 * 24);

            // Calculate current lunar cycle position (0 to < synodicMonthDays)
            const currentLunarCycleDay = daysSinceNewMoon % synodicMonthDays;

            // --- Tithi Calculation (Simplified) ---
            // There are 30 Tithis in a lunar month.
            const tithiIndex = Math.floor((currentLunarCycleDay / synodicMonthDays) * 30);
            const tithis = [
                "Pratipada", "Dwitiya", "Tritiya", "Chaturthi", "Panchami", "Shashthi", "Saptami",
                "Ashtami", "Navami", "Dashami", "Ekadashi", "Dwadashi", "Trayodashi", "Chaturdashi",
                "Purnima (Full Moon)", "Pratipada", "Dwitiya", "Tritiya", "Chaturthi", "Panchami",
                "Shashthi", "Saptami", "Ashtami", "Navami", "Dashami", "Ekadashi", "Dwadashi",
                "Trayodashi", "Chaturdashi", "Amavasya (New Moon)"
            ];

            let displayTithi = tithis[tithiIndex];

            // --- Paksha Calculation ---
            let paksha = "";
            if (tithiIndex < 15) {
                paksha = "Shukla Paksha (Bright Half)"; // From New Moon to Full Moon
            } else {
                paksha = "Krishna Paksha (Dark Half)"; // From Full Moon to New Moon
            }

            // Correct Tithi for Krishna Paksha
            if (tithiIndex >= 15) {
                displayTithi = tithis[tithiIndex]; // Keep original name
            }

            // --- Nakshatra Calculation (Simplified) ---
            // Roughly map lunar cycle to 27 Nakshatras. This is a crude approximation.
            const nakshatraIndex = Math.floor((currentLunarCycleDay / synodicMonthDays) * nakshatras.length) % nakshatras.length;
            const displayNakshatra = nakshatras[nakshatraIndex];

            // --- Hindu Year (Placeholder) ---
            // Calculating the exact Hindu year (e.g., Vikram Samvat) requires specific
            // knowledge of its starting point relative to the Gregorian calendar
            // and often involves adjustments. This is a simplified placeholder.
            // Example: Vikram Samvat is approximately 57 years ahead of the Gregorian calendar.
            const gregorianYear = now.getFullYear();
            const hinduYear = gregorianYear + 57; // A common approximation for Vikram Samvat

            document.getElementById('hindu-year').textContent = `Vikram Samvat ${hinduYear}`;
            document.getElementById('paksha').textContent = paksha;
            document.getElementById('tithi').textContent = displayTithi;
            document.getElementById('nakshatra').textContent = displayNakshatra;
        }

        // Initialize on window load
        window.onload = function() {
            displayGregorianDate();
            calculateHinduDateElements();
            // You can add a setInterval here to update every minute/hour if needed
            // setInterval(calculateHinduDateElements, 60000); // Updates every minute
        };
    </script>
</body>
</html>
