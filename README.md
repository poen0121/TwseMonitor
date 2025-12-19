# Twse Monitor

![Android Kotlin](https://img.shields.io/badge/language-android_kotlin-purple)


**Twse Monitor** is an Android application for real-time monitoring of Taiwanese stock market data (TSE and OTC). It allows users to track stock prices, volume, and other market indicators, and set alerts for price thresholds or percentage changes.  

---

## Features

- **Real-time stock updates**: Fetches and displays live stock prices, open/close prices, trading volume, and price changes.  
- **Alert notifications**: Users can configure upper/lower price alerts or percentage changes. Notifications are shown when thresholds are met.  
- **Stock management**: Add, edit, and delete stocks from your watchlist.  
- **Persistent storage**: All stock data and user settings are stored locally using Room database.  
- **LiveData updates**: UI automatically reflects real-time changes using Android’s `LiveData`.  
- **Background updates with WorkManager**: Periodically checks stock data and triggers alerts even when the app is not in the foreground.  

---

## Architecture & Components

The app follows a clean architecture using Android Jetpack components:

### 1. Room Database
- Stores `StockEntity` objects representing stocks and their alert settings.  
- DAO layer handles CRUD operations, including real-time updates (`updateRealtime`) and alert configuration (`updateAlertSetting`).  

### 2. Repository
- `StockRepository` provides a single source of truth for local and remote data.  
- Handles data transformations from API responses (`StockMsg`) to database entities.  

### 3. ViewModel
- `StockViewModel` exposes `LiveData` for observing stock lists and individual stock updates.  
- Provides functions to insert, update, delete, and update alert settings.  

### 4. API Layer
- `StockApiModel` fetches stock information from the external API.  
- Supports single or multiple stock queries.  

### 5. UI
- `MainActivity` displays the stock list in a `RecyclerView`.  
- Dialogs for viewing or editing stocks use `LayoutViewStockBinding` and `LayoutEditStockBinding`.  
- SeekBars and EditTexts allow users to set alert thresholds.  

### 6. WorkManager
- `RealtimeServiceCheckerWorker` runs periodic checks (every 15 minutes) to update stock prices and alert notifications in the background.  
- Ensures data freshness even when the app is not in the foreground.  

### 7. Notifications
- Configured for Android 13+ (`POST_NOTIFICATIONS` permission).  
- Alerts users when stocks meet the configured thresholds.  

### 8. Utilities
- Helper functions for parsing stock API responses, formatting prices, calculating change percentages, and converting trading times to UTC.  


## License

This project is licensed under the GNU General Public License (GPL) v3.0. You can freely use, modify, and distribute the code, but any derivative works must also be licensed under the GPL, and the source code must be made available.

See the full GPL license text at https://www.gnu.org/licenses/gpl-3.0.html.

