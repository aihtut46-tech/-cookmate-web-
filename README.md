import React, { useState } from "react";
import {
  Search,
  Heart,
  Clock,
  Star,
  ChefHat,
  Home,
} from "lucide-react";

const recipesData = [
  {
    id: 1,
    name: "Thai Basil Chicken",
    category: "Asian",
    time: "20 min",
    rating: 4.8,
    favorite: false,
    img: "https://images.unsplash.com/photo-1562967962-e7e00e4952df?auto=format&fit=crop&w=300&q=80",
  },
  {
    id: 2,
    name: "Spaghetti Carbonara",
    category: "Fast Food",
    time: "25 min",
    rating: 4.7,
    favorite: false,
    img: "https://images.unsplash.com/photo-1612874742237-6526221588e3?auto=format&fit=crop&w=300&q=80",
  },
  {
    id: 3,
    name: "Fried Rice",
    category: "Asian",
    time: "15 min",
    rating: 4.5,
    favorite: false,
    img: "https://images.unsplash.com/photo-1603133872833-224262b5ca0a?auto=format&fit=crop&w=300&q=80",
  },
  {
    id: 4,
    name: "Mohinga",
    category: "Myanmar",
    time: "30 min",
    rating: 4.9,
    favorite: false,
    img: "https://images.unsplash.com/photo-1544025162-d76694265947?auto=format&fit=crop&w=300&q=80",
  },
];

const categories = ["All", "Myanmar", "Asian", "Fast Food"];

export default function CookMateApp() {
  const [recipes, setRecipes] = useState(recipesData);
  const [search, setSearch] = useState("");
  const [selectedCategory, setSelectedCategory] = useState("All");

  // Toggle Favorite
  const toggleFavorite = (id) => {
    const updated = recipes.map((recipe) =>
      recipe.id === id
        ? { ...recipe, favorite: !recipe.favorite }
        : recipe
    );

    setRecipes(updated);
  };

  // Filter Recipes
  const filteredRecipes = recipes.filter((recipe) => {
    const matchesSearch = recipe.name
      .toLowerCase()
      .includes(search.toLowerCase());

    const matchesCategory =
      selectedCategory === "All" ||
      recipe.category === selectedCategory;

    return matchesSearch && matchesCategory;
  });

  return (
    <div className="max-w-md mx-auto min-h-screen bg-gray-50 shadow-xl pb-24">
      {/* HEADER */}
      <header className="bg-white p-6 sticky top-0 z-10">
        <div className="flex justify-between items-center mb-5">
          <div>
            <h1 className="text-gray-400 text-sm">
              Hello, Chef 👋
            </h1>

            <p className="text-2xl font-bold text-gray-800 leading-snug">
              What do you want to cook today?
            </p>
          </div>

          <div className="w-12 h-12 rounded-full bg-orange-100 flex items-center justify-center">
            <ChefHat className="text-orange-500 w-7 h-7" />
          </div>
        </div>

        {/* SEARCH */}
        <div className="relative">
          <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 w-5 h-5" />

          <input
            type="text"
            placeholder="Search recipes..."
            value={search}
            onChange={(e) => setSearch(e.target.value)}
            className="w-full bg-gray-100 rounded-xl py-3 pl-10 pr-4 focus:outline-none focus:ring-2 focus:ring-orange-400"
          />
        </div>
      </header>

      {/* CATEGORY */}
      <section className="px-6 py-4 flex gap-3 overflow-x-auto no-scrollbar">
        {categories.map((cat) => (
          <button
            key={cat}
            onClick={() => setSelectedCategory(cat)}
            className={`px-4 py-2 rounded-xl whitespace-nowrap text-sm transition-all ${
              selectedCategory === cat
                ? "bg-orange-500 text-white"
                : "bg-white text-gray-600 border"
            }`}
          >
            {cat}
          </button>
        ))}
      </section>

      {/* RECIPE LIST */}
      <section className="px-6">
        <div className="flex justify-between items-center mb-5">
          <h2 className="text-lg font-bold text-gray-800">
            Trending Recipes
          </h2>

          <button className="text-orange-500 text-sm font-medium">
            See all
          </button>
        </div>

        <div className="grid grid-cols-2 gap-4">
          {filteredRecipes.map((recipe) => (
            <div
              key={recipe.id}
              className="bg-white rounded-2xl overflow-hidden shadow-sm border"
            >
              <div className="relative">
                <img
                  src={recipe.img}
                  alt={recipe.name}
                  className="w-full h-32 object-cover"
                />

                <button
                  onClick={() => toggleFavorite(recipe.id)}
                  className="absolute top-2 right-2 bg-white p-1.5 rounded-full shadow"
                >
                  <Heart
                    className={`w-4 h-4 ${
                      recipe.favorite
                        ? "fill-red-500 text-red-500"
                        : "text-gray-400"
                    }`}
                  />
                </button>
              </div>

              <div className="p-3">
                <h3 className="font-semibold text-gray-800 text-sm">
                  {recipe.name}
                </h3>

                <p className="text-xs text-gray-400 mt-1">
                  {recipe.category}
                </p>

                <div className="flex justify-between items-center mt-3 text-xs text-gray-500">
                  <div className="flex items-center gap-1">
                    <Clock className="w-3 h-3 text-orange-500" />
                    {recipe.time}
                  </div>

                  <div className="flex items-center gap-1">
                    <Star className="w-3 h-3 text-yellow-400 fill-yellow-400" />
                    {recipe.rating}
                  </div>
                </div>
              </div>
            </div>
          ))}
        </div>

        {/* EMPTY STATE */}
        {filteredRecipes.length === 0 && (
          <div className="text-center py-16 text-gray-400">
            No recipes found 🍳
          </div>
        )}
      </section>

      {/* BOTTOM NAVIGATION */}
      <nav className="fixed bottom-0 max-w-md w-full bg-white border-t px-6 py-3 flex justify-between">
        <button className="flex flex-col items-center text-orange-500">
          <Home className="w-6 h-6" />
          <span className="text-[10px] mt-1 font-medium">
            Home
          </span>
        </button>

        <button className="flex flex-col items-center text-gray-400">
          <Search className="w-6 h-6" />
          <span className="text-[10px] mt-1">Search</span>
        </button>

        <button className="flex flex-col items-center text-gray-400">
          <ChefHat className="w-6 h-6" />
          <span className="text-[10px] mt-1">AI Chef</span>
        </button>

        <button className="flex flex-col items-center text-gray-400">
          <Heart className="w-6 h-6" />
          <span className="text-[10px] mt-1">Saved</span>
        </button>
      </nav>
    </div>
  );
}
