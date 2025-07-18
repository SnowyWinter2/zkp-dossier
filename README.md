<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ZKP Foundation: ZKP-001 Dossier</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a1a2e; /* Dark background */
            color: #e0e0e0; /* Light text color */
            line-height: 1.6;
            overflow-x: hidden; /* Prevent horizontal scrollbar from ticker */
        }
        .container {
            max-width: 900px;
        }
        .section-heading {
            border-bottom: 2px solid #3f3f5f; /* Subtle separator */
            padding-bottom: 0.5rem;
            margin-bottom: 1.5rem;
            color: #92e6e6; /* Accent color for headings */
        }
        .card {
            background-color: #2a2a4a; /* Slightly lighter dark background for cards */
            border-radius: 0.75rem; /* Rounded corners */
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
            padding: 2rem;
            margin-bottom: 2rem;
        }
        .zkp-accent-yellow {
            color: #facc15; /* Tailwind yellow-400 */
        }
        .zkp-accent-violet {
            color: #a78bfa; /* Tailwind violet-400 */
        }
        .zkp-accent-green {
            color: #86efac; /* Tailwind green-300 */
        }
        .zkp-accent-red {
            color: #ef4444; /* Tailwind red-500 */
        }
        .redacted {
            background-color: #000;
            color: #000;
            padding: 0 0.5rem;
            border-radius: 0.25rem;
            font-weight: bold;
        }

        /* News Ticker Specific Styling */
        .news-ticker-wrapper {
            background-color: #3f3f5f; /* A dark violet/blue for the news bar */
            border-radius: 0.75rem; /* Rounded corners */
            padding: 0.75rem 1.5rem; /* Reduced padding for a bar */
            margin-bottom: 2rem;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
            border: 1px solid #5f5f7f;
            overflow: hidden; /* Hide content outside the bar */
            white-space: nowrap; /* Keep text on a single line */
            position: relative; /* For absolute positioning of inner text */
            display: flex; /* Use flexbox to center content if needed */
            align-items: center; /* Vertically center content */
            height: 50px; /* Fixed height for the bar */
            cursor: pointer; /* Indicate it's clickable */
        }

        .news-ticker-content {
            display: inline-block; /* Allows content to be wider than its parent */
            padding-left: 100%; /* Start text off-screen to the right */
            animation: ticker-scroll 80s linear infinite; /* Animation for scrolling - SLOWER SPEED */
            font-size: 0.9rem;
            color: #92e6e6; /* Accent color for news text */
            font-weight: 500;
        }

        .news-ticker-content strong {
            color: #facc15; /* Yellow for highlights */
        }
        .news-ticker-content .warning-icon {
            color: #ef4444; /* Red for warning icon */
            margin-right: 0.5rem;
        }

        @keyframes ticker-scroll {
            0% { transform: translate(0, 0); }
            100% { transform: translate(-100%, 0); } /* Scrolls full width of content */
        }

        /* Modal / Info Card Styling */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8); /* Dark semi-transparent background */
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000; /* Ensure it's on top */
        }

        .modal-content {
            background-color: #2a2a4a;
            border-radius: 0.75rem;
            padding: 2rem;
            max-width: 700px;
            max-height: 90vh; /* Limit height to prevent overflow */
            overflow-y: auto; /* Enable scrolling for long content */
            position: relative;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.7);
            border: 1px solid #5f5f7f;
        }

        .modal-close-btn {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: none;
            border: none;
            font-size: 1.5rem;
            color: #e0e0e0;
            cursor: pointer;
            padding: 0.5rem;
            border-radius: 0.5rem;
            transition: background-color 0.2s ease;
        }
        .modal-close-btn:hover {
            background-color: #3f3f5f;
        }

        /* Styling for content within the modal to match news bar */
        .modal-content .news-item {
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px dashed #3f3f5f;
        }
        .modal-content .news-item:last-child {
            border-bottom: none;
            margin-bottom: 0;
            padding-bottom: 0;
        }
        .modal-content .news-item strong {
            color: #facc15;
        }
        .modal-content .warning-icon {
            color: #ef4444;
            margin-right: 0.5rem;
        }
        .modal-content .quote {
            font-style: italic;
            color: #b3f0f0;
            border-left: 3px solid #b3f0f0;
            padding-left: 0.75rem;
            margin: 1rem 0;
        }
        .modal-content .memo-heading {
            color: #86efac;
            font-weight: bold;
        }
        .modal-content .leak-heading {
            color: #a78bfa;
            font-weight: bold;
        }
        .modal-content .prohibited {
            color: #ef4444;
            font-weight: bold;
            margin-top: 1.5rem;
            text-align: center;
        }
        .modal-content ul {
            list-style: disc;
            list-style-position: inside;
            margin-left: 1rem;
        }
    </style>
