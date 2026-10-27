<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>Student Internship Board</title>  
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>  
</head>  
<body class="bg-slate-50 text-slate-900 min-h-screen flex flex-col">  
  
    <!-- Header -->  
    <header class="bg-indigo-600 text-white shadow-md py-6 px-4">  
        <div class="max-w-5xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4">  
            <h1 class="text-2xl font-bold">Student Internship Board</h1>  
            <!-- Search & Filters -->  
            <div class="flex flex-wrap gap-2 w-full md:w-auto">  
                <input id="searchInput" type="text" placeholder="Search internships..."   
                    class="px-4 py-2 rounded-lg text-slate-800 bg-white focus:outline-none focus:ring-2 focus:ring-indigo-400 w-full md:w-64"  
                    aria-label="Search internships">  
                <select id="domainFilter" class="px-4 py-2 rounded-lg text-slate-800 bg-white focus:outline-none focus:ring-2 focus:ring-indigo-400">  
                    <option value="">All Domains</option>  
                    <option value="Frontend">Frontend</option>  
                    <option value="Backend">Backend</option>  
                    <option value="AI/ML">AI/ML</option>  
                    <option value="UI/UX">UI/UX</option>  
                </select>  
                <select id="modeFilter" class="px-4 py-2 rounded-lg text-slate-800 bg-white focus:outline-none focus:ring-2 focus:ring-indigo-400">  
                    <option value="">All Work Modes</option>  
                    <option value="Remote">Remote</option>  
                    <option value="Hybrid">Hybrid</option>  
                    <option value="On-site">On-site</option>  
                </select>  
            </div>  
        </div>  
    </header>  
  
    <!-- Main Content -->  
    <main class="max-w-5xl mx-auto px-4 py-8 flex-grow w-full">  
        <!-- Loading State -->  
        <div id="loadingState" class="text-center py-12 text-slate-500 hidden">Loading internships...</div>  
          
        <!-- Error State -->  
        <div id="errorState" class="text-center py-12 text-red-500 hidden">  
            Failed to load data. <button onclick="loadData()" class="underline font-semibold ml-2">Retry</button>  
        </div>  
  
        <!-- Empty State -->  
        <div id="emptyState" class="text-center py-12 text-slate-500 hidden">  
            No internships found matching your criteria.  
        </div>  
  
        <!-- Internship Grid -->  
        <div id="internshipGrid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>  
    </main>  
  
    <!-- Footer -->  
    <footer class="bg-white border-t border-slate-200 text-center py-4 text-sm text-slate-500">  
        Edvyro Internship Assessment Solution &copy; 2026  
    </footer>  
  
    <!-- JavaScript Logic -->  
    <script>  
        const seedData = [  
            { id: "INT-001", title: "Frontend Developer Intern", company: "TechCorp", domain: "Frontend", mode: "Remote", stipend: "$500/mo", description: "Build accessible UIs using modern web standards." },  
            { id: "INT-002", title: "Backend Node.js Intern", company: "DataFlow", domain: "Backend", mode: "Hybrid", stipend: "$600/mo", description: "Design scalable APIs and handle secure database connections." },  
            { id: "INT-003", title: "AI/ML Research Intern", company: "NeuralSys", domain: "AI/ML", mode: "Remote", stipend: "$700/mo", description: "Assist in training and fine-tuning lightweight synthetic models." },  
            { id: "INT-004", title: "UI/UX Design Intern", company: "CreativeStudio", domain: "UI/UX", mode: "On-site", stipend: "$450/mo", description: "Create wireframes, user personas, and accessible prototypes." }  
        ];  
  
        const searchInput = document.getElementById('searchInput');  
        const domainFilter = document.getElementById('domainFilter');  
        const modeFilter = document.getElementById('modeFilter');  
        const internshipGrid = document.getElementById('internshipGrid');  
        const loadingState = document.getElementById('loadingState');  
        const errorState = document.getElementById('errorState');  
        const emptyState = document.getElementById('emptyState');  
  
        function renderData(data) {  
            internshipGrid.innerHTML = '';  
            if (data.length === 0) {  
                emptyState.classList.remove('hidden');  
                return;  
            }  
            emptyState.classList.add('hidden');  
  
            data.forEach(item => {  
                const card = document.createElement('div');  
                card.className = "bg-white p-6 rounded-xl shadow-sm border border-slate-200 flex flex-col justify-between hover:shadow-md transition";  
                card.innerHTML = `  
                    <div>  
                        <div class="flex justify-between items-start mb-2">  
                            <span class="text-xs font-semibold px-2.5 py-1 bg-indigo-50 text-indigo-700 rounded-full">${item.domain}</span>  
                            <span class="text-xs font-medium text-slate-500">${item.mode}</span>  
                        </div>  
                        <h3 class="text-lg font-bold text-slate-800">${item.title}</h3>  
                        <p class="text-sm text-indigo-600 font-medium mb-3">${item.company}</p>  
                        <p class="text-sm text-slate-600 mb-4">${item.description}</p>  
                    </div>  
                    <div class="flex justify-between items-center pt-4 border-t border-slate-100">  
                        <span class="text-sm font-bold text-slate-700">${item.stipend}</span>  
                        <button onclick="alert('Application submitted successfully for ${item.title}!')"   
                            class="px-4 py-2 bg-indigo-600 text-white text-sm font-medium rounded-lg hover:bg-indigo-700 focus:ring-2 focus:ring-indigo-400 focus:outline-none">  
                            Apply Now  
                        </button>  
                    </div>  
                `;  
                internshipGrid.appendChild(card);  
            });  
        }  
  
        function filterAndRender() {  
            const query = searchInput.value.toLowerCase();  
            const domain = domainFilter.value;  
            const mode = modeFilter.value;  
  
            const filtered = seedData.filter(item => {  
                const matchesQuery = item.title.toLowerCase().includes(query) || item.company.toLowerCase().includes(query);  
                const matchesDomain = domain === "" || item.domain === domain;  
                const matchesMode = mode === "" || item.mode === mode;  
                return matchesQuery && matchesDomain && matchesMode;  
            });  
  
            renderData(filtered);  
        }  
  
        function loadData() {  
            loadingState.classList.remove('hidden');  
            errorState.classList.add('hidden');  
            internshipGrid.innerHTML = '';  
  
            setTimeout(() => {  
                loadingState.classList.add('hidden');  
                renderData(seedData);  
            }, 400);  
        }  
  
        searchInput.addEventListener('input', filterAndRender);  
        domainFilter.addEventListener('change', filterAndRender);  
        modeFilter.addEventListener('change', filterAndRender);  
  
        loadData();  
    </script>  
</body>  
</html>  
