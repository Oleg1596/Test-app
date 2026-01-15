import React, { useState, useEffect } from 'react';
import { ShoppingCart, Heart, Search, Home, User, X, Plus, Minus } from 'lucide-react';

const FashionShopApp = () => {
  const [activeTab, setActiveTab] = useState('home');
  const [cart, setCart] = useState([]);
  const [favorites, setFavorites] = useState([]);
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedProduct, setSelectedProduct] = useState(null);
  const [selectedSize, setSelectedSize] = useState('');

  // Демо-товари
  const products = [
    {
      id: 1,
      name: 'Елегантна сукня',
      price: 1299,
      image: 'https://images.unsplash.com/photo-1595777457583-95e059d581b8?w=400',
      category: 'Сукні',
      sizes: ['XS', 'S', 'M', 'L', 'XL'],
      description: 'Вишукана вечірня сукня з високоякісної тканини'
    },
    {
      id: 2,
      name: 'Класичні джинси',
      price: 899,
      image: 'https://images.unsplash.com/photo-1542272454315-7f6fabf2b90c?w=400',
      category: 'Джинси',
      sizes: ['26', '27', '28', '29', '30'],
      description: 'Стильні джинси з еластичною тканиною'
    },
    {
      id: 3,
      name: 'Літня блуза',
      price: 599,
      image: 'https://images.unsplash.com/photo-1564257577-f18e4f3e2e95?w=400',
      category: 'Блузи',
      sizes: ['XS', 'S', 'M', 'L'],
      description: 'Легка блуза для літнього сезону'
    },
    {
      id: 4,
      name: 'Тренч coat',
      price: 1899,
      image: 'https://images.unsplash.com/photo-1539533113208-f6df8cc8b543?w=400',
      category: 'Верхній одяг',
      sizes: ['S', 'M', 'L', 'XL'],
      description: 'Класичний тренч на всі сезони'
    },
    {
      id: 5,
      name: 'Спортивний костюм',
      price: 1199,
      image: 'https://images.unsplash.com/photo-1556821840-3a63f95609a7?w=400',
      category: 'Спорт',
      sizes: ['XS', 'S', 'M', 'L', 'XL'],
      description: 'Комфортний костюм для активного відпочинку'
    },
    {
      id: 6,
      name: 'Коктейльна сукня',
      price: 1599,
      image: 'https://images.unsplash.com/photo-1566174053879-31528523f8ae?w=400',
      category: 'Сукні',
      sizes: ['XS', 'S', 'M', 'L'],
      description: 'Ефектна сукня для особливих випадків'
    }
  ];

  // Telegram WebApp API
  useEffect(() => {
    if (window.Telegram?.WebApp) {
      const tg = window.Telegram.WebApp;
      tg.ready();
      tg.expand();
      tg.MainButton.setText('Оформити замовлення');
      tg.MainButton.hide();
    }
  }, []);

  useEffect(() => {
    if (window.Telegram?.WebApp && cart.length > 0) {
      const tg = window.Telegram.WebApp;
      const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
      tg.MainButton.setText(`Замовити (${total} ₴)`);
      tg.MainButton.show();
      tg.MainButton.onClick(() => {
        const orderText = cart.map(item => 
          `${item.name} (${item.size}) x${item.quantity} = ${item.price * item.quantity}₴`
        ).join('\n');
        tg.sendData(JSON.stringify({
          cart,
          total,
          orderText
        }));
      });
    } else if (window.Telegram?.WebApp) {
      window.Telegram.WebApp.MainButton.hide();
    }
  }, [cart]);

  const addToCart = (product, size) => {
    if (!size) {
      alert('Оберіть розмір');
      return;
    }
    
    const existingItem = cart.find(item => item.id === product.id && item.size === size);
    
    if (existingItem) {
      setCart(cart.map(item =>
        item.id === product.id && item.size === size
          ? { ...item, quantity: item.quantity + 1 }
          : item
      ));
    } else {
      setCart([...cart, { ...product, size, quantity: 1 }]);
    }
    
    setSelectedProduct(null);
    setSelectedSize('');
  };

  const updateQuantity = (id, size, delta) => {
    setCart(cart.map(item => {
      if (item.id === id && item.size === size) {
        const newQuantity = item.quantity + delta;
        return newQuantity > 0 ? { ...item, quantity: newQuantity } : item;
      }
      return item;
    }).filter(item => item.quantity > 0));
  };

  const removeFromCart = (id, size) => {
    setCart(cart.filter(item => !(item.id === id && item.size === size)));
  };

  const toggleFavorite = (productId) => {
    setFavorites(prev =>
      prev.includes(productId)
        ? prev.filter(id => id !== productId)
        : [...prev, productId]
    );
  };

  const filteredProducts = products.filter(p =>
    p.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
    p.category.toLowerCase().includes(searchQuery.toLowerCase())
  );

  const ProductCard = ({ product }) => (
    <div className="bg-white rounded-lg shadow-md overflow-hidden">
      <div className="relative">
        <img
          src={product.image}
          alt={product.name}
          className="w-full h-48 object-cover"
        />
        <button
          onClick={() => toggleFavorite(product.id)}
          className="absolute top-2 right-2 bg-white rounded-full p-2 shadow-lg"
        >
          <Heart
            className={`w-5 h-5 ${
              favorites.includes(product.id) ? 'fill-red-500 text-red-500' : 'text-gray-400'
            }`}
          />
        </button>
      </div>
      <div className="p-4">
        <div className="text-xs text-gray-500 mb-1">{product.category}</div>
        <h3 className="font-semibold text-lg mb-2">{product.name}</h3>
        <div className="flex justify-between items-center">
          <span className="text-xl font-bold text-pink-600">{product.price} ₴</span>
          <button
            onClick={() => setSelectedProduct(product)}
            className="bg-pink-500 text-white px-4 py-2 rounded-lg hover:bg-pink-600 transition"
          >
            Купити
          </button>
        </div>
      </div>
    </div>
  );

  const HomeTab = () => (
    <div className="pb-20">
      <div className="bg-gradient-to-r from-pink-500 to-purple-500 text-white p-6 rounded-lg mb-6">
        <h1 className="text-2xl font-bold mb-2">Новинки сезону!</h1>
        <p>Знижки до 30% на вибрані моделі</p>
      </div>

      <div className="mb-4">
        <div className="relative">
          <Search className="absolute left-3 top-3 text-gray-400 w-5 h-5" />
          <input
            type="text"
            placeholder="Пошук товарів..."
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-pink-500"
          />
        </div>
      </div>

      <div className="grid grid-cols-2 gap-4">
        {filteredProducts.map(product => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  );

  const CartTab = () => {
    const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);

    return (
      <div className="pb-20">
        <h2 className="text-2xl font-bold mb-6">Кошик</h2>
        
        {cart.length === 0 ? (
          <div className="text-center py-20">
            <ShoppingCart className="w-16 h-16 text-gray-300 mx-auto mb-4" />
            <p className="text-gray-500">Ваш кошик порожній</p>
          </div>
        ) : (
          <>
            <div className="space-y-4 mb-6">
              {cart.map(item => (
                <div key={`${item.id}-${item.size}`} className="bg-white rounded-lg shadow p-4 flex gap-4">
                  <img
                    src={item.image}
                    alt={item.name}
                    className="w-20 h-20 object-cover rounded"
                  />
                  <div className="flex-1">
                    <h3 className="font-semibold">{item.name}</h3>
                    <p className="text-sm text-gray-500">Розмір: {item.size}</p>
                    <p className="text-pink-600 font-bold">{item.price} ₴</p>
                    
                    <div className="flex items-center gap-3 mt-2">
                      <button
                        onClick={() => updateQuantity(item.id, item.size, -1)}
                        className="bg-gray-200 rounded-full p-1 hover:bg-gray-300"
                      >
                        <Minus className="w-4 h-4" />
                      </button>
                      <span className="font-semibold">{item.quantity}</span>
                      <button
                        onClick={() => updateQuantity(item.id, item.size, 1)}
                        className="bg-gray-200 rounded-full p-1 hover:bg-gray-300"
                      >
                        <Plus className="w-4 h-4" />
                      </button>
                      <button
                        onClick={() => removeFromCart(item.id, item.size)}
                        className="ml-auto text-red-500 hover:text-red-700"
                      >
                        <X className="w-5 h-5" />
                      </button>
                    </div>
                  </div>
                </div>
              ))}
            </div>

            <div className="bg-pink-50 rounded-lg p-4">
              <div className="flex justify-between items-center mb-2">
                <span className="text-gray-600">Товарів:</span>
                <span>{cart.reduce((sum, item) => sum + item.quantity, 0)} шт</span>
              </div>
              <div className="flex justify-between items-center text-xl font-bold">
                <span>Разом:</span>
                <span className="text-pink-600">{total} ₴</span>
              </div>
            </div>
          </>
        )}
      </div>
    );
  };

  const FavoritesTab = () => {
    const favoriteProducts = products.filter(p => favorites.includes(p.id));

    return (
      <div className="pb-20">
        <h2 className="text-2xl font-bold mb-6">Обране</h2>
        
        {favoriteProducts.length === 0 ? (
          <div className="text-center py-20">
            <Heart className="w-16 h-16 text-gray-300 mx-auto mb-4" />
            <p className="text-gray-500">Список обраного порожній</p>
          </div>
        ) : (
          <div className="grid grid-cols-2 gap-4">
            {favoriteProducts.map(product => (
              <ProductCard key={product.id} product={product} />
            ))}
          </div>
        )}
      </div>
    );
  };

  const ProfileTab = () => (
    <div className="pb-20">
      <h2 className="text-2xl font-bold mb-6">Профіль</h2>
      
      <div className="bg-white rounded-lg shadow p-6 mb-4">
        <div className="flex items-center gap-4 mb-6">
          <div className="bg-pink-100 rounded-full p-4">
            <User className="w-12 h-12 text-pink-500" />
          </div>
          <div>
            <h3 className="font-bold text-lg">
              {window.Telegram?.WebApp?.initDataUnsafe?.user?.first_name || 'Користувач'}
            </h3>
            <p className="text-gray-500 text-sm">Telegram User</p>
          </div>
        </div>

        <div className="space-y-4">
          <div className="border-t pt-4">
            <p className="text-sm text-gray-500 mb-1">Замовлень</p>
            <p className="font-semibold">0</p>
          </div>
          <div className="border-t pt-4">
            <p className="text-sm text-gray-500 mb-1">Обране</p>
            <p className="font-semibold">{favorites.length}</p>
          </div>
          <div className="border-t pt-4">
            <p className="text-sm text-gray-500 mb-1">В кошику</p>
            <p className="font-semibold">{cart.length}</p>
          </div>
        </div>
      </div>

      <div className="bg-pink-50 rounded-lg p-4 text-center">
        <p className="text-sm text-gray-600">
          Підпишіться на наш канал, щоб не пропустити новинки та акції!
        </p>
      </div>
    </div>
  );

  return (
    <div className="max-w-2xl mx-auto bg-gray-50 min-h-screen">
      <div className="p-4">
        {activeTab === 'home' && <HomeTab />}
        {activeTab === 'cart' && <CartTab />}
        {activeTab === 'favorites' && <FavoritesTab />}
        {activeTab === 'profile' && <ProfileTab />}
      </div>

      {/* Модальне вікно вибору розміру */}
      {selectedProduct && (
        <div className="fixed inset-0 bg-black bg-opacity-50 flex items-end z-50">
          <div className="bg-white w-full rounded-t-3xl p-6 animate-slide-up">
            <div className="flex justify-between items-start mb-4">
              <div>
                <h3 className="text-xl font-bold">{selectedProduct.name}</h3>
                <p className="text-gray-600 text-sm mt-1">{selectedProduct.description}</p>
              </div>
              <button onClick={() => setSelectedProduct(null)}>
                <X className="w-6 h-6" />
              </button>
            </div>

            <div className="mb-6">
              <img
                src={selectedProduct.image}
                alt={selectedProduct.name}
                className="w-full h-64 object-cover rounded-lg"
              />
            </div>

            <div className="mb-6">
              <p className="font-semibold mb-3">Оберіть розмір:</p>
              <div className="flex gap-2 flex-wrap">
                {selectedProduct.sizes.map(size => (
                  <button
                    key={size}
                    onClick={() => setSelectedSize(size)}
                    className={`px-4 py-2 rounded-lg border-2 transition ${
                      selectedSize === size
                        ? 'border-pink-500 bg-pink-50 text-pink-600'
                        : 'border-gray-300 hover:border-pink-300'
                    }`}
                  >
                    {size}
                  </button>
                ))}
              </div>
            </div>

            <button
              onClick={() => addToCart(selectedProduct, selectedSize)}
              className="w-full bg-pink-500 text-white py-4 rounded-lg font-semibold hover:bg-pink-600 transition"
            >
              Додати в кошик • {selectedProduct.price} ₴
            </button>
          </div>
        </div>
      )}

      {/* Нижня навігація */}
      <div className="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 max-w-2xl mx-auto">
        <div className="flex justify-around py-3">
          <button
            onClick={() => setActiveTab('home')}
            className={`flex flex-col items-center gap-1 ${
              activeTab === 'home' ? 'text-pink-500' : 'text-gray-400'
            }`}
          >
            <Home className="w-6 h-6" />
            <span className="text-xs">Головна</span>
          </button>
          
          <button
            onClick={() => setActiveTab('favorites')}
            className={`flex flex-col items-center gap-1 relative ${
              activeTab === 'favorites' ? 'text-pink-500' : 'text-gray-400'
            }`}
          >
            <Heart className="w-6 h-6" />
            <span className="text-xs">Обране</span>
            {favorites.length > 0 && (
              <span className="absolute -top-1 right-2 bg-pink-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                {favorites.length}
              </span>
            )}
          </button>
          
          <button
            onClick={() => setActiveTab('cart')}
            className={`flex flex-col items-center gap-1 relative ${
              activeTab === 'cart' ? 'text-pink-500' : 'text-gray-400'
            }`}
          >
            <ShoppingCart className="w-6 h-6" />
            <span className="text-xs">Кошик</span>
            {cart.length > 0 && (
              <span className="absolute -top-1 right-2 bg-pink-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
                {cart.length}
              </span>
            )}
          </button>
          
          <button
            onClick={() => setActiveTab('profile')}
            className={`flex flex-col items-center gap-1 ${
              activeTab === 'profile' ? 'text-pink-500' : 'text-gray-400'
            }`}
          >
            <User className="w-6 h-6" />
            <span className="text-xs">Профіль</span>
          </button>
        </div>
      </div>
    </div>
  );
};

export default FashionShopApp;