</head>
<body class="p-4 sm:p-8">
    <div class="container mx-auto">
        <!-- News Ticker Bar -->
        <div id="newsTicker" class="news-ticker-wrapper">
            <div class="news-ticker-content">
                <span class="warning-icon">🛑</span><span class="redacted">CLASSIFIED BROADCAST – LEVEL SIGMA</span><span class="warning-icon">🛑</span> &nbsp;&nbsp;&nbsp; ZKP Internal Emergency Bulletin – DO NOT PUBLICLY DISTRIBUTE &nbsp;&nbsp;&nbsp; <strong>Time Stamp:</strong> <span class="redacted">2025-07-18 14:30 EEST</span> &nbsp;&nbsp;&nbsp; <strong>Origin:</strong> ZKP-CORE – Nyxora Systems Observation & Containment &nbsp;&nbsp;&nbsp; <strong>Subject:</strong> VAB-001-1 (Velmire Ashen Blei) Compromise Detected &nbsp;&nbsp;&nbsp; <span class="warning-icon">⚠️</span> <strong>BREAKING – SPOROGENIC BREACH IN ZONE-9:</strong> Containment of specimen ZKP-001-1.NIX has failed after a temperature fluctuation triggered Velrium Dust aerosolization. All personnel within 10m radius began exhibiting Stage 1 symptoms within 11 minutes. Emergency sealing protocols initiated, but two researchers (ID: <span class="redacted">DR. ELARA VANCE</span>, <span class="redacted">DR. KAI MILLER</span>) are unresponsive. &nbsp;&nbsp;&nbsp; ❝ We thought it was safe. The flower hadn’t bloomed in weeks. The aura changed color before it burst. ❞ – Surveillance Log #V-A/27 &nbsp;&nbsp;&nbsp; <strong>🔍 INTERNAL MEMO: Unconfirmed Mutation:</strong> Dr. <span class="redacted">LIAM CHEN</span> of BioSystems reported an anomaly during a routine spore analysis. One spore cluster contained an unknown 6th stage DNA strand not present in archived VAB samples. This suggests evolution despite the flower being engineered sterile. Investigation pending. &nbsp;&nbsp;&nbsp; <strong>💬 Leaked Excerpt – Urban Sector C Messageboard:</strong> A redacted audio recording surfaced on an encrypted Nyxora chat forum. AI review confirms audio tone and voice match with known Stage 3 host behavior. "It’s... not killing me anymore. I can hear it now. Not speak—hear. It wants something. And I think... I want it too." Leak is currently being traced and scrubbed. &nbsp;&nbsp;&nbsp; <span class="warning-icon">🧬</span> <strong>VAB AEROSPORE RANGE MISCLASSIFIED:</strong> New tests show that a single open flower in oxygen-rich environments can spread viable spores in a 350m radius in under 7 minutes—5x the originally logged distance. Containment protocols are now being urgently revised. &nbsp;&nbsp;&nbsp; <strong>📍 UNKNOWN GRASS PATH APPEARANCE:</strong> A 19m-long grass path lined with decayed VAB flower husks was discovered near an unmarked burial site in the Ashen Basin. No known victims were interred there. The grass was analyzed and showed pre-infection pollen clusters—implying spontaneous generation or symbiotic regrowth. &nbsp;&nbsp;&nbsp; <span class="warning-icon">⛔</span> <strong>CLASSIFIED: UNDER NO CIRCUMSTANCES:</strong> Do NOT inhale ANY dust or visible spores, even in filtered suits. Do NOT incinerate the flower. Smoke accelerates bloodstream absorption. Do NOT bury infected hosts in soil outside of Zone-7B. Do NOT inform the public of Stage 5 gravesite phenomena. &nbsp;&nbsp;&nbsp;
            </div>
        </div>

        <!-- Header -->
        <header class="text-center mb-12">
            <h1 class="text-4xl sm:text-5xl font-bold mb-2 text-white">ZKP Foundation</h1>
            <p class="text-lg sm:text-xl text-gray-400">Zonal Katalysis Protocol</p>
            <p class="text-md sm:text-lg text-gray-500 mt-1">Securing Anomalous Biologies. Protecting Planetary Health.</p>
        </header>

        <!-- ZKP-001 Dossier -->
        <main class="card">
            <h2 class="text-3xl sm:text-4xl font-bold section-heading mb-6 text-white">ZKP-001 Dossier</h2>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-8">
                <div>
                    <p class="text-xl font-semibold mb-1">Item #:</p>
                    <p class="text-2xl font-bold zkp-accent-green">ZKP-001 <span class="text-xl">(V.A.B. - Velmire Ashen Blei)</span></p>
                </div>
                <div>
                    <p class="text-xl font-semibold mb-1">Object Class:</p>
                    <p class="text-2xl font-bold zkp-accent-violet">Bloom</p>
                </div>
            </div>

            <!-- Special Containment Procedures -->
            <section class="mb-8">
                <h3 class="text-2xl font-semibold section-heading">Special Containment Procedures</h3>
                <p class="mb-4">Due to its unique propagation method and severe biological threat, ZKP-001 requires advanced containment protocols to prevent dissemination.</p>
                <ul class="list-disc list-inside ml-4">
                    <li class="mb-2">ZKP-001 specimens are housed in <strong class="zkp-accent-red">hermetically sealed biohazard chambers</strong>, meticulously engineered to replicate atmospheric conditions of its exoplanetary origin, <strong class="zkp-accent-yellow">Nyxora (NIX)</strong>.</li>
                    <li class="mb-2">Precise environmental controls are maintained: <strong class="zkp-accent-green">temperature 12-15°C</strong>, <strong class="zkp-accent-green">humidity 85%</strong>, and <strong class="zkp-accent-green">low UV-filtered light exposure</strong> to inhibit decay and spore release.</li>
                    <li class="mb-2">Chambers utilize <strong class="zkp-accent-red">negative pressure</strong> systems and <strong class="zkp-accent-red">multi-stage HEPA filtration</strong> to meticulously scrub the air, ensuring absolute prevention of spore egress.</li>
                    <li class="mb-2">Access to ZKP-001 containment zones is strictly restricted. Authorized personnel must don <strong class="zkp-accent-red">full Level 4 biohazard suits</strong> equipped with integrated respirators.</li>
                    <li class="mb-2">Direct exposure to ZKP-001 spores or the flower's emitted aura is <strong class="zkp-accent-red">strictly prohibited beyond two minutes</strong>.</li>
                    <li class="mb-2">Any suspected contamination or breach requires <strong class="zkp-accent-red">immediate quarantine</strong> of personnel and the prompt administration of experimental countermeasures developed by our Bio-Containment Division.</li>
                </ul>
            </section>

            <!-- Description -->
            <section class="mb-8">
                <h3 class="text-2xl font-semibold section-heading">Description</h3>
                <p class="mb-4">The entity designated Velmire Ashen Blei (VAB) is a meticulously lab-engineered flora, originating from extraterrestrial biological material recovered from the exoplanet Nyxora (designated VAB-001-1.NIX).</p>
                <ul class="list-disc list-inside ml-4">
                    <li class="mb-2">Notably, ZKP-001 is <strong class="zkp-accent-red">non-reproductive</strong> through traditional means. Its sole propagation mechanism involves the release of <strong class="zkp-accent-red">airborne spores</strong> upon rapid decay, a process triggered by exposure to oxygen.</li>
                    <li class="mb-2">The flower presents with <strong class="zkp-accent-green">five prominent petals</strong> of a delicate pastel green hue, strikingly bordered by <strong class="zkp-accent-yellow">glowing yellow edges</strong>. Intricate <strong class="zkp-accent-violet">violet vein patterns</strong> traverse its surface, converging towards a <strong class="zkp-accent-red">pulsating deep crimson core</strong>.</li>
                    <li class="mb-2">ZKP-001 emits a faint, ethereal <strong class="zkp-accent-violet">whitish-violet aura</strong>, accompanied by a scent described as pleasantly alluring yet demonstrably <strong class="zkp-accent-red">toxic</strong>. Preliminary chemical analysis indicates this aura, while compositionally similar to carbon dioxide, contains unique, unidentified organic compounds.</li>
                </ul>
            </section>

            <!-- Infection and Symptoms -->
            <section class="mb-8">
                <h3 class="text-2xl font-semibold section-heading">Infection and Symptomatology</h3>
                <p class="mb-4">Infection with ZKP-001 is initiated solely through the inhalation of spores released when the flower undergoes its decay process in oxygenated environments.</p>
                <h4 class="text-xl font-semibold mb-2 text-gray-300">Progression Stages:</h4>
                <ol class="list-decimal list-inside ml-4">
                    <li class="mb-2"><strong>Stage I (Initial):</strong> Characterized by acute <strong class="zkp-accent-red">headaches, episodes of fainting</strong>, and an absence of any overt physical symptoms. Onset typically within hours of exposure.</li>
                    <li class="mb-2"><strong>Stage II (Latent Manifestation):</strong> Occurs approximately <strong class="zkp-accent-yellow">three years post-infection</strong>. Marked by the onset of subtle physical alterations, including notable <strong class="zkp-accent-green">increases in stamina and hand strength</strong>. A faint, floral scent begins to emanate from the infected individual.</li>
                    <li class="mb-2"><strong>Stage III (Peak Enhancement):</strong> The infected subject reaches a state of <strong class="zkp-accent-green">peak physical conditioning</strong>, demonstrating significantly enhanced endurance. Subtle, but discernible, <strong class="zkp-accent-violet">shape-shifting abilities</strong> manifest in extremities (e.g., fingers, toes).</li>
                    <li class="mb-2"><strong>Stage IV (Degeneration):</strong> A progressive and debilitating <strong class="zkp-accent-red">deterioration</strong> commences, clinically mirroring advanced cancerous conditions. Individuals experience increasing <strong class="zkp-accent-red">fatigue and systemic weakness</strong>.</li>
                    <li class="mb-2"><strong>Stage V (Terminal):</strong> The final, <strong class="zkp-accent-red">terminal stage</strong> is characterized by severe debilitation, with death imminent within a typical timeframe of <strong class="zkp-accent-red">up to 30 years post-infection</strong>.</li>
                </ol>
                <p class="mt-4"><strong>Post-Mortem Phenomenon:</strong> Should an infected subject be interred post-mortem, a distinctive ecological anomaly may occur: a path of <strong class="zkp-accent-green">vibrant green grass, tipped with violet</strong>, alongside patches of short-lived Velmire Ashen Blei flowers, may spontaneously sprout from the grave site. These post-mortem blooms are ephemeral due to immediate oxygen exposure.</p>
            </section>

            <!-- Specimens On Record -->
            <section class="mb-8">
                <h3 class="text-2xl font-semibold section-heading">Specimens On Record</h3>
                <ul class="list-disc list-inside ml-4">
                    <li class="mb-2"><strong>ZKP-001-1:</strong> The <strong class="zkp-accent-yellow">original flower specimen</strong>, retrieved directly from the exoplanet Nyxora. Serves as the genetic baseline.</li>
                    <li class="mb-2"><strong>ZKP-001-1.NIX:</strong> A specific <strong class="zkp-accent-red">Nyxora strain</strong> of ZKP-001, exhibiting considerably heightened spore potency and a more intense aura emission compared to other observed variations.</li>
                    <li class="mb-2"><strong>ZKP-001-2:</strong> The <strong class="zkp-accent-violet">first confirmed human subject</strong> infected by ZKP-001 (distinct from Patient Zero, whose identity remains classified). This individual currently exhibits advanced Stage III symptoms, including pronounced altered pigmentation and early-stage extremity shape-shifting.</li>
                </ul>
            </section>

            <!-- Addendum 001-A: Mythology and Origins -->
            <section class="mb-8">
                <h3 class="text-2xl font-semibold section-heading">Addendum 001-A: Mythology and Origins</h3>
                <p class="mb-4">Field reports and recovered extraterrestrial documents provide crucial insights into the presumed origins of Velmire Ashen Blei. The flower is posited to have been <strong class="zkp-accent-yellow">artificially created within a laboratory setting</strong>, utilizing sophisticated biological materials not indigenous to the Milky Way galaxy.</p>
                <p class="mb-4">Local Nyxoran myths frequently refer to its enigmatic creators as "<strong class="zkp-accent-red">monsters</strong>" – a term whose precise meaning remains under investigation, potentially metaphorically denoting advanced scientists or an unknown, non-humanitarian entity responsible for its development.</p>
                <p>Ancient legends suggest a chilling correlation: the flower's infectious properties are believed to instigate <strong class="zkp-accent-red">further monstrous transformations</strong> within its hosts, extending beyond mere physical alteration.</p>
            </section>

            <!-- Research Notes & Current Status -->
            <section>
                <h3 class="text-2xl font-semibold section-heading">Research Notes & Current Status</h3>
                <p class="mb-4">Ongoing research efforts are predominantly focused on two critical objectives: the development of an <strong class="zkp-accent-green">effective antidote or counter-agent</strong> to reverse or mitigate infection, and the refinement of <strong class="zkp-accent-green">advanced containment strategies</strong> to decisively prevent any form of spore dissemination.</p>
                <p>Given the flower's rapid decay upon oxygen exposure and the potent infectious nature of its spores, any <strong class="zkp-accent-red">containment breaches represent a significant, catastrophic threat</strong> to global public health and safety.</p>
            </section>
        </main>

        <!-- Footer -->
        <footer class="text-center mt-12 text-gray-500 text-sm">
            <p>&copy; 2025 ZKP Foundation. All Rights Reserved. Classified Information. Unauthorized Access Prohibited.</p>
        </footer>
    </div>

    <!-- Modal / Info Card Structure -->
    <div id="newsModal" class="modal-overlay hidden">
        <div class="modal-content">
            <button id="closeModalBtn" class="modal-close-btn">&times;</button>
            <h3 class="text-2xl text-center mb-4 text-red-500">🛑 CLASSIFIED BROADCAST – LEVEL SIGMA 🛑</h3>
            <p class="text-center text-lg font-semibold mb-4 text-gray-300">ZKP Internal Emergency Bulletin – DO NOT PUBLICLY DISTRIBUTE</p>

            <div class="news-item">
                <p><strong>Time Stamp:</strong> <span class="redacted">2025-07-18 14:30 EEST</span></p>
                <p><strong>Origin:</strong> ZKP-CORE – Nyxora Systems Observation & Containment</p>
                <p><strong>Subject:</strong> VAB-001-1 (Velmire Ashen Blei) Compromise Detected</p>
            </div>

            <div class="news-item">
                <p class="warning-icon text-xl">⚠️ <strong>BREAKING – SPOROGENIC BREACH IN ZONE-9:</strong></p>
                <p class="ml-4">Containment of specimen ZKP-001-1.NIX has failed after a temperature fluctuation triggered Velrium Dust aerosolization. All personnel within 10m radius began exhibiting Stage 1 symptoms within 11 minutes. Emergency sealing protocols initiated, but two researchers (ID: <span class="redacted">DR. ELARA VANCE</span>, <span class="redacted">DR. KAI MILLER</span>) are unresponsive.</p>
                <p class="quote">❝ We thought it was safe. The flower hadn’t bloomed in weeks. The aura changed color before it burst. ❞</p>
                <p class="text-right text-gray-400 text-sm">– Surveillance Log #V-A/27</p>
            </div>

            <div class="news-item">
                <p class="memo-heading text-xl">🔍 INTERNAL MEMO: Unconfirmed Mutation:</p>
                <p class="ml-4">Dr. <span class="redacted">LIAM CHEN</span> of BioSystems reported an anomaly during a routine spore analysis. One spore cluster contained an unknown 6th stage DNA strand not present in archived VAB samples. This suggests evolution despite the flower being engineered sterile. Investigation pending.</p>
            </div>

            <div class="news-item">
                <p class="leak-heading text-xl">💬 Leaked Excerpt – Urban Sector C Messageboard:</p>
                <p class="ml-4">A redacted audio recording surfaced on an encrypted Nyxora chat forum. AI review confirms audio tone and voice match with known Stage 3 host behavior.</p>
                <p class="quote ml-4">"It’s... not killing me anymore. I can hear it now. Not speak—hear. It wants something. And I think... I want it too."</p>
                <p class="ml-4">Leak is currently being traced and scrubbed.</p>
            </div>

            <div class="news-item">
                <p class="warning-icon text-xl">🧬 <strong>VAB AEROSPORE RANGE MISCLASSIFIED:</strong></p>
                <p class="ml-4">New tests show that a single open flower in oxygen-rich environments can spread viable spores in a 350m radius in under 7 minutes—5x the originally logged distance. Containment protocols are now being urgently revised.</p>
            </div>

            <div class="news-item">
                <p class="memo-heading text-xl">📍 UNKNOWN GRASS PATH APPEARANCE:</p>
                <p class="ml-4">A 19m-long grass path lined with decayed VAB flower husks was discovered near an unmarked burial site in the Ashen Basin. No known victims were interred there.</p>
                <p class="ml-4">The grass was analyzed and showed pre-infection pollen clusters—implying spontaneous generation or symbiotic regrowth.</p>
            </div>

            <div class="news-item">
                <p class="prohibited text-xl">⛔ CLASSIFIED: UNDER NO CIRCUMSTANCES:</p>
                <ul class="list-disc list-inside ml-8 text-left">
                    <li>Do NOT inhale ANY dust or visible spores, even in filtered suits.</li>
                    <li>Do NOT incinerate the flower. Smoke accelerates bloodstream absorption.</li>
                    <li>Do NOT bury infected hosts in soil outside of Zone-7B.</li>
                    <li>Do NOT inform the public of Stage 5 gravesite phenomena.</li>
                </ul>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const newsTicker = document.getElementById('newsTicker');
            const newsModal = document.getElementById('newsModal');
            const closeModalBtn = document.getElementById('closeModalBtn');

            // Show modal when ticker is clicked
            newsTicker.addEventListener('click', () => {
                newsModal.classList.remove('hidden');
            });

            // Hide modal when close button is clicked
            closeModalBtn.addEventListener('click', () => {
                newsModal.classList.add('hidden');
            });

            // Optional: Hide modal if clicking outside the modal content
            newsModal.addEventListener('click', (e) => {
                if (e.target === newsModal) {
                    newsModal.classList.add('hidden');
                }
            });
        });
    </script>
</body>
</html>
