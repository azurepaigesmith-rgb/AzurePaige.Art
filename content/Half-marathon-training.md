---
title: Half Marathon training
slug: Half-marathon-training
description: What better way to see nature than to trail run through it.
menu: Tech Tinkers
allowHtml: true
---

My SIL invited me to run a half marthon with her. I had 20 weeks to do it, as someone of decent physical fitness and in the midst of fun-employment I figured why not! I paircoded a training plan and away I went. This is the result of the paircoding and me doing my best to stick to the plan.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Trail Half Marathon Plan</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Calm Harmony -->
    <!-- Application Structure Plan: The application is designed as a single-page dashboard with a tabbed interface. This structure was chosen to break down the dense training plan into logical, digestible sections: 1) An interactive weekly schedule, 2) a visual progress chart, and 3) a collapsible tips section. This avoids overwhelming the user with a long scroll of information and allows them to focus on one aspect of their training at a time. The user flow is simple: select a tab to change the main content view, and within the plan, select a specific week to see details. This is more intuitive and mobile-friendly than a static document. -->
    <!-- Visualization & Content Choices: 
        - Report Info: 20-week training table -> Goal: Provide an easy-to-navigate weekly schedule -> Viz/Presentation: Interactive week selector with a detail display area -> Interaction: User clicks a week number to instantly view that week's three workouts. -> Justification: Replaces a long, hard-to-scan table with a focused, on-demand view, improving usability. -> Library/Method: Vanilla JS DOM manipulation.
        - Report Info: Long run mileage progression -> Goal: Visualize the increase in endurance over time -> Viz/Presentation: Line chart showing mileage per week -> Interaction: Hover tooltips display exact mileage for each week. -> Justification: A line chart clearly and immediately communicates the training load, including build-up, peak, and taper phases. -> Library/Method: Chart.js (Canvas).
        - Report Info: Sections on Strength, Technique, Gear, Nutrition -> Goal: Organize supplementary content without cluttering the main view -> Viz/Presentation: Accordion component -> Interaction: User clicks a topic header to expand and read the details. -> Justification: An accordion is a space-efficient way to present detailed text, allowing users to explore tips as needed. -> Library/Method: Vanilla JS toggle classes.
    -->

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F7F4;
            color: #3D405B;
        }
        .tab-active {
            background-color: #81B29A;
            color: #FFFFFF;
        }
        .tab-inactive {
            background-color: #E0E0E0;
            color: #3D405B;
        }
        .week-button-active {
            background-color: #3D405B;
            color: #FFFFFF;
            transform: scale(1.05);
        }
        .week-button-inactive {
            background-color: #FDFCFB;
            color: #3D405B;
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
    </style>
</head>
<body class="antialiased">
    <div class="container mx-auto p-4 sm:p-6 md:p-8">

        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-[#3D405B] mb-2">19-Week Trail Half Marathon Plan</h1>
            <p class="text-lg text-gray-600">Your interactive guide from a 5k runner to a trail half marathon finisher.</p>
        </header>

        <div class="w-full max-w-4xl mx-auto">
            <div class="mb-6 flex justify-center rounded-lg p-1 bg-gray-200 shadow-inner">
                <button data-tab="plan" class="tab-button flex-1 py-2 px-4 text-center font-semibold rounded-md transition-colors duration-300">The Plan</button>
                <button data-tab="progress" class="tab-button flex-1 py-2 px-4 text-center font-semibold rounded-md transition-colors duration-300">Progress</button>
                <button data-tab="tips" class="tab-button flex-1 py-2 px-4 text-center font-semibold rounded-md transition-colors duration-300">Essential Tips</button>
            </div>

            <div id="plan" class="tab-content">
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <div class="text-center mb-6">
                        <h2 class="text-2xl font-bold text-[#3D405B]">Weekly Schedule</h2>
                        <p class="text-gray-600 mt-1">Select a week to view your detailed training plan. This schedule is your roadmap, designed to build endurance and strength gradually and safely.</p>
                    </div>
                    <div id="week-selector" class="flex flex-wrap justify-center gap-2 mb-6"></div>
                    <div id="week-details" class="transition-opacity duration-500"></div>
                </div>
            </div>

            <div id="progress" class="tab-content hidden">
                <div class="bg-white p-6 rounded-xl shadow-lg">
                     <div class="text-center mb-4">
                        <h2 class="text-2xl font-bold text-[#3D405B]">Long Run Progression</h2>
                        <p class="text-gray-600 mt-1 max-w-2xl mx-auto">This chart visualizes the cornerstone of your training: the weekly long run. Watch your endurance build over time, with planned recovery weeks to help your body adapt and a final taper to ensure you're fresh for race day.</p>
                    </div>
                    <div class="chart-container">
                        <canvas id="progressChart"></canvas>
                    </div>
                </div>
            </div>

            <div id="tips" class="tab-content hidden">
                 <div class="bg-white p-6 rounded-xl shadow-lg">
                    <div class="text-center mb-6">
                        <h2 class="text-2xl font-bold text-[#3D405B]">Essential Tips for Success</h2>
                        <p class="text-gray-600 mt-1">Trail running involves more than just logging miles. These tips cover crucial areas like strength, technique, and nutrition to help you stay healthy, strong, and prepared for the unique challenges of the trail.</p>
                    </div>
                    <div id="accordion-container" class="space-y-4"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // Training data reverted and starting from Oct 21, 2025
            const trainingData = [
                { week: 1, date: 'Oct 21, 2025', run1: '2 miles easy', run2: '2 miles with 4x30s hill efforts', longRun: '3 miles easy trail run', longRunDist: 3 },
                { week: 2, date: 'Oct 28', run1: '2.5 miles easy', run2: '2 miles easy', longRun: '3.5 miles trail run', longRunDist: 3.5 },
                { week: 3, date: 'Nov 4', run1: '2.5 miles easy', run2: '2.5 miles with 4x45s hill efforts', longRun: '4 miles trail run', longRunDist: 4 },
                { week: 4, date: 'Nov 11', run1: '2 miles easy', run2: '2 miles easy', longRun: '3 miles trail run', longRunDist: 3, recovery: true },
                { week: 5, date: 'Nov 18', run1: '3 miles easy', run2: '3 miles with 4x60s hill repeats', longRun: '5 miles trail run', longRunDist: 5 },
                { week: 6, date: 'Nov 25', run1: '3 miles easy', run2: '3 miles total: 1 easy, 1 tempo, 1 easy', longRun: '6 miles trail run', longRunDist: 6 },
                { week: 7, date: 'Dec 2', run1: '3.5 miles easy', run2: '3 miles with 5x60s hill repeats', longRun: '7 miles trail run', longRunDist: 7 },
                { week: 8, date: 'Dec 9', run1: '2.5 miles easy', run2: '2 miles easy', longRun: '5 miles trail run', longRunDist: 5, recovery: true },
                { week: 9, date: 'Dec 16', run1: '3.5 miles easy', run2: '4 miles total: 1E, 2T, 1E', longRun: '8 miles trail run', longRunDist: 8 },
                { week: 10, date: 'Dec 23', run1: '4 miles easy', run2: '3.5 miles with 6x60s hill repeats', longRun: '9 miles trail run', longRunDist: 9 },
                { week: 11, date: 'Dec 30', run1: '4 miles easy', run2: '4 miles total: 1E, 2T, 1E', longRun: '10 miles trail run', longRunDist: 10 },
                { week: 12, date: 'Jan 6, 2026', run1: '3 miles easy', run2: '2.5 miles easy', longRun: '7 miles trail run', longRunDist: 7, recovery: true },
                { week: 13, date: 'Jan 13', run1: '4 miles easy', run2: '4 miles with rolling hills', longRun: '11 miles trail run', longRunDist: 11 },
                { week: 14, date: 'Jan 20', run1: '4.5 miles total: 1E, 2.5T, 1E', run2: '4 miles with 4x90s hill repeats', longRun: '13.1 miles trail run', longRunDist: 13.1 },
                { week: 15, date: 'Jan 27', run1: '4 miles easy', run2: '3 miles easy with rolling hills', longRun: '10 miles trail run (slight cutback)', longRunDist: 10 },
                { week: 16, date: 'Feb 3', run1: '4.5 miles easy', run2: '4 miles with rolling hills', longRun: '11 miles trail run', longRunDist: 11 },
                { week: 17, date: 'Feb 10', run1: '3 miles easy', run2: '2.5 miles easy', longRun: '8 miles trail run', longRunDist: 8, recovery: true },
                { week: 18, date: 'Feb 17', run1: '3 miles easy', run2: '2 miles easy + 4 strides', longRun: '6 miles trail run', longRunDist: 6 },
                { week: 19, date: 'Feb 24', run1: '2 miles easy', run2: '1.5 miles easy', longRun: '4-5 miles very easy trail run', longRunDist: 4.5, taper: true },
                { week: 20, date: 'Mar 3', run1: '20 min very easy run', run2: 'REST', longRun: 'RACE DAY! (Mar 7th)', longRunDist: 13.1, race: true },
            ];

            const tipsData = [
                {
                    title: 'Strength & Cross-Training (1-2 days/week)',
                    content: `Trail running requires more stability and single-leg strength than road running. On your non-running days, incorporate 20-30 minutes of strength work.
                        <ul class="list-disc list-inside mt-2 space-y-1">
                            <li><b>Core:</b> Planks, side planks, bird-dog, dead bug.</li>
                            <li><b>Legs:</b> Bodyweight squats, lunges (forward and lateral), glute bridges, and calf raises.</li>
                            <li><b>Stability:</b> Single-leg deadlifts (no weight needed at first) and standing on one foot.</li>
                            <li><b>Cross-Training:</b> Hiking is a perfect cross-training activity! It uses similar muscles but with less impact. Yoga or cycling are also great options.</li>
                        </ul>`
                },
                {
                    title: 'Trail Technique',
                    content: `
                        <ul class="list-disc list-inside mt-2 space-y-2">
                            <li><b>Look Ahead:</b> Don't stare at your feet. Scan the trail 10-15 feet ahead of you to pick your line and anticipate obstacles.</li>
                            <li><b>Uphills:</b> Shorten your stride, stay upright, and use your arms. It is often more efficient to "power hike" steep sections than to run them. Don't be afraid to walk!</li>
                            <li><b>Downhills:</b> Avoid leaning back and braking with your heels. Try to stay more perpendicular to the slope, keep your feet landing underneath you, and use your arms for balance. Think "controlled falling."</li>
                            <li><b>Lift Your Feet:</b> Roots and rocks love to grab toes. Consciously lift your feet a little higher than you would on the road.</li>
                        </ul>`
                },
                {
                    title: 'Gear Recommendations',
                    content: `
                        <ul class="list-disc list-inside mt-2 space-y-1">
                            <li><b>Trail Running Shoes:</b> The most important investment. They offer better grip, protection, and stability than road shoes.</li>
                            <li><b>Hydration Vest or Pack:</b> For any run over 60-75 minutes, you'll need to carry water and fuel.</li>
                            <li><b>Socks:</b> Non-cotton, moisture-wicking socks are a must to prevent blisters.</li>
                            <li><b>Anti-Chafe Balm:</b> Useful for long runs.</li>
                        </ul>`
                },
                {
                    title: 'Nutrition & Hydration',
                    content: `
                        <ul class="list-disc list-inside mt-2 space-y-1">
                            <li><b>Practice on Long Runs:</b> Use your long runs to figure out what fuel works for you. Don't try anything new on race day!</li>
                            <li><b>General Guideline:</b> For runs over 90 minutes, aim to consume 30-60 grams of carbohydrates per hour. This can be from gels, chews, or real food.</li>
                            <li><b>Hydrate:</b> Sip water from the start of your long runs. If it's hot, consider adding an electrolyte tablet to your water.</li>
                        </ul>`
                }
            ];
            
            let currentWeek = 1;
            const weekSelector = document.getElementById('week-selector');
            const weekDetailsContainer = document.getElementById('week-details');
            const tabButtons = document.querySelectorAll('.tab-button');
            const tabContents = document.querySelectorAll('.tab-content');
            const accordionContainer = document.getElementById('accordion-container');

            function renderWeekSelector() {
                weekSelector.innerHTML = '';
                trainingData.forEach(weekData => {
                    const button = document.createElement('button');
                    button.dataset.week = weekData.week;
                    button.textContent = weekData.week;
                    let weekTypeClasses = 'border-gray-300';
                    if (weekData.recovery) weekTypeClasses = 'border-blue-400';
                    if (weekData.taper) weekTypeClasses = 'border-green-400';
                    if (weekData.race) weekTypeClasses = 'border-red-500 font-bold';
                    
                    button.className = `week-button h-10 w-10 sm:h-12 sm:w-12 flex-shrink-0 items-center justify-center rounded-full border-2 text-sm font-semibold transition-all duration-200 shadow-sm ${weekTypeClasses}`;
                    weekSelector.appendChild(button);
                });
            }

            function updateActiveWeekButton() {
                document.querySelectorAll('.week-button').forEach(btn => {
                    if (parseInt(btn.dataset.week) === currentWeek) {
                        btn.classList.add('week-button-active');
                        btn.classList.remove('week-button-inactive');
                    } else {
                        btn.classList.add('week-button-inactive');
                        btn.classList.remove('week-button-active');
                    }
                });
            }
            
            function renderWeekDetails() {
                const data = trainingData[currentWeek - 1];
                let title = `Week ${data.week}`;
                if (data.recovery) title += ' (Recovery)';
                if (data.taper) title += ' (Taper)';
                if (data.race) title = 'Race Week!';

                weekDetailsContainer.style.opacity = 0;
                setTimeout(() => {
                    weekDetailsContainer.innerHTML = `
                        <div class="text-center border-t border-gray-200 pt-6">
                             <h3 class="text-xl font-bold text-[#3D405B]">${title}</h3>
                             <p class="text-sm text-gray-500 mb-4">Week of ${data.date}</p>
                             <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-left">
                                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                    <h4 class="font-bold text-gray-700">Run 1: Easy/Tempo</h4>
                                    <p class="text-gray-600">${data.run1}</p>
                                </div>
                                <div class="bg-gray-50 p-4 rounded-lg border border-gray-200">
                                    <h4 class="font-bold text-gray-700">Run 2: Hills/Strength</h4>
                                    <p class="text-gray-600">${data.run2}</p>
                                </div>
                                <div class="bg-[#81B29A] bg-opacity-20 p-4 rounded-lg border border-[#81B29A]">
                                    <h4 class="font-bold text-[#3D405B]">Run 3: Long Run</h4>
                                    <p class="text-gray-800">${data.longRun}</p>
                                </div>
                             </div>
                        </div>
                    `;
                    weekDetailsContainer.style.opacity = 1;
                }, 200);
            }

            function setupTabs() {
                tabButtons.forEach(button => {
                    button.addEventListener('click', () => {
                        const targetTab = button.dataset.tab;

                        tabContents.forEach(content => {
                            content.classList.toggle('hidden', content.id !== targetTab);
                        });

                        tabButtons.forEach(btn => {
                            btn.classList.toggle('tab-active', btn.dataset.tab === targetTab);
                            btn.classList.toggle('tab-inactive', btn.dataset.tab !== targetTab);
                        });
                    });
                });
                document.querySelector('.tab-button[data-tab="plan"]').click();
            }

            function renderAccordion() {
                 tipsData.forEach(tip => {
                    const item = document.createElement('div');
                    item.className = 'border border-gray-200 rounded-lg';
                    item.innerHTML = `
                        <button class="accordion-header w-full flex justify-between items-center text-left p-4 font-semibold bg-gray-50 hover:bg-gray-100 rounded-lg">
                            <span>${tip.title}</span>
                            <span class="accordion-icon text-xl font-light transition-transform duration-300">+</span>
                        </button>
                        <div class="accordion-content">
                            <div class="p-4 border-t border-gray-200 text-gray-700">
                                ${tip.content}
                            </div>
                        </div>
                    `;
                    accordionContainer.appendChild(item);
                });

                document.querySelectorAll('.accordion-header').forEach(header => {
                    header.addEventListener('click', () => {
                        const content = header.nextElementSibling;
                        const icon = header.querySelector('.accordion-icon');
                        const isOpen = content.style.maxHeight;

                        document.querySelectorAll('.accordion-content').forEach(c => {
                            if (c !== content) {
                                c.style.maxHeight = null;
                                c.previousElementSibling.querySelector('.accordion-icon').textContent = '+';
                                c.previousElementSibling.querySelector('.accordion-icon').classList.remove('rotate-45');
                            }
                        });

                        if (isOpen) {
                            content.style.maxHeight = null;
                            icon.textContent = '+';
                            icon.classList.remove('rotate-45');
                        } else {
                            content.style.maxHeight = content.scrollHeight + "px";
                            icon.textContent = '+';
                            icon.classList.add('rotate-45');
                        }
                    });
                });
            }

            function createProgressChart() {
                const ctx = document.getElementById('progressChart').getContext('2d');
                const labels = trainingData.map(w => `W${w.week}`);
                const data = trainingData.map(w => w.longRunDist);

                new Chart(ctx, {
                    type: 'line',
                    data: {
                        labels: labels,
                        datasets: [{
                            label: 'Long Run Distance (miles)',
                            data: data,
                            borderColor: '#81B29A',
                            backgroundColor: 'rgba(129, 178, 154, 0.1)',
                            fill: true,
                            tension: 0.3,
                            borderWidth: 3,
                            pointBackgroundColor: '#3D405B',
                            pointRadius: 4,
                            pointHoverRadius: 7
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: { display: true, text: 'Distance (miles)', font: { size: 14 } }
                            },
                            x: {
                                title: { display: true, text: 'Training Week', font: { size: 14 } }
                            }
                        },
                        plugins: {
                            legend: { display: false },
                            tooltip: {
                                displayColors: false,
                                callbacks: {
                                    title: function(context) {
                                        return `Week ${context[0].label.substring(1)}`;
                                    },
                                    label: function(context) {
                                        return `Long Run: ${context.raw} miles`;
                                    }
                                }
                            }
                        }
                    }
                });
            }
            
            weekSelector.addEventListener('click', (e) => {
                if (e.target.matches('.week-button')) {
                    currentWeek = parseInt(e.target.dataset.week);
                    updateActiveWeekButton();
                    renderWeekDetails();
                }
            });

            function init() {
                renderWeekSelector();
                updateActiveWeekButton();
                renderWeekDetails();
                setupTabs();
                renderAccordion();
                createProgressChart();
            }

            init();
        });
    </script>
</body>
</html>