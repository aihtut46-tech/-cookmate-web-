<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CookMate AI - Recipe App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        .no-scrollbar::-webkit-scrollbar { display: none; }
        body { font-family: 'Inter', sans-serif; }
        /* ခလုတ်နှိပ်ရင် အရောင်ပြောင်းဖို့ Active Class */
        .cat-btn-active { background-color: #f97316 !important; color: white !important; box-shadow: 0 10px 15px -3px rgba(251, 146, 60, 0.3) !important; }
    </style>
</head>
<body class="bg-gray-50 flex justify-center">

    <div class="w-full max-w-md bg-white min-h-screen shadow-2xl relative overflow-hidden">
        
        <header class="p-6 pb-2">
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h1 class="text-gray-400 text-sm font-medium">Hello, Chef!</h1>
                    <p class="text-xl font-bold text-gray-800">What do you want to cook today?</p>
                </div>
                <div class="w-12 h-12 bg-orange-100 rounded-full flex items-center justify-center border-2 border-white shadow-sm">
                    <i class="fa-solid fa-user-chef text-orange-500 text-xl"></i>
                </div>
            </div>

            <div class="relative group">
                <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-gray-400"></i>
                <input type="text" id="searchInput" placeholder="Search recipes..." 
                    class="w-full pl-12 pr-4 py-4 bg-gray-100 rounded-2xl focus:outline-none focus:ring-2 focus:ring-orange-500 transition-all border-none">
            </div>
        </header>

        <div id="categoryContainer" class="px-6 py-4 flex gap-3 overflow-x-auto no-scrollbar">
            <button onclick="filterCategory('All', this)" class="cat-btn cat-btn-active px-6 py-2 bg-white text-gray-500 rounded-xl text-sm font-semibold border border-gray-100 shadow-sm">All</button>
            <button onclick="filterCategory('Myanmar', this)" class="cat-btn px-6 py-2 bg-white text-gray-500 rounded-xl text-sm font-semibold border border-gray-100 shadow-sm">Myanmar</button>
            <button onclick="filterCategory('Asian', this)" class="cat-btn px-6 py-2 bg-white text-gray-500 rounded-xl text-sm font-semibold border border-gray-100 shadow-sm">Asian</button>
            <button onclick="filterCategory('Fast Food', this)" class="cat-btn px-6 py-2 bg-white text-gray-500 rounded-xl text-sm font-semibold border border-gray-100 shadow-sm">Fast Food</button>
        </div>

        <section class="px-6 py-4">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-lg font-bold text-gray-800 flex items-center gap-2">
                    <i class="fa-solid fa-fire text-orange-500"></i> <span id="sectionTitle">Trending Recipes</span>
                </h2>
                <a href="#" class="text-orange-500 text-sm font-bold">See all</a>
            </div>
            
            <div id="recipeContainer" class="flex gap-4 overflow-x-auto no-scrollbar pb-4">
                </div>
        </section>

        <section class="px-6 py-2 mb-20">
            <h2 class="text-lg font-bold text-gray-800 mb-4">Quick & Easy</h2>
            <div class="bg-orange-50 rounded-3xl p-4 flex items-center gap-4 border border-orange-100">
                <img src="https://images.unsplash.com/photo-1603133872833-224262b5ca0a?w=200" class="w-20 h-20 object-cover rounded-2xl shadow-sm">
                <div>
                    <h4 class="font-bold text-gray-800 text-sm">Special Fried Rice</h4>
                    <p class="text-xs text-gray-500 mt-1">Perfect for a quick dinner</p>
                    <div class="flex gap-3 mt-2">
                         <span class="text-[10px] font-bold text-orange-500 underline cursor-pointer">Get Recipe</span>
                    </div>
                </div>
            </div>
        </section>

        <nav class="absolute bottom-0 w-full bg-white/80 backdrop-blur-md border-t border-gray-100 px-8 py-4 flex justify-between items-center rounded-t-[30px] shadow-2xl">
            <i class="fa-solid fa-house text-orange-500 text-xl cursor-pointer"></i>
            <i class="fa-solid fa-magnifying-glass text-gray-300 text-xl cursor-pointer"></i>
            <div class="bg-orange-500 p-3 rounded-full -mt-12 border-4 border-white shadow-lg shadow-orange-200 cursor-pointer hover:scale-105 transition">
                <i class="fa-solid fa-robot text-white text-xl"></i>
            </div>
            <i class="fa-solid fa-heart text-gray-300 text-xl cursor-pointer"></i>
            <i class="fa-solid fa-user text-gray-300 text-xl cursor-pointer"></i>
        </nav>

    </div>

    <script>
        // ၁။ ဟင်းချက်နည်း Data များ (ဒီနေရာမှာ မိမိစိတ်ကြိုက် ထပ်တိုးနိုင်ပါတယ်)
        const allRecipes = [
            { name: "Thai Basil Chicken", time: "20 min", rating: 4.8, category: "Asian", img: "https://images.unsplash.com/photo-1562967962-e7e00e4952df?w=300" },
            { name: "Spaghetti Carbonara", time: "25 min", rating: 4.7, category: "Fast Food", img: "https://images.unsplash.com/photo-1612874742237-6526221588e3?w=300" },
            { name: "Myanmar Chicken Curry", time: "45 min", rating: 4.9, category: "Myanmar", img: "https://images.unsplash.com/photo-1604908176997-125f25cc6f3d?w=300" },
            { name: "Sushi Rolls", time: "30 min", rating: 4.6, category: "Asian", img: "https://images.unsplash.com/photo-1579871494447-9811cf80d66c?w=300" },
            { name: "Crispy Burger", time: "15 min", rating: 4.5, category: "Fast Food", img: "https://images.unsplash.com/photo-1568901346375-23c9450c58cd?w=300" },
            { name: "Mohinga", time: "60 min", rating: 5.0, category: "Myanmar", img: "https://images.unsplash.com/photo-1645037920786-fb7c9c0f99a6?w=300" }
        ];

        const recipeContainer = document.getElementById('recipeContainer');
        const searchInput = document.getElementById('searchInput');
        const sectionTitle = document.getElementById('sectionTitle');

        // ၂။ Data တွေကို Screen ပေါ်ပြပေးမယ့် Function
        function renderRecipes(recipesList) {
            recipeContainer.innerHTML = ''; // အရင်ရှိတာကို ရှင်းမယ်
            
            if(recipesList.length === 0) {
                recipeContainer.innerHTML = '<p class="text-gray-400 text-sm mt-4">No recipes found!</p>';
                return;
            }

            recipesList.forEach(recipe => {
                const card = `
                <div class="min-w-[170px] bg-white rounded-3xl p-3 shadow-xl shadow-gray-100 border border-gray-50 cursor-pointer hover:scale-105 transition-transform" onclick="alert('Open recipe: ${recipe.name}')">
                    <img src="${recipe.img}" class="w-full h-32 object-cover rounded-2xl mb-3 shadow-md">
                    <h3 class="font-bold text-sm text-gray-800 truncate">${recipe.name}</h3>
                    <div class="flex items-center justify-between mt-2">
                        <span class="text-[10px] text-gray-400"><i class="fa-regular fa-clock text-orange-500"></i> ${recipe.time}</span>
                        <span class="text-[10px] text-gray-400"><i class="fa-solid fa-star text-yellow-400"></i> ${recipe.rating}</span>
                    </div>
                </div>`;
                recipeContainer.innerHTML += card;
            });
        }

        // ၃။ ပထမဆုံး App စဖွင့်တဲ့အချိန်မှာ အကုန်ပြထားမယ်
        renderRecipes(allRecipes);

        // ၄။ Search Bar မှာ စာရိုက်ရှာရင် အလုပ်လုပ်မယ့် စနစ်
        searchInput.addEventListener('input', (e) => {
            const searchText = e.target.value.toLowerCase();
            const filteredRecipes = allRecipes.filter(recipe => 
                recipe.name.toLowerCase().includes(searchText)
            );
            sectionTitle.innerText = searchText ? "Search Results" : "Trending Recipes";
            renderRecipes(filteredRecipes);
        });

        // ၅။ Category ခလုတ်တွေ နှိပ်ရင် အလုပ်လုပ်မယ့် စနစ်
        function filterCategory(categoryName, btnElement) {
            // ခလုတ်အရောင်ပြောင်းတဲ့ အပိုင်း
            document.querySelectorAll('.cat-btn').forEach(btn => btn.classList.remove('cat-btn-active', 'bg-orange-500', 'text-white'));
            document.querySelectorAll('.cat-btn').forEach(btn => btn.classList.add('bg-white', 'text-gray-500'));
            
            btnElement.classList.remove('bg-white', 'text-gray-500');
            btnElement.classList.add('cat-btn-active');

            // Data ကို စစ်ထုတ်တဲ့ အပိုင်း
            searchInput.value = ''; // Search box ကိုရှင်းမယ်
            sectionTitle.innerText = categoryName === 'All' ? "Trending Recipes" : categoryName + " Recipes";

            if(categoryName === 'All') {
                renderRecipes(allRecipes);
            } else {
                const filteredRecipes = allRecipes.filter(recipe => recipe.category === categoryName);
                renderRecipes(filteredRecipes);
            }
        }
    </script>
</body>
</html>
