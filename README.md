// mobile_app/lib/models/payment_model.dart (Дополнение)

// ... (PaymentMethod, TransactionStatus, AuthStatus, CardDetails, TransactionResponse - остаются прежними) ...

enum TokenType { ephemeral, multiUse, recurring }
enum RiskLevel { low, medium, high, fraud }
enum AuthenticationRequirement { none, biometric, pin, threeDSecure } // SCA требования

// Обновленная модель токена
class PaymentToken {
  final String tokenId;
  final String maskedPan;
  final DateTime expiration;
  final String cardId;
  final TokenType type; // Новый тип

  PaymentToken({
    required this.tokenId, required this.maskedPan, required this.expiration, 
    required this.cardId, this.type = TokenType.ephemeral // По умолчанию одноразовый
  });
}

// Обновленная модель запроса транзакции
class TransactionRequest {
  final String merchantId;
  final double amount;
  final String currency;
  final String processingToken;
  final String userDeviceId;  // Идентификатор устройства для Risk Scoring
  final RiskLevel riskScore;  // Оценка риска, приложенная к запросу

  TransactionRequest({
    // ... (предыдущие поля) ...
    required this.userDeviceId, required this.riskScore
  });
}

// Модель для запроса 3DS/SCA
class SCARequest {
  final AuthenticationRequirement requirement;
  final String? challengeUrl; // URL для 3DS-экрана, если требуется

  SCARequest({required this.requirement, this.challengeUrl});
}
