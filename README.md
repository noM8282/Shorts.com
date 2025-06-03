# Shorts.com import { useState, useEffect } from ‘react’;
import { ShoppingCart, Star, Heart, Search, Menu, X, Plus, Minus, Filter, Shirt } from ‘lucide-react’;

const ShirtStore = () => {
const [cart, setCart] = useState([]);
const [searchTerm, setSearchTerm] = useState(’’);
const [selectedCategory, setSelectedCategory] = useState(‘all’);
const [selectedSize, setSelectedSize] = useState(‘all’);
const [selectedColor, setSelectedColor] = useState(‘all’);
const [priceRange, setPriceRange] = useState([0, 1000]);
const [showCart, setShowCart] = useState(false);
const [showFilters, setShowFilters] = useState(false);
const [favorites, setFavorites] = useState([]);
const [showMobileMenu, setShowMobileMenu] = useState(false);

const shirts = [
{
id: 1,
name: ‘חולצת פולו קלאסית’,
price: 149,
originalPrice: 199,
category: ‘polo’,
sizes: [‘S’, ‘M’, ‘L’, ‘XL’, ‘XXL’],
colors: [‘לבן’, ‘שחור’, ‘כחול נייבי’, ‘אפור’],
image: ‘https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?w=400&h=300&fit=crop’,
rating: 4.8,
reviews: 234,
description: ‘חולצת פולו איכותית מכותנה 100%, נוחה ומעוצבת’,
inStock: true,
material: ‘כותנה 100%’,
fit: ‘רגיל’
},
{
id: 2,
name: ‘חולצה מכופתרת עסקית’,
price: 189,
originalPrice: 249,
category: ‘dress’,
sizes: [‘S’, ‘M’, ‘L’, ‘XL’],
colors: [‘לבן’, ‘תכלת’, ‘ורוד בהיר’, ‘פסים כחולים’],
image: ‘https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?w=400&h=300&fit=crop’,
rating: 4.9,
reviews: 456,
description: ‘חולצה מכופתרת אלגנטית למשרד ואירועים’,
inStock: true,
material: ‘כותנה וסיבים סינתטיים’,
fit: ‘צמוד’
},
{
id: 3,
name: ‘טי-שירט בייסיק’,
price: 79,
originalPrice: 99,
category: ‘tshirt’,
sizes: [‘XS’, ‘S’, ‘M’, ‘L’, ‘XL’, ‘XXL’],
colors: [‘לבן’, ‘שחור’, ‘אפור’, ‘כחול’, ‘אדום’, ‘ירוק’],
image: ‘https://images.unsplash.com/photo-1521572163474-6864f9cf17ab?w=400&h=300&fit=crop’,
rating: 4.5,
reviews: 1234,
description: ‘טי-שירט בסיסי איכותי לשימוש יומיומי’,
inStock: true,
material: ‘כותנה 100%’,
fit: ‘רגיל’
},
{
id: 4,
name: ‘חולצת ספורט טכנית’,
price: 129,
originalPrice: 159,
category: ‘sport’,
sizes: [‘S’, ‘M’, ‘L’, ‘XL’],
colors: [‘שחור’, ‘כחול’, ‘אדום’, ‘ירוק ליים’],
image: ‘https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop’,
rating: 4.7,
reviews: 567,
description: ‘חולצת ספורט עם טכנולוגיית ייבוש מהיר’,
inStock: true,
material: ‘פוליאסטר טכני’,
fit: ‘ספורטיבי’
},
{
id: 5,
name: ‘חולצה מעוצבת הדפס’,
price: 99,
originalPrice: 139,
category: ‘casual’,
sizes: [‘S’, ‘M’, ‘L’, ‘XL’],
colors: [‘לבן עם הדפס’, ‘שחור עם הדפס’, ‘כחול עם הדפס’],
image: ‘https://images.unsplash.com/photo-1583743814966-8936f37f86c6?w=400&h=300&fit=crop’,
rating: 4.3,
reviews: 189,
description: ‘חולצה עם הדפס יצירתי וייחודי’,
inStock: true,
material: ‘כותנה וסיבים סינתטיים’,
fit: ‘רגיל’
},
{
id: 6,
name: ‘חולצת לינן קיצית’,
price: 169,
originalPrice: 219,
category: ‘casual’,
sizes: [‘S’, ‘M’, ‘L’, ‘XL’],
colors: [‘לבן’, ‘בז׳’, ‘כחול בהיר’, ‘ירוק בהיר’],
image: ‘https://images.unsplash.com/photo-1618354691373-d851c5c3a990?w=400&h=300&fit=crop’,
rating: 4.6,
reviews: 321,
description: ‘חולצת לינן קלילה ונושמת לקיץ’,
inStock: false,
material: ‘לינן 100%’,
fit: ‘רגיל’
}
];

const categories = [
{ id: ‘all’, name: ‘כל החולצות’, icon: ‘👕’ },
{ id: ‘polo’, name: ‘פולו’, icon: ‘🏌️’ },
{ id: ‘dress’, name: ‘עסקיות’, icon: ‘👔’ },
{ id: ‘tshirt’, name: ‘טי-שירט’, icon: ‘👕’ },
{ id: ‘sport’, name: ‘ספורט’, icon: ‘🏃’ },
{ id: ‘casual’, name: ‘קז׳ואל’, icon: ‘😎’ }
];

const sizes = [‘XS’, ‘S’, ‘M’, ‘L’, ‘XL’, ‘XXL’];
const colors = [‘לבן’, ‘שחור’, ‘כחול’, ‘אפור’, ‘אדום’, ‘ירוק’, ‘ורוד’, ‘בז׳’];

const filteredShirts = shirts.filter(shirt => {
const matchesSearch = shirt.name.toLowerCase().includes(searchTerm.toLowerCase());
const matchesCategory = selectedCategory === ‘all’ || shirt.category === selectedCategory;
const matchesSize = selectedSize === ‘all’ || shirt.sizes.includes(selectedSize);
const matchesColor = selectedColor === ‘all’ || shirt.colors.some(color => color.includes(selectedColor));
const matchesPrice = shirt.price >= priceRange[0] && shirt.price <= priceRange[1];
return matchesSearch && matchesCategory && matchesSize && matchesColor && matchesPrice;
});

const addToCart = (shirt, selectedSize, selectedColor) => {
const cartItem = {
…shirt,
selectedSize: selectedSize || shirt.sizes[0],
selectedColor: selectedColor || shirt.colors[0],
cartId: `${shirt.id}-${selectedSize || shirt.sizes[0]}-${selectedColor || shirt.colors[0]}`
};

```
setCart(prev => {
  const existing = prev.find(item => item.cartId === cartItem.cartId);
  if (existing) {
    return prev.map(item =>
      item.cartId === cartItem.cartId ? { ...item, quantity: item.quantity + 1 } : item
    );
  }
  return [...prev, { ...cartItem, quantity: 1 }];
});
```

};

const removeFromCart = (cartId) => {
setCart(prev => prev.filter(item => item.cartId !== cartId));
};

const updateQuantity = (cartId, change) => {
setCart(prev =>
prev.map(item => {
if (item.cartId === cartId) {
const newQuantity = item.quantity + change;
return newQuantity > 0 ? { …item, quantity: newQuantity } : item;
}
return item;
}).filter(item => item.quantity > 0)
);
};

const toggleFavorite = (shirtId) => {
setFavorites(prev =>
prev.includes(shirtId)
? prev.filter(id => id !== shirtId)
: […prev, shirtId]
);
};

const getTotalPrice = () => {
return cart.reduce((total, item) => total + (item.price * item.quantity), 0);
};

const getTotalItems = () => {
return cart.reduce((total, item) => total + item.quantity, 0);
};

const ShirtCard = ({ shirt }) => {
const [selectedSize, setSelectedSize] = useState(shirt.sizes[0]);
const [selectedColor, setSelectedColor] = useState(shirt.colors[0]);

```
return (
  <div className="bg-white rounded-2xl shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-2 overflow-hidden group">
    <div className="relative overflow-hidden">
      <img
        src={shirt.image}
        alt={shirt.name}
        className="w-full h-64 object-cover group-hover:scale-110 transition-transform duration-500"
      />
      <button
        onClick={() => toggleFavorite(shirt.id)}
        className="absolute top-4 left-4 p-2 bg-white/80 rounded-full hover:bg-white transition-colors"
      >
        <Heart
          size={18}
          className={favorites.includes(shirt.id) ? 'text-red-500 fill-current' : 'text-gray-600'}
        />
      </button>
      {!shirt.inStock && (
        <div className="absolute inset-0 bg-black/50 flex items-center justify-center">
          <span className="bg-red-500 text-white px-4 py-2 rounded-full font-bold">
            אזל מהמלאי
          </span>
        </div>
      )}
      {shirt.originalPrice > shirt.price && (
        <div className="absolute top-4 right-4 bg-red-500 text-white px-3 py-1 rounded-full text-sm font-bold">
          -{Math.round((1 - shirt.price / shirt.originalPrice) * 100)}%
        </div>
      )}
    </div>

    <div className="p-6">
      <h3 className="text-xl font-bold text-gray-800 mb-2">{shirt.name}</h3>
      <p className="text-gray-600 text-sm mb-3">{shirt.description}</p>
      
      <div className="flex items-center justify-between mb-3">
        <div className="flex items-center space-x-reverse space-x-1">
          <Star className="text-yellow-400 fill-current" size={16} />
          <span className="text-sm font-medium">{shirt.rating}</span>
          <span className="text-xs text-gray-500">({shirt.reviews} ביקורות)</span>
        </div>
        <div className="text-sm text-gray-600">
          <span className="font-medium">{shirt.material}</span>
        </div>
      </div>

      <div className="mb-4">
        <div className="flex items-center justify-between text-sm text-gray-600 mb-2">
          <span>גודל:</span>
          <span>צבע:</span>
        </div>
        <div className="flex items-center justify-between mb-3">
          <select
            value={selectedSize}
            onChange={(e) => setSelectedSize(e.target.value)}
            className="px-3 py-1 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-500"
          >
            {shirt.sizes.map(size => (
              <option key={size} value={size}>{size}</option>
            ))}
          </select>
          <select
            value={selectedColor}
            onChange={(e) => setSelectedColor(e.target.value)}
            className="px-3 py-1 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-blue-500"
          >
            {shirt.colors.map(color => (
              <option key={color} value={color}>{color}</option>
            ))}
          </select>
        </div>
      </div>

      <div className="flex items-center justify-between mb-4">
        <div className="flex items-center space-x-reverse space-x-2">
          <span className="text-2xl font-bold text-blue-600">₪{shirt.price}</span>
          {shirt.originalPrice > shirt.price && (
            <span className="text-lg text-gray-400 line-through">₪{shirt.originalPrice}</span>
          )}
        </div>
      </div>

      <button
        onClick={() => addToCart(shirt, selectedSize, selectedColor)}
        disabled={!shirt.inStock}
        className={`w-full py-3 rounded-xl font-bold transition-all ${
          shirt.inStock
            ? 'bg-gradient-to-r from-blue-600 to-purple-600 text-white hover:shadow-lg transform hover:scale-105'
            : 'bg-gray-300 text-gray-500 cursor-not-allowed'
        }`}
      >
        {shirt.inStock ? '🛒 הוסף לעגלה' : 'אזל מהמלאי'}
      </button>
    </div>
  </div>
);
```

};

return (
<div className=“min-h-screen bg-gradient-to-br from-blue-50 via-white to-purple-50” style={{ direction: ‘rtl’ }}>
{/* Header */}
<header className="bg-white/90 backdrop-blur-md shadow-lg sticky top-0 z-50">
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
<div className="flex items-center justify-between h-16">
<div className="flex items-center space-x-reverse space-x-4">
<button
onClick={() => setShowMobileMenu(!showMobileMenu)}
className=“md:hidden p-2 rounded-lg hover:bg-gray-100 transition-colors”
>
{showMobileMenu ? <X size={20} /> : <Menu size={20} />}
</button>
<div className="flex items-center space-x-reverse space-x-2">
<Shirt className="text-blue-600" size={28} />
<h1 className="text-2xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
חנות החולצות 👕
</h1>
</div>
</div>

```
        <div className="hidden md:flex items-center flex-1 max-w-md mx-8">
          <div className="relative w-full">
            <Search className="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={20} />
            <input
              type="text"
              placeholder="חיפוש חולצות..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="w-full pr-10 pl-4 py-2 border border-gray-300 rounded-full focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
            />
          </div>
        </div>

        <div className="flex items-center space-x-reverse space-x-4">
          <button
            onClick={() => setShowFilters(!showFilters)}
            className="hidden md:flex items-center space-x-reverse space-x-2 px-4 py-2 bg-gray-100 rounded-full hover:bg-gray-200 transition-colors"
          >
            <Filter size={18} />
            <span>סינון</span>
          </button>
          
          <button
            onClick={() => setShowCart(!showCart)}
            className="relative p-3 bg-gradient-to-r from-blue-600 to-purple-600 text-white rounded-full hover:shadow-lg transition-all transform hover:scale-105"
          >
            <ShoppingCart size={20} />
            {getTotalItems() > 0 && (
              <span className="absolute -top-2 -left-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center animate-pulse">
                {getTotalItems()}
              </span>
            )}
          </button>
        </div>
      </div>
    </div>

    {/* Mobile Search */}
    {showMobileMenu && (
      <div className="md:hidden bg-white border-t px-4 py-3">
        <div className="relative mb-3">
          <Search className="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={20} />
          <input
            type="text"
            placeholder="חיפוש חולצות..."
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
            className="w-full pr-10 pl-4 py-2 border border-gray-300 rounded-full focus:ring-2 focus:ring-blue-500 focus:border-transparent"
          />
        </div>
        <button
          onClick={() => setShowFilters(!showFilters)}
          className="flex items-center space-x-reverse space-x-2 px-4 py-2 bg-gray-100 rounded-full hover:bg-gray-200 transition-colors"
        >
          <Filter size={18} />
          <span>סינון</span>
        </button>
      </div>
    )}
  </header>

  {/* Categories */}
  <div className="bg-white shadow-sm">
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
      <div className="flex overflow-x-auto space-x-reverse space-x-4 pb-2">
        {categories.map(category => (
          <button
            key={category.id}
            onClick={() => setSelectedCategory(category.id)}
            className={`flex items-center space-x-reverse space-x-2 px-6 py-3 rounded-full whitespace-nowrap transition-all ${
              selectedCategory === category.id
                ? 'bg-gradient-to-r from-blue-600 to-purple-600 text-white shadow-lg'
                : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
            }`}
          >
            <span className="text-lg">{category.icon}</span>
            <span className="font-medium">{category.name}</span>
          </button>
        ))}
      </div>
    </div>
  </div>

  {/* Filters Panel */}
  {showFilters && (
    <div className="bg-white shadow-sm border-t">
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
        <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">גודל</label>
            <select
              value={selectedSize}
              onChange={(e) => setSelectedSize(e.target.value)}
              className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
            >
              <option value="all">כל הגדלים</option>
              {sizes.map(size => (
                <option key={size} value={size}>{size}</option>
              ))}
            </select>
          </div>
          
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">צבע</label>
            <select
              value={selectedColor}
              onChange={(e) => setSelectedColor(e.target.value)}
              className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
            >
              <option value="all">כל הצבעים</option>
              {colors.map(color => (
                <option key={color} value={color}>{color}</option>
              ))}
            </select>
          </div>
          
          <div className="md:col-span-2">
            <label className="block text-sm font-medium text-gray-700 mb-2">
              טווח מחירים: ₪{priceRange[0]} - ₪{priceRange[1]}
            </label>
            <div className="flex items-center space-x-reverse space-x-4">
              <input
                type="range"
                min="0"
                max="1000"
                value={priceRange[0]}
                onChange={(e) => setPriceRange([parseInt(e.target.value), priceRange[1]])}
                className="flex-1"
              />
              <input
                type="range"
                min="0"
                max="1000"
                value={priceRange[1]}
                onChange={(e) => setPriceRange([priceRange[0], parseInt(e.target.value)])}
                className="flex-1"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  )}

  {/* Main Content */}
  <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
    <div className="flex justify-between items-center mb-8">
      <h2 className="text-3xl font-bold text-gray-900">
        {selectedCategory === 'all' ? 'כל החולצות' : categories.find(c => c.id === selectedCategory)?.name}
      </h2>
      <span className="text-gray-600">
        {filteredShirts.length} מוצרים
      </span>
    </div>

    {/* Products Grid */}
    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
      {filteredShirts.map(shirt => (
        <ShirtCard key={shirt.id} shirt={shirt} />
      ))}
    </div>

    {filteredShirts.length === 0 && (
      <div className="text-center py-16">
        <Shirt size={64} className="mx-auto text-gray-300 mb-4" />
        <h3 className="text-xl font-medium text-gray-500 mb-2">לא נמצאו חולצות</h3>
        <p className="text-gray-400">נסה לשנות את הפילטרים או החיפוש</p>
      </div>
    )}
  </main>

  {/* Shopping Cart Sidebar */}
  {showCart && (
    <div className="fixed inset-0 z-50 overflow-hidden">
      <div className="absolute inset-0 bg-black/50" onClick={() => setShowCart(false)} />
      <div className="absolute left-0 top-0 h-full w-96 bg-white shadow-2xl transform transition-transform">
        <div className="flex items-center justify-between p-6 border-b">
          <h3 className="text-xl font-bold">עגלת קניות</h3>
          <button
            onClick={() => setShowCart(false)}
            className="p-2 hover:bg-gray-100 rounded-full transition-colors"
          >
            <X size={20} />
          </button>
        </div>

        <div className="flex-1 overflow-y-auto p-6">
          {cart.length === 0 ? (
            <div className="text-center py-8">
              <ShoppingCart size={48} className="mx-auto text-gray-300 mb-4" />
              <p className="text-gray-500">העגלה ריקה</p>
            </div>
          ) : (
            <div className="space-y-4">
              {cart.map(item => (
                <div key={item.cartId} className="flex items-center space-x-reverse space-x-4 p-4 bg-gray-50 rounded-lg">
                  <img src={item.image} alt={item.name} className="w-16 h-16 object-cover rounded-lg" />
                  <div className="flex-1">
                    <h4 className="font-medium text-sm">{item.name}</h4>
                    <p className="text-xs text-gray-600">{item.selectedSize} • {item.selectedColor}</p>
                    <p className="text-sm font-bold text-blue-600">₪{item.price}</p>
                  </div>
                  <div className="flex items-center space-x-reverse space-x-2">
                    <button
                      onClick={() => updateQuantity(item.cartId, -1)}
                      className="p-1 hover:bg-gray-200 rounded"
                    >
                      <Minus size={16} />
                    </button>
                    <span className="text-sm font-medium w-8 text-center">{item.quantity}</span>
                    <button
                      onClick={() => updateQuantity(item.cartId, 1)}
                      className="p-1 hover:bg-gray-200 rounded"
                    >
                      <Plus size={16} />
                    </button>
                    <button
                      onClick={() => removeFromCart(item.cartId)}
                      className="p-1 hover:bg-red-100 text-red-600 rounded"
                    >
                      <X size={16} />
                    </button>
                  </div>
                </div>
              ))}
            </div>
          )}
        </div>

        {cart.length > 0 && (
          <div className="border-t p-6">
            <div className="flex justify-between items-center mb-4">
              <span className="text-lg font-bold">סכום כולל:</span>
              <span className="text-2xl font-bold text-blue-600">₪{getTotalPrice()}</span>
            </div>
            <button className="w-full py-3 bg-gradient-to-r from-blue-600 to-purple-600 text-white rounded-xl font-bold hover:shadow-lg transition-all">
              🛒 עבור לתשלום
            </button>
          </div>
        )}
      </div>
    </div>
  )}
</div>
```

);
};

export default ShirtStore;
