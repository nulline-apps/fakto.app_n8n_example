# n8n wFirma MCP Integration Workflow

Przykładowy workflow n8n demonstrujący integrację z serwerem MCP wFirma poprzez protokół HTTP Streamable.

## 📋 Opis

Ten workflow pokazuje jak połączyć n8n z serwerem MCP wFirma, aby automatyzować operacje księgowe i zarządzać fakturami bezpośrednio z poziomu automatyzacji n8n.

### Co robi ten workflow:

✅ Inicjalizuje połączenie z MCP Server (protokół JSON-RPC 2.0)  
✅ Wywołuje narzędzie `get_invoices` z parametrami filtrowania  
✅ Przetwarza wyniki i zwraca liczbę pobranych faktur

## 🚀 Szybki Start

### Wymagania

- n8n (self-hosted lub cloud)
- Aktywne konto wFirma z dostępem API
- Klucz subskrypcji MCP wFirma z [fakto.app](https://fakto.app)

### Krok 1: Import Workflow

1. Otwórz n8n
2. Przejdź do **Workflows** → **Import from File**
3. Wybierz plik `n8n-http-streamable-mcp-workflow.json`

### Krok 2: Aktualizacja Endpoint URL

Zaktualizuj URL w węzłach HTTP Request:
```
https://fakto.app/wfirma/stream
```
## Krok 3: Konfiguracja Headers

Workflow używa protokołu MCP (Model Context Protocol) z następującymi headerami:

| Header | Wartość | Opis |
|--------|---------|------|
| `Content-Type` | `application/json` | Typ zawartości |
| `mcp-protocol-version` | `2025-11-05` | Wersja protokołu MCP |
| `x-wfirma-credentials` | JSON object | Dane uwierzytelniające |

### Format `x-wfirma-credentials`:
```json
{
  "accessKey":"2158c549....",
  "secretKey":"182a0dda2.....",
  "appKey":"408ead4b.....",
  "companyId":"25...9",
  "subscription_api_key":"ak_prod_SgtDfYTb...."
}
```

### Krok 4: Test

Kliknij **Execute Workflow** i sprawdź czy faktury są poprawnie pobierane.



## 🛠️ Dostępne Narzędzia MCP

Po połączeniu z serwerem MCP wFirma, masz dostęp do 50+ narzędzi:

### 📄 Faktury

- `get_invoices` - Pobieranie listy faktur z filtrowaniem
- `get_invoice` - Pobieranie pojedynczej faktury po ID
- `create_invoice` - Tworzenie nowej faktury
- `get_invoice_pdf` - Pobieranie PDF faktury

### 👥 Kontrahenci

- `get_contractors` - Lista kontrahentów
- `create_contractor` - Dodawanie kontrahenta
- `get_contractor_by_nip` - Wyszukiwanie po NIP

### 📦 Magazyn

- `get_stock` - Stan magazynowy
- `get_warehouses` - Lista magazynów

### 💰 Finanse

- `get_financial_summary` - Podsumowanie finansowe
- `get_expenses` - Lista wydatków
- `get_payments` - Płatności
- `predict_payments` - Prognoza płatności


## 🤖 Integracja z AI Agent

Możesz również użyć tego workflow z węzłem AI Agent (LangChain) do tworzenia inteligentnego asystenta księgowego:
```
┌─────────────┐     ┌───────────┐     ┌─────────────────┐
│   Webhook   │ ──▶ │  AI Agent │ ──▶ │ MCP wFirma Tool │
└─────────────┘     └───────────┘     └─────────────────┘
                          │
                          ▼
                   ┌─────────────┐
                   │   Response  │
                   └─────────────┘
```

### Przykładowe zapytania do AI Agent:

- "Pokaż mi 5 ostatnich nieopłaconych faktur"
- "Wygeneruj raport finansowy za styczeń 2024"
- "Znajdź kontrahenta z NIP 1234567890"
- "Ile wynosi suma faktur wystawionych w tym miesiącu?"

## 🔧 Rozwiązywanie Problemów

### ❌ Error: "Invalid credentials"

- Sprawdź format JSON w headerze `x-wfirma-credentials`
- Upewnij się, że `companyId` to liczba (nie string)

### ❌ Error: "Subscription API key required"

- Dodaj `subscriptionApiKey` do credentials
- Sprawdź czy klucz jest aktywny na [fakto.app](https://fakto.app)

### ❌ Error: "Timeout"

- Zwiększ timeout w **Settings** → **Execution** → **Timeout** (60000ms)

### ❌ Error: "Tool not found"

- Sprawdź czy URL kończy się na `/stream`
- Użyj poprawnego endpointu: `https://fakto.app/wfirma/stream`

## 📚 Dokumentacja

- 🌐 [fakto.app](https://fakto.app) - Portal z kluczami API
- 📧 Wsparcie: contact@elvai.app

## 📝 Licencja

MIT License - szczegóły w pliku [LICENSE](LICENSE)
