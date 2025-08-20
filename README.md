<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cocinando con El Tipo</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Font Awesome for social icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #fcf8f3; /* Un color cálido y cercano */
        }
        /* Estilos personalizados para la barra lateral */
        .sidebar-item-active {
            @apply bg-amber-200 text-amber-800 font-semibold;
        }
        .main-nav-button {
            @apply py-2 px-4 rounded-full transition-all duration-300 ease-in-out hover:bg-amber-200 hover:text-amber-800 focus:outline-none focus:ring-2 focus:ring-amber-500 focus:ring-opacity-50;
        }
        .recipe-card {
            @apply bg-white rounded-xl shadow-lg overflow-hidden transition-transform duration-300 ease-in-out transform hover:scale-105 cursor-pointer;
        }
        .text-overlay {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background-image: linear-gradient(to top, rgba(0,0,0,0.7), rgba(0,0,0,0));
            padding: 1rem;
            color: white;
            opacity: 1;
            transition: opacity 0.3s ease-in-out;
        }
        .recipe-card:hover .text-overlay {
            opacity: 1; /* Ensure it stays visible on hover */
        }
        /* Custom modal styles */
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 1000; /* Sit on top */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            max-width: 90%;
            max-height: 90%;
            overflow-y: auto;
            position: relative;
        }
        .close-button {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            position: absolute;
            top: 10px;
            right: 20px;
        }
        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen">
    <!-- Navbar (Top Navigation) -->
    <header class="bg-white shadow-md p-4 sticky top-0 z-50 rounded-b-xl mx-2 mt-2">
        <div class="container mx-auto flex justify-between items-center flex-wrap">
            <h1 class="text-2xl font-bold text-orange-600">Cocinando con <span class="text-amber-500">[tu nombre]</span></h1>
            <nav class="hidden md:flex space-x-4">
                <button id="nav-inicio" class="main-nav-button text-orange-700 font-semibold">Inicio</button>
                <button id="nav-recetas" class="main-nav-button text-orange-700 font-semibold">Recetas</button>
                <button id="nav-culturas" class="main-nav-button text-orange-700 font-semibold">Culturas</button>
                <button id="nav-foros" class="main-nav-button text-orange-700 font-semibold">Foros</button>
            </nav>
            <!-- Hamburger Menu for Mobile -->
            <button id="mobile-menu-button" class="md:hidden text-orange-700 text-2xl focus:outline-none">
                &#9776;
            </button>
        </div>
        <!-- Mobile Navigation Menu -->
        <div id="mobile-menu" class="hidden md:hidden mt-4 space-y-2">
            <button id="mobile-nav-inicio" class="block w-full text-left py-2 px-4 rounded-lg bg-orange-100 text-orange-700 font-semibold">Inicio</button>
            <button id="mobile-nav-recetas" class="block w-full text-left py-2 px-4 rounded-lg bg-orange-100 text-orange-700 font-semibold">Recetas</button>
            <button id="mobile-nav-culturas" class="block w-full text-left py-2 px-4 rounded-lg bg-orange-100 text-orange-700 font-semibold">Culturas</button>
            <button id="mobile-nav-foros" class="block w-full text-left py-2 px-4 rounded-lg bg-orange-100 text-orange-700 font-semibold">Foros</button>
        </div>
    </header>

    <!-- Main Content Area -->
    <div class="flex flex-1 flex-col md:flex-row mt-4 px-2">
        <!-- Sidebar (Left Menu) -->
        <aside class="md:w-64 bg-white p-4 rounded-xl shadow-lg md:mr-4 mb-4 md:mb-0">
            <h2 class="text-xl font-semibold text-orange-700 mb-4">Explora por País</h2>
            <ul id="country-list" class="space-y-2">
                <!-- Country list will be populated here -->
            </ul>
        </aside>

        <!-- Main Content Display -->
        <main id="main-content-display" class="flex-1 bg-white p-6 rounded-xl shadow-lg min-h-[60vh]">
            <!-- Content will be dynamically loaded here -->
        </main>
    </div>

    <!-- Modals for messages (e.g., login, report) -->
    <div id="message-modal" class="modal">
        <div class="modal-content text-center">
            <span class="close-button" onclick="closeModal('message-modal')">&times;</span>
            <p id="modal-message" class="text-lg font-medium text-gray-800"></p>
            <button onclick="closeModal('message-modal')" class="mt-4 px-6 py-2 bg-amber-500 text-white rounded-lg hover:bg-amber-600 transition-colors duration-200">Aceptar</button>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-orange-700 text-white p-6 mt-8 rounded-t-xl mx-2">
        <div class="container mx-auto flex flex-col md:flex-row justify-between items-center">
            <p>&copy; 2025 Cocinando con [tu nombre]. Todos los derechos reservados.</p>
            <div class="flex space-x-4 mt-4 md:mt-0">
                <a href="#" class="text-white hover:text-amber-300 text-2xl transition-colors duration-200" aria-label="Instagram"><i class="fab fa-instagram"></i></a>
                <a href="#" class="text-white hover:text-amber-300 text-2xl transition-colors duration-200" aria-label="YouTube"><i class="fab fa-youtube"></i></a>
                <a href="#" class="text-white hover:text-amber-300 text-2xl transition-colors duration-200" aria-label="Facebook"><i class="fab fa-facebook"></i></a>
                <a href="#" class="text-white hover:text-amber-300 text-2xl transition-colors duration-200" aria-label="TikTok"><i class="fab fa-tiktok"></i></a>
            </div>
        </div>
    </footer>

    <script>
        // --- Mock Data ---
        const appData = {
            countries: [
                "República Dominicana", "Italia", "Venezuela", "India", "China", "Australia"
            ],
            recipes: [
                // República Dominicana
                {
                    id: 'sancocho', name: 'Sancocho', country: 'República Dominicana',
                    description: 'Un guiso sustancioso con siete tipos de carne y vegetales, un clásico dominicano.',
                    image: 'https://placehold.co/400x300/F97316/FFFFFF?text=Sancocho',
                    ingredients: ['7 tipos de carne', 'Yuca', 'Plátano', 'Ñame', 'Auyama', 'Cilantro', 'Ajo', 'Cebolla', 'Ají cubanela'],
                    preparationSteps: ['Paso 1: Sellar las carnes.', 'Paso 2: Añadir vegetales y cocinar.', 'Paso 3: Servir caliente.'],
                    nutritionalInfo: 'Calorías: 600, Proteínas: 40g, Carbohidratos: 50g'
                },
                {
                    id: 'manguyqueso', name: 'Mangú con Los Tres Golpes', country: 'República Dominicana',
                    description: 'Plátanos verdes majados con cebolla, queso frito, salami y huevos fritos.',
                    image: 'https://placehold.co/400x300/F97316/FFFFFF?text=Mangu',
                    ingredients: ['Plátanos verdes', 'Cebolla roja', 'Aceite', 'Vinagre', 'Queso frito', 'Salami', 'Huevos'],
                    preparationSteps: ['Paso 1: Hervir y majar plátanos.', 'Paso 2: Freír los "golpes".', 'Paso 3: Unir y servir.'],
                    nutritionalInfo: 'Calorías: 750, Proteínas: 30g, Carbohidratos: 60g'
                },
                // Italia
                {
                    id: 'lasana', name: 'Lasaña Clásica', country: 'Italia',
                    description: 'Capas de pasta, ragú de carne, bechamel y queso, horneado a la perfección.',
                    image: 'https://placehold.co/400x300/DC2626/FFFFFF?text=Lasana',
                    ingredients: ['Pasta para lasaña', 'Carne molida', 'Tomate triturado', 'Cebolla', 'Bechamel', 'Queso mozzarella', 'Parmesano'],
                    preparationSteps: ['Paso 1: Preparar el ragú.', 'Paso 2: Montar las capas.', 'Paso 3: Hornear hasta dorar.'],
                    nutritionalInfo: 'Calorías: 550, Proteínas: 35g, Carbohidratos: 45g'
                },
                {
                    id: 'pizza-margherita', name: 'Pizza Margherita', country: 'Italia',
                    description: 'La esencia de Italia: salsa de tomate, mozzarella fresca y albahaca.',
                    image: 'https://placehold.co/400x300/DC2626/FFFFFF?text=Pizza',
                    ingredients: ['Harina', 'Levadura', 'Agua', 'Tomate San Marzano', 'Mozzarella di Bufala', 'Albahaca fresca'],
                    preparationSteps: ['Paso 1: Preparar la masa.', 'Paso 2: Añadir ingredientes.', 'Paso 3: Hornear en horno muy caliente.'],
                    nutritionalInfo: 'Calorías: 400, Proteínas: 15g, Carbohidratos: 50g'
                },
                // Venezuela
                {
                    id: 'pabellon-criollo', name: 'Pabellón Criollo', country: 'Venezuela',
                    description: 'El plato nacional de Venezuela: carne mechada, arroz blanco, caraotas negras y tajadas.',
                    image: 'https://placehold.co/400x300/FBBF24/FFFFFF?text=Pabellon',
                    ingredients: ['Carne mechada', 'Arroz blanco', 'Caraotas negras', 'Plátano maduro (tajadas)'],
                    preparationSteps: ['Paso 1: Cocinar cada componente por separado.', 'Paso 2: Ensamblar el plato.', 'Paso 3: Servir.'],
                    nutritionalInfo: 'Calorías: 700, Proteínas: 45g, Carbohidratos: 70g'
                },
                {
                    id: 'arepas', name: 'Arepas Rellenas', country: 'Venezuela',
                    description: 'Discos de masa de maíz asados o fritos, rellenos con una variedad de ingredientes.',
                    image: 'https://placehold.co/400x300/FBBF24/FFFFFF?text=Arepas',
                    ingredients: ['Harina de maíz precocida', 'Agua', 'Sal', 'Rellenos (queso, pernil, reina pepiada, etc.)'],
                    preparationSteps: ['Paso 1: Preparar la masa.', 'Paso 2: Formar y cocinar las arepas.', 'Paso 3: Rellenar al gusto.'],
                    nutritionalInfo: 'Varía según el relleno.'
                },
                // India
                {
                    id: 'curry-pollo', name: 'Curry de Pollo Tikka Masala', country: 'India',
                    description: 'Tiernos trozos de pollo marinados en yogur y especias, cocinados en una rica salsa cremosa.',
                    image: 'https://placehold.co/400x300/10B981/FFFFFF?text=Curry',
                    ingredients: ['Pollo', 'Yogur', 'Jengibre', 'Ajo', 'Especias (comino, cilantro, cúrcuma, garam masala)', 'Tomate', 'Crema'],
                    preparationSteps: ['Paso 1: Marinar y asar el pollo.', 'Paso 2: Preparar la salsa.', 'Paso 3: Combinar y cocinar a fuego lento.'],
                    nutritionalInfo: 'Calorías: 500, Proteínas: 30g, Grasas: 30g'
                },
                {
                    id: 'biryani', name: 'Biryani de Cordero', country: 'India',
                    description: 'Un aromático plato de arroz basmati cocinado con tiernos trozos de cordero y especias exóticas.',
                    image: 'https://placehold.co/400x300/10B981/FFFFFF?text=Biryani',
                    ingredients: ['Arroz basmati', 'Cordero', 'Cebolla', 'Jengibre', 'Ajo', 'Especias enteras y molidas', 'Yogur', 'Hierbas frescas'],
                    preparationSteps: ['Paso 1: Marinar el cordero.', 'Paso 2: Cocinar el arroz.', 'Paso 3: Cocinar en capas lentamente.'],
                    nutritionalInfo: 'Calorías: 650, Proteínas: 40g, Carbohidratos: 60g'
                },
                // China
                {
                    id: 'dumplings', name: 'Jiaozi (Dumplings)', country: 'China',
                    description: 'Deliciosas empanadillas de masa fina rellenas de carne y vegetales, cocidas al vapor o fritas.',
                    image: 'https://placehold.co/400x300/3B82F6/FFFFFF?text=Dumplings',
                    ingredients: ['Harina', 'Agua', 'Carne de cerdo molida', 'Repollo', 'Cebollino', 'Jengibre', 'Salsa de soja', 'Aceite de sésamo'],
                    preparationSteps: ['Paso 1: Preparar el relleno.', 'Paso 2: Estirar la masa y formar los dumplings.', 'Paso 3: Cocinar al vapor o freír.'],
                    nutritionalInfo: 'Calorías: 350, Proteínas: 15g, Carbohidratos: 40g'
                },
                {
                    id: 'arroz-frito', name: 'Arroz Frito Yangshuo', country: 'China',
                    description: 'Un clásico arroz frito con camarones, cerdo asado, huevo y vegetales.',
                    image: 'https://placehold.co/400x300/3B82F6/FFFFFF?text=Arroz+Frito',
                    ingredients: ['Arroz cocido frío', 'Camarones', 'Cerdo asado (Char Siu)', 'Huevo', 'Zanahoria', 'Guisantes', 'Salsa de soja', 'Aceite de sésamo'],
                    preparationSteps: ['Paso 1: Saltear los ingredientes en un wok.', 'Paso 2: Añadir el arroz y la salsa.', 'Paso 3: Cocinar hasta que esté caliente y bien mezclado.'],
                    nutritionalInfo: 'Calorías: 480, Proteínas: 25g, Carbohidratos: 50g'
                },
                // Australia
                {
                    id: 'lamington', name: 'Lamington', country: 'Australia',
                    description: 'Cuadrados de bizcocho cubiertos de chocolate y coco rallado, un dulce icónico australiano.',
                    image: 'https://placehold.co/400x300/059669/FFFFFF?text=Lamington',
                    ingredients: ['Harina', 'Azúcar', 'Huevos', 'Leche', 'Mantequilla', 'Cacao en polvo', 'Coco rallado'],
                    preparationSteps: ['Paso 1: Hornear el bizcocho.', 'Paso 2: Preparar el glaseado de chocolate.', 'Paso 3: Cortar, bañar en chocolate y rebozar en coco.'],
                    nutritionalInfo: 'Calorías: 280, Carbohidratos: 35g, Grasas: 15g'
                },
                {
                    id: 'meat-pie', name: 'Australian Meat Pie', country: 'Australia',
                    description: 'Un pastel de carne tradicional australiano, con carne picada y salsa en masa de hojaldre.',
                    image: 'https://placehold.co/400x300/059669/FFFFFF?text=Meat+Pie',
                    ingredients: ['Masa de hojaldre', 'Carne de res molida', 'Cebolla', 'Salsa de tomate', 'Caldo de carne', 'Especias'],
                    preparationSteps: ['Paso 1: Cocinar el relleno de carne.', 'Paso 2: Forrar moldes con masa.', 'Paso 3: Rellenar y cubrir con masa, luego hornear.'],
                    nutritionalInfo: 'Calorías: 450, Proteínas: 20g, Grasas: 25g'
                }
            ],
            blogArticles: [
                {
                    id: 'historia-sushi', title: 'La Fascinante Historia del Sushi',
                    content: 'El sushi, un plato emblemático de Japón, tiene sus orígenes en el sudeste asiático como un método para conservar el pescado en arroz fermentado. Llegó a Japón alrededor del siglo VIII y evolucionó a lo largo de los siglos, transformándose de un método de conservación a una forma de arte culinario. No fue hasta el siglo XIX que el sushi se convirtió en el plato que conocemos hoy, con la introducción del sushi nigiri en Edo (actual Tokio). Descubre cómo este plato ha conquistado el mundo, manteniendo sus raíces tradicionales mientras se adapta a los gustos modernos.',
                    image: 'https://placehold.co/600x400/808080/FFFFFF?text=Historia+Sushi',
                    relatedLinks: [
                        { text: 'Tipos de Sushi', url: '#' },
                        { text: 'Receta de Arroz para Sushi', url: '#' }
                    ]
                },
                {
                    id: 'especias-india', title: 'Las Especias: Alma de la Cocina India',
                    content: 'La cocina india es famosa por su uso audaz y aromático de las especias. Desde el vibrante color de la cúrcuma hasta el calor del chile y la dulzura del cardamomo, cada especia juega un papel crucial en la creación de sabores complejos y equilibrados. Las especias no solo añaden sabor, sino que muchas tienen propiedades medicinales. Exploramos las especias esenciales de la despensa india y cómo se combinan para crear platos inolvidables como curries, biryanis y masalas. Aprende sobre el arte de "temperar" las especias para liberar todo su aroma.',
                    image: 'https://placehold.co/600x400/808080/FFFFFF?text=Especias+India',
                    relatedLinks: [
                        { text: 'Garam Masala casero', url: '#' },
                        { text: 'Beneficios de la Cúrcuma', url: '#' }
                    ]
                }
            ],
            forumTopics: [
                { id: 'consejos', title: 'Consejos de cocina', description: 'Comparte tus mejores trucos y consejos culinarios.' },
                { id: 'rapidas', title: 'Recetas rápidas', description: 'Ideas para comidas deliciosas en poco tiempo.' },
                { id: 'errores', title: 'Errores comunes', description: 'Discute y aprende de los errores más frecuentes en la cocina.' }
            ]
        };

        // --- Global State and Utility Functions ---
        let currentView = 'inicio';
        const mainContentDisplay = document.getElementById('main-content-display');
        const countryList = document.getElementById('country-list');
        const mobileMenuButton = document.getElementById('mobile-menu-button');
        const mobileMenu = document.getElementById('mobile-menu');

        // Function to show custom modal messages
        function showModal(message) {
            document.getElementById('modal-message').innerText = message;
            document.getElementById('message-modal').style.display = 'flex'; // Use flex to center
        }

        // Function to close custom modal
        function closeModal(modalId) {
            document.getElementById(modalId).style.display = 'none';
        }

        // --- Navigation Handlers ---
        function setView(view, data = null) {
            currentView = view;
            mainContentDisplay.innerHTML = ''; // Clear previous content
            window.scrollTo({ top: 0, behavior: 'smooth' }); // Scroll to top on view change

            // Update active state for main navigation buttons
            document.querySelectorAll('.main-nav-button, .mobile-nav-button').forEach(btn => {
                btn.classList.remove('bg-amber-200', 'text-amber-800');
                btn.classList.add('text-orange-700');
            });
            const activeBtnId = `nav-${view}`;
            const mobileActiveBtnId = `mobile-nav-${view}`;
            document.getElementById(activeBtnId)?.classList.add('bg-amber-200', 'text-amber-800');
            document.getElementById(mobileActiveBtnId)?.classList.add('bg-amber-200', 'text-amber-800');

            renderContent(view, data);
        }

        function renderContent(view, data) {
            switch (view) {
                case 'inicio':
                    renderInicioPage();
                    break;
                case 'recetas':
                    renderRecetasPage();
                    break;
                case 'culturas':
                    renderCulturasPage();
                    break;
                case 'foros':
                    renderForosPage();
                    break;
                case 'recipe-detail':
                    renderRecipeDetailPage(data); // data will be the recipe ID
                    break;
                case 'blog-article':
                    renderBlogArticlePage(data); // data will be the article ID
                    break;
                default:
                    renderInicioPage();
            }
        }

        // --- Render Functions for each section ---

        // 1. Página principal (Inicio)
        function renderInicioPage() {
            let featuredRecipes = [];
            // Select 6 random recipes to be "most consumed" dynamically
            const shuffledRecipes = [...appData.recipes].sort(() => 0.5 - Math.random());
            featuredRecipes = shuffledRecipes.slice(0, Math.min(6, shuffledRecipes.length)); // Ensure we don't try to get more recipes than available

            mainContentDisplay.innerHTML = `
                <h2 class="text-3xl font-bold text-orange-700 mb-6 border-b-2 border-amber-300 pb-2">Destacados del Año</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    ${featuredRecipes.map(recipe => `
                        <div class="recipe-card" data-recipe-id="${recipe.id}">
                            <div class="relative pb-2/3 overflow-hidden rounded-t-xl">
                                <img src="${recipe.image}" alt="${recipe.name}" class="absolute h-full w-full object-cover">
                                <div class="text-overlay">
                                    <h3 class="text-xl font-semibold">${recipe.name}</h3>
                                    <p class="text-sm line-clamp-2">${recipe.description}</p>
                                </div>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
            addRecipeCardListeners();
        }

        // 2. Secciones Principales
        function renderRecetasPage() {
            mainContentDisplay.innerHTML = `
                <h2 class="text-3xl font-bold text-orange-700 mb-6 border-b-2 border-amber-300 pb-2">Todas las Recetas</h2>
                <div class="mb-6 flex flex-col md:flex-row gap-4">
                    <input type="text" id="recipe-search" placeholder="Buscar por nombre o ingrediente..." class="flex-1 p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500">
                    <select id="filter-country" class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500">
                        <option value="">Filtrar por País</option>
                        ${appData.countries.map(country => `<option value="${country}">${country}</option>`).join('')}
                    </select>
                    <select id="filter-difficulty" class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-amber-500">
                        <option value="">Nivel de Dificultad</option>
                        <option value="Fácil">Fácil</option>
                        <option value="Medio">Medio</option>
                        <option value="Difícil">Difícil</option>
                    </select>
                </div>
                <div id="recipe-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Recipes will be filtered and displayed here -->
                </div>
            `;
            const recipeListDiv = document.getElementById('recipe-list');
            const recipeSearchInput = document.getElementById('recipe-search');
            const filterCountrySelect = document.getElementById('filter-country');
            const filterDifficultySelect = document.getElementById('filter-difficulty'); // Assuming difficulty could be added to mock data

            // Initial render of all recipes
            renderFilteredRecipes();

            // Add event listeners for search and filters
            recipeSearchInput.addEventListener('input', renderFilteredRecipes);
            filterCountrySelect.addEventListener('change', renderFilteredRecipes);
            filterDifficultySelect.addEventListener('change', renderFilteredRecipes);

            function renderFilteredRecipes() {
                const searchTerm = recipeSearchInput.value.toLowerCase();
                const selectedCountry = filterCountrySelect.value;
                const selectedDifficulty = filterDifficultySelect.value; // Not implemented in mock data, but structure is here

                const filtered = appData.recipes.filter(recipe => {
                    const matchesSearch = recipe.name.toLowerCase().includes(searchTerm) ||
                                          recipe.ingredients.some(ing => ing.toLowerCase().includes(searchTerm));
                    const matchesCountry = selectedCountry === '' || recipe.country === selectedCountry;
                    // const matchesDifficulty = selectedDifficulty === '' || (recipe.difficulty && recipe.difficulty === selectedDifficulty);

                    return matchesSearch && matchesCountry; // && matchesDifficulty;
                });

                recipeListDiv.innerHTML = filtered.map(recipe => `
                    <div class="recipe-card" data-recipe-id="${recipe.id}">
                        <div class="relative pb-2/3 overflow-hidden rounded-t-xl">
                            <img src="${recipe.image}" alt="${recipe.name}" class="absolute h-full w-full object-cover">
                            <div class="text-overlay">
                                <h3 class="text-xl font-semibold">${recipe.name}</h3>
                                <p class="text-sm line-clamp-2">${recipe.description}</p>
                            </div>
                        </div>
                    </div>
                `).join('');
                addRecipeCardListeners();
            }
        }

        function renderCulturasPage() {
            mainContentDisplay.innerHTML = `
                <h2 class="text-3xl font-bold text-orange-700 mb-6 border-b-2 border-amber-300 pb-2">Culturas Culinarias</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    ${appData.blogArticles.map(article => `
                        <div class="bg-white rounded-xl shadow-lg overflow-hidden flex flex-col cursor-pointer hover:shadow-xl transition-shadow duration-300" data-article-id="${article.id}">
                            <img src="${article.image}" alt="${article.title}" class="w-full h-48 object-cover">
                            <div class="p-5 flex-1 flex flex-col justify-between">
                                <h3 class="text-xl font-semibold text-orange-700 mb-2">${article.title}</h3>
                                <p class="text-gray-600 text-sm line-clamp-3 mb-4">${article.content}</p>
                                <button class="mt-auto self-end text-amber-600 hover:underline">Leer más</button>
                            </div>
                        </div>
                    `).join('')}
                </div>
            `;
            document.querySelectorAll('#main-content-display [data-article-id]').forEach(card => {
                card.addEventListener('click', (event) => {
                    const articleId = event.currentTarget.dataset.articleId;
                    setView('blog-article', articleId);
                });
            });
        }

        function renderBlogArticlePage(articleId) {
            const article = appData.blogArticles.find(a => a.id === articleId);
            if (!article) {
                mainContentDisplay.innerHTML = `<p class="text-red-600">Artículo no encontrado.</p>`;
                return;
            }

            mainContentDisplay.innerHTML = `
                <nav class="text-sm text-gray-500 mb-4">
                    <a href="#" onclick="setView('culturas')" class="hover:underline text-amber-600">Culturas</a> > ${article.title}
                </nav>
                <h2 class="text-3xl font-bold text-orange-700 mb-4">${article.title}</h2>
                <img src="${article.image}" alt="${article.title}" class="w-full h-72 object-cover rounded-lg mb-6 shadow-md">
                <p class="text-gray-700 leading-relaxed mb-6">${article.content}</p>
                ${article.relatedLinks.length > 0 ? `
                    <h3 class="text-xl font-semibold text-orange-700 mb-3">Enlaces Relacionados:</h3>
                    <ul class="list-disc list-inside text-amber-700">
                        ${article.relatedLinks.map(link => `
                            <li><a href="${link.url}" class="hover:underline">${link.text}</a></li>
                        `).join('')}
                    </ul>
                ` : ''}
                <button onclick="setView('culturas')" class="mt-8 px-6 py-3 bg-amber-500 text-white rounded-lg hover:bg-amber-600 transition-colors duration-200">Volver a Culturas</button>
            `;
        }


        function renderForosPage() {
            mainContentDisplay.innerHTML = `
                <h2 class="text-3xl font-bold text-orange-700 mb-6 border-b-2 border-amber-300 pb-2">Foros de Discusión</h2>
                <p class="text-gray-700 mb-6">¡Bienvenido a nuestros foros! Un espacio para conectar y compartir con otros amantes de la cocina.</p>

                <div class="mb-8 p-6 bg-amber-100 rounded-lg shadow-inner">
                    <h3 class="text-2xl font-semibold text-orange-700 mb-4">Únete a la Conversación</h3>
                    <div class="flex flex-col md:flex-row gap-4 mb-4">
                        <input type="text" placeholder="Usuario" class="p-3 border border-gray-300 rounded-lg flex-1">
                        <input type="password" placeholder="Contraseña" class="p-3 border border-gray-300 rounded-lg flex-1">
                    </div>
                    <div class="flex flex-col md:flex-row gap-4">
                        <button onclick="showModal('Funcionalidad de login aún no implementada.')" class="flex-1 py-3 bg-orange-500 text-white rounded-lg hover:bg-orange-600 transition-colors duration-200 font-semibold">Iniciar Sesión</button>
                        <button onclick="showModal('Funcionalidad de registro aún no implementada.')" class="flex-1 py-3 bg-amber-600 text-white rounded-lg hover:bg-amber-700 transition-colors duration-200 font-semibold">Registrarse</button>
                    </div>
                </div>

                <h3 class="text-2xl font-semibold text-orange-700 mb-4">Temas Populares</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    ${appData.forumTopics.map(topic => `
                        <div class="bg-white p-5 rounded-lg shadow-md border border-gray-200 hover:shadow-lg transition-shadow duration-200">
                            <h4 class="text-xl font-semibold text-orange-600 mb-2">${topic.title}</h4>
                            <p class="text-gray-600">${topic.description}</p>
                            <button onclick="showModal('Esta es una vista previa. Para participar, inicie sesión o regístrese.')" class="mt-4 text-sm text-amber-600 hover:underline">Ver Discusión</button>
                        </div>
                    `).join('')}
                </div>

                <div class="mt-8 p-6 bg-gray-100 rounded-lg shadow-inner">
                    <h3 class="text-2xl font-semibold text-orange-700 mb-4">Administración del Foro (Mock)</h3>
                    <p class="text-gray-700 mb-4">
                        Este apartado simula funcionalidades de moderación. En una aplicación real,
                        estas acciones requerirían autenticación de moderadores.
                    </p>
                    <div class="flex flex-col md:flex-row gap-4">
                        <button onclick="showModal('Comentario reportado. Se enviará a moderación.')" class="flex-1 py-3 bg-red-500 text-white rounded-lg hover:bg-red-600 transition-colors duration-200 font-semibold">Reportar Comentario</button>
                        <button onclick="showModal('Usuario bloqueado. (Mock)')" class="flex-1 py-3 bg-gray-500 text-white rounded-lg hover:bg-gray-600 transition-colors duration-200 font-semibold">Bloquear Usuario</button>
                    </div>
                </div>
            `;
        }


        // 3. Barra Lateral (Menú Desplegable de Países)
        function populateSidebar() {
            countryList.innerHTML = appData.countries.map(country => `
                <li class="cursor-pointer">
                    <div class="flex justify-between items-center py-2 px-3 rounded-lg hover:bg-amber-100 transition-colors duration-200 country-toggle" data-country="${country}">
                        <span class="text-gray-800 font-medium">${country}</span>
                        <i class="fas fa-chevron-down text-gray-500 text-sm transform transition-transform duration-300"></i>
                    </div>
                    <ul class="country-recipes hidden ml-4 mt-1 space-y-1">
                        ${appData.recipes
                            .filter(recipe => recipe.country === country)
                            .map(recipe => `<li class="py-1 px-2 text-sm text-gray-700 hover:bg-amber-50 rounded-md recipe-link" data-recipe-id="${recipe.id}">${recipe.name}</li>`)
                            .join('')}
                    </ul>
                </li>
            `).join('');

            // Add event listeners for country toggles
            document.querySelectorAll('.country-toggle').forEach(item => {
                item.addEventListener('click', (event) => {
                    const countryItem = event.currentTarget.closest('li');
                    const recipeList = countryItem.querySelector('.country-recipes');
                    const icon = event.currentTarget.querySelector('i');

                    recipeList.classList.toggle('hidden');
                    icon.classList.toggle('rotate-180'); // Rotate arrow icon
                });
            });

            // Add event listeners for recipe links in sidebar
            document.querySelectorAll('.recipe-link').forEach(item => {
                item.addEventListener('click', (event) => {
                    event.stopPropagation(); // Prevent country toggle from closing immediately
                    const recipeId = event.currentTarget.dataset.recipeId;
                    setView('recipe-detail', recipeId);

                    // De-highlight all sidebar recipes
                    document.querySelectorAll('.recipe-link').forEach(link => {
                        link.classList.remove('sidebar-item-active');
                    });
                    // Highlight the clicked recipe
                    event.currentTarget.classList.add('sidebar-item-active');
                });
            });
        }

        function renderRecipeDetailPage(recipeId) {
            const recipe = appData.recipes.find(r => r.id === recipeId);
            if (!recipe) {
                mainContentDisplay.innerHTML = `<p class="text-red-600">Receta no encontrada.</p>`;
                return;
            }

            mainContentDisplay.innerHTML = `
                <nav class="text-sm text-gray-500 mb-4">
                    <a href="#" onclick="setView('inicio')" class="hover:underline text-amber-600">Inicio</a> >
                    <span class="text-gray-800">${recipe.country}</span> >
                    ${recipe.name}
                </nav>
                <h2 class="text-4xl font-bold text-orange-700 mb-4">${recipe.name}</h2>
                <p class="text-xl text-gray-600 mb-6">${recipe.description}</p>
                <img src="${recipe.image}" alt="${recipe.name}" class="w-full h-96 object-cover rounded-lg mb-8 shadow-md">

                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div>
                        <h3 class="text-2xl font-semibold text-orange-700 mb-4 border-b pb-2">Ingredientes</h3>
                        <ul class="list-disc list-inside text-gray-700 space-y-2">
                            ${recipe.ingredients.map(ing => `<li>${ing}</li>`).join('')}
                        </ul>
                    </div>
                    <div>
                        <h3 class="text-2xl font-semibold text-orange-700 mb-4 border-b pb-2">Pasos de Preparación</h3>
                        <ol class="list-decimal list-inside text-gray-700 space-y-2">
                            ${recipe.preparationSteps.map(step => `<li>${step}</li>`).join('')}
                        </ol>
                    </div>
                </div>

                ${recipe.nutritionalInfo ? `
                    <div class="mt-8 p-6 bg-amber-50 rounded-lg shadow-inner">
                        <h3 class="text-2xl font-semibold text-orange-700 mb-4 border-b pb-2">Información Nutricional</h3>
                        <p class="text-gray-700">${recipe.nutritionalInfo}</p>
                    </div>
                ` : ''}

                <button onclick="setView('recetas')" class="mt-10 px-6 py-3 bg-amber-500 text-white rounded-lg hover:bg-amber-600 transition-colors duration-200 font-semibold">Volver a Recetas</button>
            `;
        }

        // Add event listener to recipe cards (for Inicio and Recetas pages)
        function addRecipeCardListeners() {
            document.querySelectorAll('.recipe-card').forEach(card => {
                card.addEventListener('click', (event) => {
                    const recipeId = event.currentTarget.dataset.recipeId;
                    setView('recipe-detail', recipeId);
                });
            });
        }

        // --- Event Listeners for Main Navigation ---
        document.getElementById('nav-inicio').addEventListener('click', () => setView('inicio'));
        document.getElementById('nav-recetas').addEventListener('click', () => setView('recetas'));
        document.getElementById('nav-culturas').addEventListener('click', () => setView('culturas'));
        document.getElementById('nav-foros').addEventListener('click', () => setView('foros'));

        // Mobile menu toggle
        mobileMenuButton.addEventListener('click', () => {
            mobileMenu.classList.toggle('hidden');
        });

        // Mobile navigation buttons
        document.getElementById('mobile-nav-inicio').addEventListener('click', () => {
            setView('inicio');
            mobileMenu.classList.add('hidden');
        });
        document.getElementById('mobile-nav-recetas').addEventListener('click', () => {
            setView('recetas');
            mobileMenu.classList.add('hidden');
        });
        document.getElementById('mobile-nav-culturas').addEventListener('click', () => {
            setView('culturas');
            mobileMenu.classList.add('hidden');
        });
        document.getElementById('mobile-nav-foros').addEventListener('click', () => {
            setView('foros');
            mobileMenu.classList.add('hidden');
        });

        // --- Initial Load ---
        document.addEventListener('DOMContentLoaded', () => {
            populateSidebar();
            setView('inicio'); // Load the home page by default
        });
    </script>
</body>
</html>
