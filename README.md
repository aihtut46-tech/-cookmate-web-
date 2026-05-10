import React, { useState, useMemo } from 'react';
import { ChevronRight, Clock, Star, Heart, Search, Home, Compass, ChefHat, Bookmark, User, Menu, X } from 'lucide-react';

const CookMateApp = () => {
  const [currentScreen, setCurrentScreen] = useState('home');
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedCategory, setSelectedCategory] = useState('All');
  const [favorites, setFavorites] = useState(new Set());
  const [selectedRecipe, setSelectedRecipe] = useState(null);
  const [menuOpen, setMenuOpen] = useState(false);

  // Mock recipe data
  const recipes = [
    { id: 1, name: 'Thai Basil Chicken', time: 30, rating: 4.8, reviews: 2543, category: 'Asian', image: '🍗', description: 'Fragrant Thai chicken with basil leaves and chilies', difficulty: 'Easy', servings: 4, ingredients: ['Chicken breast', 'Thai basil', 'Garlic', 'Chilies', 'Fish sauce', 'Lime'] },
    { id: 2, name: 'Spaghetti Carbonara', time: 25, rating: 4.7, reviews: 1892, category: 'Italian', image: '🍝', description: 'Creamy Italian pasta with bacon and parmesan', difficulty: 'Medium', servings: 2, ingredients: ['Spaghetti', 'Eggs', 'Pancetta', 'Parmesan', 'Black pepper'] },
    { id: 3, name: 'Fried Rice', time: 20, rating: 4.6, reviews: 3102, category: 'Asian', image: '🍚', description: 'Golden fried rice with vegetables and egg', difficulty: 'Easy', servings: 4, ingredients: ['Rice', 'Eggs', 'Peas', 'Carrots', 'Soy sauce', 'Garlic'] },
    { id: 4, name: 'Chocolate Cake', time: 45, rating: 4.9, reviews: 2156, category: 'Dessert', image: '🍰', description: 'Rich, moist chocolate cake with ganache', difficulty: 'Medium', servings: 8, ingredients: ['Flour', 'Cocoa', 'Eggs', 'Butter', 'Sugar', 'Milk'] },
    { id: 5, name: 'Pad Thai Noodles', time: 25, rating: 4.7, reviews: 1534, category: 'Asian', image: '🥢', description: 'Traditional Thai noodles with peanut sauce', difficulty: 'Easy', servings: 2, ingredients: ['Rice noodles', 'Peanut butter', 'Lime', 'Bean sprouts', 'Shrimp', 'Eggs'] },
    { id: 6, name: 'Chicken Curry', time: 40, rating: 4.8, reviews: 2789, category: 'Myanmar', image: '🍛', description: 'Authentic Myanmar chicken curry with rice', difficulty: 'Medium', servings: 4, ingredients: ['Chicken', 'Onion', 'Garlic', 'Ginger', 'Curry powder', 'Coconut milk'] },
  ];

  const categories = ['All', 'Myanmar', 'Asian', 'Italian', 'Dessert'];

  // Search & Filter Logic
  const filteredRecipes = useMemo(() => {
    return recipes.filter(recipe => {
      const matchesSearch = recipe.name.toLowerCase().includes(searchQuery.toLowerCase());
      const matchesCategory = selectedCategory === 'All' || recipe.category === selectedCategory;
      return matchesSearch && matchesCategory;
    });
  }, [searchQuery, selectedCategory]);

  const toggleFavorite = (id) => {
    const newFavorites = new Set(favorites);
    if (newFavorites.has(id)) {
      newFavorites.delete(id);
    } else {
      newFavorites.add(id);
    }
    setFavorites(newFavorites);
  };

  // --- UI Components ---

  const HomeScreen = () => (
    <div className="flex-1 overflow-y-auto pb-24">
      <div className="bg-gradient-to-b from-orange-50 to-white px-6 pt-8 pb-6 border-b border-orange-100">
        <div className="flex items-center justify-between mb-6">
          <div>
            <h1 className="text-3xl font-bold text-gray-900">Hello, Chef 👋</h1>
            <p className="text-gray-600 text-sm mt-1">What do you want to cook today?</p>
          </div>
          <button onClick={() => setMenuOpen(!menuOpen)} className="p-2 hover:bg-orange-100 rounded-full transition">
            {menuOpen ? <X size={24} /> : <Menu size={24} />}
          </button>
        </div>

        <div className="relative">
          <Search size={20} className="absolute left-4 top-1/2 transform -translate-y-1/2 text-gray-400" />
          <input
            type="text"
            placeholder="Search recipes..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full bg-white border border-gray-200 rounded-xl pl-12 pr-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-orange-400 shadow-sm"
          />
        </div>
      </div>

      {/* Category Pills */}
      <div className="px-6 py-5 flex gap-3 overflow-x-auto no-scrollbar border-b border-gray-100">
        {categories.map((cat) => (
          <button
            key={cat}
            onClick={() => setSelectedCategory(cat)}
            className={`px-6 py-2 rounded-full font-medium text-sm whitespace-nowrap transition-all duration-200 ${
              selectedCategory === cat ? 'bg-orange-500 text-white' : 'bg-orange-50 text-orange-600 hover:bg-orange-100'
            }`}
          >
            {cat}
          </button>
        ))}
      </div>

      <div className="px-6 py-6">
        <h2 className="text-xl font-bold text-gray-900 mb-4">
          {selectedCategory === 'All' ? 'All Recipes' : `${selectedCategory} Specials`}
        </h2>
        <div className="space-y-4">
          {filteredRecipes.map((recipe) => (
            <div
              key={recipe.id}
              onClick={() => { setSelectedRecipe(recipe); setCurrentScreen('recipe-detail'); }}
              className="bg-white rounded-2xl p-4 flex items-center gap-4 shadow-sm border border-gray-50 hover:shadow-md transition-all cursor-pointer"
            >
              <div className="bg-orange-100 rounded-xl w-20 h-20 flex items-center justify-center text-3xl flex-shrink-0">
                {recipe.image}
              </div>
              <div className="flex-1 min-w-0">
                <h3 className="font-bold text-gray-900 text-sm truncate">{recipe.name}</h3>
                <p className="text-xs text-gray-500 mt-1">{recipe.time} min • {recipe.difficulty}</p>
                <div className="flex items-center gap-2 mt-2">
                  <Star size={12} className="fill-yellow-400 text-yellow-400" />
                  <span className="text-xs font-bold">{recipe.rating}</span>
                </div>
              </div>
              <button
                onClick={(e) => { e.stopPropagation(); toggleFavorite(recipe.id); }}
                className="p-2 hover:bg-orange-50 rounded-full transition"
              >
                <Heart size={20} className={favorites.has(recipe.id) ? 'fill-red-500 text-red-500' : 'text-gray-300'} />
              </button>
            </div>
          ))}
          {filteredRecipes.length === 0 && (
            <p className="text-center text-gray-400 py-10">No recipes found...</p>
          )}
        </div>
      </div>
    </div>
  );

  const RecipeDetailScreen = () => (
    <div className="flex-1 overflow-y-auto pb-24 bg-white">
      <div className="relative h-64 bg-orange-100 flex items-center justify-center text-8xl">
        <button 
          onClick={() => setCurrentScreen('home')}
          className="absolute top-6 left-6 p-2 bg-white rounded-full shadow-lg z-10"
        >
          <X size={20} />
        </button>
        {selectedRecipe?.image}
      </div>
      <div className="px-6 py-6">
        <div className="flex justify-between items-start mb-4">
          <h1 className="text-2xl font-bold text-gray-900">{selectedRecipe?.name}</h1>
          <button onClick={() => toggleFavorite(selectedRecipe.id)}>
            <Heart size={24} className={favorites.has(selectedRecipe?.id) ? 'fill-red-500 text-red-500' : 'text-gray-300'} />
          </button>
        </div>
        <div className="flex gap-4 mb-6">
           <div className="bg-orange-50 px-3 py-2 rounded-lg flex items-center gap-2">
              <Clock size={16} className="text-orange-500" />
              <span className="text-sm font-medium">{selectedRecipe?.time} mins</span>
           </div>
           <div className="bg-orange-50 px-3 py-2 rounded-lg flex items-center gap-2">
              <ChefHat size={16} className="text-orange-500" />
              <span className="text-sm font-medium">{selectedRecipe?.difficulty}</span>
           </div>
        </div>
        <h3 className="font-bold mb-3">Ingredients</h3>
        <ul className="space-y-2 mb-8">
          {selectedRecipe?.ingredients.map((ing, i) => (
            <li key={i} className="flex items-center gap-3 text-gray-600 border-b border-gray-50 pb-2">
              <div className="w-2 h-2 bg-orange-400 rounded-full" />
              {ing}
            </li>
          ))}
        </ul>
        <button 
          onClick={() => setCurrentScreen('home')}
          className="w-full bg-orange-500 text-white py-4 rounded-2xl font-bold shadow-lg shadow-orange-200"
        >
          Start Cooking
        </button>
      </div>
    </div>
  );

  const SavedScreen = () => (
    <div className="flex-1 overflow-y-auto pb-24 px-6 pt-8">
      <h1 className="text-3xl font-bold mb-6">Saved Recipes</h1>
      <div className="space-y-4">
        {recipes.filter(r => favorites.has(r.id)).map(recipe => (
          <div key={recipe.id} className="flex gap-4 bg-gray-50 p-4 rounded-2xl">
            <span className="text-4xl">{recipe.image}</span>
            <div>
              <h3 className="font-bold">{recipe.name}</h3>
              <p className="text-sm text-gray-500">{recipe.time} mins</p>
            </div>
          </div>
        ))}
        {favorites.size === 0 && <p className="text-gray-400">No favorites yet.</p>}
      </div>
    </div>
  );

  return (
    <div className="flex justify-center bg-gray-100 min-h-screen">
      <div className="w-full max-w-md bg-white min-h-screen flex flex-col shadow-xl relative overflow-hidden">
        
        {currentScreen === 'home' && <HomeScreen />}
        {currentScreen === 'recipe-detail' && <RecipeDetailScreen />}
        {currentScreen === 'saved' && <SavedScreen />}
        {currentScreen === 'profile' && <div className="p-10 text-center">Profile Screen</div>}

        {/* Navigation Bar */}
        <div className="absolute bottom-0 left-0 right-0 bg-white/80 backdrop-blur-md border-t border-gray-100 flex justify-around py-4">
          <button onClick={() => setCurrentScreen('home')} className={currentScreen === 'home' ? 'text-orange-500' : 'text-gray-400'}>
            <Home size={24} />
          </button>
          <button onClick={() => setCurrentScreen('saved')} className={currentScreen === 'saved' ? 'text-orange-500' : 'text-gray-400'}>
            <Bookmark size={24} />
          </button>
          <button onClick={() => setCurrentScreen('profile')} className={currentScreen === 'profile' ? 'text-orange-500' : 'text-gray-400'}>
            <User size={24} />
          </button>
        </div>
      </div>
    </div>
  );
};

export default CookMateApp;
