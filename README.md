# mt-lf2
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lab Equipment Data Search</title>
    <!-- Tailwind CSS from CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #4a90e2;
            animation: spin 1s ease infinite;
        }
        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center">

    <div class="bg-white p-8 rounded-2xl shadow-xl w-full max-w-2xl mx-4">
        <div class="text-center mb-8">
            <h1 class="text-3xl font-bold text-gray-800 mb-2">Lab Equipment Search</h1>
            <p class="text-gray-600">Enter a keyword to find equipment details and their obsolete dates.</p>
        </div>

        <div class="relative mb-6">
            <input
                type="text"
                id="search-input"
                placeholder="Search by equipment name..."
                class="w-full px-5 py-3 pr-12 text-lg border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-200 ease-in-out shadow-sm"
            >
            <div id="loading-spinner" class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none hidden">
                <div class="spinner"></div>
            </div>
        </div>

        <div id="results-container" class="space-y-4">
            <!-- Results will be displayed here -->
            <div class="text-center text-gray-500 py-10">
                <p>Start typing to see results.</p>
            </div>
        </div>
    </div>

    <script>
        // Add your equipment data here
        const equipmentData = [
            
        ];

        const searchInput = document.getElementById('search-input');
        const resultsContainer = document.getElementById('results-container');
        const loadingSpinner = document.getElementById('loading-spinner');

        let timeout = null;

        const renderResults = (results) => {
            if (results.length === 0) {
                resultsContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-10">
                        <p>No equipment found matching your search.</p>
                    </div>
                `;
                return;
            }

            const html = results.map(item => {
                const obsoleteYear = item.obsoleteDate.substring(0, 4);
                const isObsolete = item.isObsolete;

                return `
                    <div class="bg-gray-50 p-6 rounded-xl border border-gray-200 shadow-sm flex flex-col sm:flex-row justify-between items-start sm:items-center">
                        <div>
                            <div class="text-lg font-semibold text-gray-800 mb-1">${item.name}</div>
                            <div class="text-sm text-gray-500">
                                Year Discontinued: <span class="font-medium text-gray-600">${obsoleteYear}</span>
                            </div>
                            <div class="text-sm text-gray-500">
                                Obsolete: <span class="font-medium ${isObsolete === 'YES' ? 'text-red-500' : 'text-green-500'}">${isObsolete}</span>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
            resultsContainer.innerHTML = html;
        };

        const performSearch = () => {
            const searchTerm = searchInput.value.toLowerCase().trim();

            if (searchTerm === '') {
                renderResults([]);
                resultsContainer.innerHTML = `
                    <div class="text-center text-gray-500 py-10">
                        <p>Start typing to see results.</p>
                    </div>
                `;
                return;
            }
            
            loadingSpinner.classList.remove('hidden');

            setTimeout(() => {
                const filteredResults = equipmentData.filter(item =>
                    item.name.toLowerCase().includes(searchTerm)
                );
                renderResults(filteredResults);
                loadingSpinner.classList.add('hidden');
            }, 500);
        };

        searchInput.addEventListener('input', () => {
            clearTimeout(timeout);
            timeout = setTimeout(performSearch, 300);
        });

        document.addEventListener('DOMContentLoaded', () => {
            renderResults(equipmentData);
        });
    </script>

</body>
</html>
