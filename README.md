// mobile_app/lib/models/food_model.dart (Дополнение)

// ... (OrderStatus, RestaurantRules, Restaurant - остаются прежними) ...

class DishModifier {
  final String modifierId;
  final String name;
  final double priceDelta; // Изменение цены (может быть отрицательным)
  
  DishModifier({required this.modifierId, required this.name, this.priceDelta = 0.0});
}

class Dish {
  // ... (id, name, description, price, isAvailable - остаются прежними) ...
  final List<DishModifier> modifiers;
  
  Dish({
    // ... (предыдущие поля) ...
    this.modifiers = const [],
  });
}

class CartItem {
  final Dish dish;
  final int quantity;
  final List<DishModifier> selectedModifiers; // Список выбранных модификаторов
  
  CartItem({required this.dish, required this.quantity, this.selectedModifiers = const []});
  
  // Расчет общей стоимости с учетом модификаторов
  double get total {
    final modifierCost = selectedModifiers.fold(0.0, (sum, m) => sum + m.priceDelta);
    return (dish.price + modifierCost) * quantity;
  }
}

class DeliveryDetails {
  final double baseFee;
  final double surcharge; // Динамическая наценка за загруженность
  final double finalFee;
  final DateTime kitchenReadyTime; // Когда кухня обещает закончить
  final DateTime deliverySlotStart; // Начало окна доставки
  final DateTime deliverySlotEnd;   // Конец окна доставки

  DeliveryDetails({
    required this.baseFee, required this.surcharge, required this.finalFee, 
    required this.kitchenReadyTime, required this.deliverySlotStart, required this.deliverySlotEnd
  });
}

class Order {
  // ... (orderId, restaurantId, items, deliveryAddress, status - остаются прежними) ...
  final DeliveryDetails deliveryDetails; // Вся информация о доставке
  
  // Общая сумма заказа: (Сумма блюд) + FinalFee
  double get totalWithDelivery => totalAmount + deliveryDetails.finalFee;

  Order({
    // ... (предыдущие поля) ...
    required this.deliveryDetails,
  });
}

// Обновление Real-Time статуса
class DeliveryUpdate {
  // ... (orderId, status - остаются прежними) ...
  final double courierLatitude;
  final double courierLongitude;
  final Duration courierRouteRemaining; // Оставшееся время до клиента (динамическое)
  
  DeliveryUpdate({
    // ... (предыдущие поля) ...
    required this.courierLatitude, required this.courierLongitude, 
    required this.courierRouteRemaining
  });
}
