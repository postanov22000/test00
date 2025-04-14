<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EliteSeller Qualifier | Luxury Lead Filtering</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/noUiSlider/15.6.1/nouislider.min.css">
</head>
<body class="font-['Roboto'] bg-gradient-to-br from-gray-900 to-gray-800 min-h-screen">
    
    <!-- Hero Section -->
    <header class="container mx-auto px-4 py-16 text-center">
        <h1 class="text-5xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-amber-400 to-amber-600 mb-6 animate-fade-in">
            EliteSeller Qualifier
        </h1>
        <p class="text-xl text-gray-300 mb-8">AI-Powered Lead Qualification for Top Performing Agents</p>
        
        <!-- Stats Counter -->
        <div class="flex justify-center gap-8 mb-12">
            <div class="bg-gray-800 p-6 rounded-2xl shadow-xl transform hover:scale-105 transition-all">
                <div class="text-4xl font-bold text-amber-500" id="qualifiedCount">0</div>
                <div class="text-gray-400 text-sm">Qualified Leads</div>
            </div>
            <div class="bg-gray-800 p-6 rounded-2xl shadow-xl transform hover:scale-105 transition-all">
                <div class="text-4xl font-bold text-emerald-500">$2.4B</div>
                <div class="text-gray-400 text-sm">Closed Volume</div>
            </div>
        </div>
    </header>

    <!-- Filter Section -->
    <section class="container mx-auto px-4 mb-16">
        <div class="bg-gray-800 rounded-3xl p-8 shadow-2xl">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-8">
                <!-- Price Range -->
                <div class="space-y-4">
                    <label class="block text-amber-500 font-medium">Price Range ($)</label>
                    <div id="priceRange" class="h-2 bg-gray-700 rounded-lg"></div>
                    <div class="flex justify-between text-gray-400 text-sm">
                        <span id="priceMin">$500k</span>
                        <span id="priceMax">$10M+</span>
                    </div>
                </div>

                <!-- Property Type -->
                <div class="space-y-4">
                    <label class="block text-amber-500 font-medium">Property Type</label>
                    <select class="w-full bg-gray-700 text-gray-300 rounded-lg p-3 border border-gray-600 focus:border-amber-500 focus:ring-2 focus:ring-amber-500">
                        <option>All Properties</option>
                        <option>Luxury Single Family</option>
                        <option>Penthouse</option>
                        <option>Waterfront Estate</option>
                        <option>Commercial Complex</option>
                    </select>
                </div>

                <!-- Days on Market -->
                <div class="space-y-4">
                    <label class="block text-amber-500 font-medium">Days on Market</label>
                    <div id="daysOnMarket" class="h-2 bg-gray-700 rounded-lg"></div>
                    <div class="flex justify-between text-gray-400 text-sm">
                        <span id="daysMin">0-30</span>
                        <span id="daysMax">180+</span>
                    </div>
                </div>
            </div>

            <!-- Advanced Filters -->
            <div class="border-t border-gray-700 pt-8">
                <h3 class="text-amber-500 text-xl font-bold mb-6">Advanced Qualification</h3>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-6">
                    <label class="flex items-center space-x-3">
                        <input type="checkbox" class="form-checkbox h-5 w-5 text-amber-500 border-2 border-gray-600 rounded-md">
                        <span class="text-gray-300">Motivated Seller</span>
                    </label>
                    <label class="flex items-center space-x-3">
                        <input type="checkbox" class="form-checkbox h-5 w-5 text-amber-500 border-2 border-gray-600 rounded-md">
                        <span class="text-gray-300">Pre-Approved</span>
                    </label>
                    <label class="flex items-center space-x-3">
                        <input type="checkbox" class="form-checkbox h-5 w-5 text-amber-500 border-2 border-gray-600 rounded-md">
                        <span class="text-gray-300">Equity > 50%</span>
                    </label>
                    <label class="flex items-center space-x-3">
                        <input type="checkbox" class="form-checkbox h-5 w-5 text-amber-500 border-2 border-gray-600 rounded-md">
                        <span class="text-gray-300">Relocation</span>
                    </label>
                </div>
            </div>
        </div>
    </section>

    <!-- Results Grid -->
    <section class="container mx-auto px-4 mb-16">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8" id="resultsGrid">
            <!-- Lead cards will be dynamically inserted here -->
        </div>
    </section>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/noUiSlider/15.6.1/nouislider.min.js"></script>
    <script>
        // Sample Lead Data
        const leads = [
            {
                price: 2500000,
                type: 'Luxury Single Family',
                daysOnMarket: 45,
                location: 'Beverly Hills',
                sqft: 8500,
                motivated: true,
                preApproved: true,
                equity: 65,
                image: 'https://source.unsplash.com/random/800x600/?mansion'
            },
            // Add more lead objects here
        ];

        // Initialize Sliders
        const priceSlider = document.getElementById('priceRange');
        const daysSlider = document.getElementById('daysOnMarket');

        noUiSlider.create(priceSlider, {
            start: [500000, 5000000],
            connect: true,
            range: {
                'min': 500000,
                'max': 10000000
            },
            step: 250000
        });

        noUiSlider.create(daysSlider, {
            start: [0, 90],
            connect: true,
            range: {
                'min': 0,
                'max': 180
            },
            step: 15
        });

        // Filtering Logic
        function filterLeads() {
            // Get current filter values
            const priceValues = priceSlider.noUiSlider.get();
            const daysValues = daysSlider.noUiSlider.get();

            // Filter leads array
            const filtered = leads.filter(lead => {
                return lead.price >= priceValues[0] && 
                       lead.price <= priceValues[1] &&
                       lead.daysOnMarket >= daysValues[0] &&
                       lead.daysOnMarket <= daysValues[1];
            });

            updateResults(filtered);
        }

        // Update Display
        function updateResults(filteredLeads) {
            const grid = document.getElementById('resultsGrid');
            grid.innerHTML = '';

            filteredLeads.forEach(lead => {
                const card = document.createElement('div');
                card.className = 'bg-gray-800 rounded-2xl p-6 transform hover:scale-102 transition-all';
                card.innerHTML = `
                    <div class="relative mb-4">
                        <img src="${lead.image}" alt="Property" class="rounded-xl h-48 w-full object-cover">
                        <div class="absolute top-4 right-4 bg-amber-500 text-gray-900 px-3 py-1 rounded-full text-sm font-medium">
                            ${lead.equity}% Equity
                        </div>
                    </div>
                    <h3 class="text-xl font-bold text-gray-100 mb-2">$${lead.price.toLocaleString()}</h3>
                    <div class="flex justify-between text-gray-400 text-sm mb-4">
                        <span>${lead.sqft.toLocaleString()} sqft</span>
                        <span>${lead.daysOnMarket} Days</span>
                    </div>
                    <div class="flex flex-wrap gap-2">
                        ${lead.motivated ? '<span class="bg-emerald-500/20 text-emerald-400 px-3 py-1 rounded-full text-sm">Motivated</span>' : ''}
                        ${lead.preApproved ? '<span class="bg-blue-500/20 text-blue-400 px-3 py-1 rounded-full text-sm">Pre-Approved</span>' : ''}
                    </div>
                `;
                grid.appendChild(card);
            });

            document.getElementById('qualifiedCount').textContent = filteredLeads.length;
        }

        // Event Listeners
        priceSlider.noUiSlider.on('update', filterLeads);
        daysSlider.noUiSlider.on('update', filterLeads);
        document.querySelectorAll('input[type="checkbox"], select').forEach(el => {
            el.addEventListener('change', filterLeads);
        });

        // Initial load
        filterLeads();
    </script>
</body>
</html>
