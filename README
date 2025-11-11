IoT-Based Real-Time Weather Monitoring and Forecasting System
📋 Project Overview
An end-to-end IoT system for real-time weather data acquisition and short-term forecasting using machine learning. This system collects environmental data through custom-built sensors and provides accurate weather predictions for specific regions.

https://via.placeholder.com/800x400.png?text=Weather+Station+Architecture

🚀 Features
Real-time Data Collection: Temperature, humidity, pressure, wind speed/direction, rainfall, UV index

Cloud Integration: Automatic data upload to Google Cloud Storage

Machine Learning Forecasting: Short-term weather prediction using SVR, KNN, Decision Trees, and Ridge Regression

Custom Sensor Design: Cost-effective anemometer, wind vane, and rain gauge

Remote Monitoring: Global access to weather data via web interface

🛠️ Hardware Architecture
System Block Diagram
text
┌─────────────────────────────────────────────────┐
│              OUTDOOR SENSING UNIT               │
│  ┌─────────────┐    ┌──────────────────────┐    │
│  │   Arduino   │    │      Sensors:        │    │
│  │    Uno      │◄───┤  • DHT22 (Temp/Hum)  │    │
│  │             │    │  • BMP280 (Pressure) │    │
│  └─────────────┘    │  • GYML8511 (UV)     │    │
│         │           │  • Anemometer        │    │
│         │           │  • Wind Vane         │    │
│         ▼           │  • Rain Gauge        │    │
│  ┌─────────────┐    └──────────────────────┘    │
│  │   Serial    │                                │
│  │ Communication│                               │
│  └─────────────┘                                │
└─────────────────────────────────────────────────┘
         │
         │ 5m Cable
         │
┌─────────────────────────────────────────────────┐
│               INDOOR GROUND UNIT                │
│  ┌─────────────┐    ┌──────────────────────┐    │
│  │    ESP32    │    │   Connectivity:      │    │
│  │             │    │  • Wi-Fi             │    │
│  └─────────────┘    │  • Cloud Upload      │    │
│         │           └──────────────────────┘    │
│         ▼                                       │
│  ┌─────────────┐                                │
│  │   Google    │                                │
│  │ Cloud Storage│                               │
│  └─────────────┘                                │
└─────────────────────────────────────────────────┘
📋 Bill of Materials
Microcontrollers
Component	Quantity	Specifications	Purpose
Arduino Uno R3	1	ATmega328P, 16MHz	Outdoor sensor control
ESP32 Dev Board	1	Dual-core, Wi-Fi, Bluetooth	Indoor data processing
Environmental Sensors
Sensor	Model	Measurements	Interface
Temperature/Humidity	DHT22	-40°C to 80°C, 0-100% RH	Digital
Barometric Pressure	BMP280	300-1100 hPa	I2C
UV Light	GYML8511	280-390nm, 0-15 mW/cm²	Analog
Custom Weather Instruments
Anemometer (Wind Speed)
A3144 Hall Effect Sensor ×1

Neodymium Magnets ×3

608ZZ Bearings ×1

3D Printed Cups ×3 (75mm diameter)

Wind Vane (Direction)
A3144 Hall Effect Sensors ×8

Ring Magnet ×1

10kΩ Resistors ×8

685ZZ Bearings ×1

Rain Gauge
Magnetic Reed Switch ×1

Tipping Buckets ×2

Collection Funnel (150mm diameter)

Supporting Components
RTC DS1302 ×1 (Real-time clock with CR2032 battery)

LM7805 Voltage Regulator ×1

Waterproof Enclosures ×2 (IP65 rated)

4-conductor Shielded Cable ×5m

Various Resistors/Capacitors (see full BOM)

🔌 Circuit Schematics
Outdoor Unit (Arduino Uno)
text
Arduino Uno Pinout:
├── D2: Anemometer (Hall Sensor) - Interrupt 0
├── A0-A3: Wind Vane (Hall Sensor Array)
├── D4: Rain Gauge (Reed Switch)
├── D5: RTC DS1302 (CLK)
├── D6: RTC DS1302 (DAT)
├── D7: RTC DS1302 (RST)
├── A4: BMP280 (SDA)
├── A5: BMP280 (SCL)
├── D8: DHT22 (Data)
└── A0: GYML8511 (Analog UV)
Indoor Unit (ESP32)
text
ESP32 Pinout:
├── RX2 (GPIO16): Serial receive from Arduino
├── TX2 (GPIO17): Serial transmit to Arduino
├── 3.3V: Sensor power
├── GND: Common ground
└── Built-in Wi-Fi: Cloud connectivity
Power Supply Schematic
text
Outdoor Unit:
9V-12V DC ────┐
              │
           [LM7805]
              │
              ├── 5V ─── Arduino Vin
              └── 5V ─── Sensors
              
Indoor Unit:
USB 5V ────┐
           │
        [ESP32] ─── 3.3V ─── Level Shifter
           │
        Wi-Fi ──── Cloud
🎯 Sensor Specifications & Formulas
Wind Speed Calculation
cpp
// Anemometer formula
float calculateWindSpeed(int rotationCount, float timePeriod) {
    float circumference = 0.2; // meters (cup rotation)
    float rotationsPerSecond = rotationCount / timePeriod;
    float windSpeed_ms = circumference * rotationsPerSecond;
    float windSpeed_kmh = windSpeed_ms * 3.6;
    return windSpeed_kmh;
}
Rainfall Measurement
cpp
// Rain gauge calibration
#define BUCKET_TIP_MM 0.2  // Each tip = 0.2mm rainfall
float rainfall_mm = tipCount * BUCKET_TIP_MM;
UV Index Conversion
cpp
// GYML8511 analog to UV index
float readUVIndex() {
    int uvValue = analogRead(UV_SENSOR_PIN);
    float voltage = uvValue * (5.0 / 1023.0);
    float uvIntensity = voltage * 15.0; // mW/cm²
    return uvIntensity;
}
💻 Installation & Setup
1. Hardware Assembly
Outdoor Unit
Mount sensors on 50cm aluminum arms

Connect sensors to Arduino Uno following pinout

Install in waterproof enclosure with cable glands

Mount on 2-meter pole with U-bolts

Indoor Unit
Connect ESP32 to serial communication cable

Set up power supply with 5V 2A adapter

Configure Wi-Fi credentials in code

2. Software Installation
Arduino IDE Setup
bash
# Install required libraries
Tools -> Manage Libraries -> Install:
- DHT sensor library
- Adafruit BMP280 Library
- RTClib for DS1302
ESP32 Board Setup
bash
# Add ESP32 to Arduino IDE
File -> Preferences -> Additional Board URLs:
https://dl.espressif.com/dl/package_esp32_index.json

# Install ESP32 board package
Tools -> Board -> Boards Manager -> ESP32
3. Cloud Configuration
Google Cloud Storage
python
# Python script for data upload
from google.cloud import storage
import json

def upload_weather_data(data):
    client = storage.Client()
    bucket = client.bucket('weather-station-data')
    blob = bucket.blob(f"data/{data['timestamp']}.json")
    blob.upload_from_string(json.dumps(data))
📊 Machine Learning Setup
Data Collection
python
# Historical data from Visual Crossing API
import requests

def get_historical_data(start_date, end_date):
    api_key = "your_api_key"
    url = f"https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/Sylhet/{start_date}/{end_date}"
    response = requests.get(url, params={"key": api_key})
    return response.json()
Model Training
python
from sklearn.svm import SVR
from sklearn.model_selection import GridSearchCV
from sklearn.preprocessing import StandardScaler

# SVR Model with RBF kernel
svr_model = SVR(kernel='rbf')
param_grid = {
    'C': [0.1, 1, 10, 100],
    'gamma': [0.1, 0.01, 0.001],
    'epsilon': [0.1, 0.01, 0.001]
}
grid_search = GridSearchCV(svr_model, param_grid, cv=5, scoring='neg_mean_squared_error')
🔧 Configuration
Arduino Code Configuration
cpp
// config.h
#define WIFI_SSID "your_wifi_ssid"
#define WIFI_PASSWORD "your_wifi_password"
#define CLOUD_URL "https://your-cloud-endpoint.com/data"
#define SENSOR_READ_INTERVAL 300000  // 5 minutes

// Sensor calibration constants
#define ANEMOMETER_CALIBRATION 2.5
#define RAIN_GAUGE_BUCKET_SIZE 0.2
Python ML Configuration
python
# config.py
DATA_PATH = "weather_data.csv"
MODEL_SAVE_PATH = "trained_models/"
TEST_SIZE = 0.2
RANDOM_STATE = 42

# Features for training
FEATURES = [
    'temperature', 'humidity', 'pressure', 
    'wind_speed', 'wind_direction', 'uv_index'
]
TARGET = 'temperature_next_6h'
📈 Performance Metrics
Model Comparison Results
Model	MSE	RMSE	MAE	R² Score
SVR	0.770332	0.877686	0.649473	0.985424
Decision Tree	1.318675	1.148336	0.841667	0.975048
Ridge Regression	1.463547	1.209772	0.896166	0.972307
KNN	1.891079	1.375165	1.025283	0.964217
🚀 Usage
Real-time Monitoring
python
# Start data collection
python run_weather_station.py

# Monitor live data
python monitor_dashboard.py

# Train ML models
python train_models.py
Data Export
python
# Export data for analysis
from utils.data_export import export_weather_data
export_weather_data('2024-12-01', '2025-01-20', 'weather_data.csv')
🛠️ Maintenance
Regular Calibration
Weekly: Check rain gauge bucket alignment

Monthly: Verify anemometer bearing rotation

Quarterly: Calibrate pressure sensor against reference

Yearly: Full system calibration and sensor replacement

Troubleshooting
Issue	Possible Cause	Solution
No data upload	Wi-Fi connection lost	Check ESP32 connection
Sensor readings erratic	Power supply issues	Verify 5V output
Wind direction inaccurate	Magnet alignment	Recalibrate hall sensor array
🔮 Future Enhancements
Planned Features
Solar power integration

Mobile app dashboard

Advanced deep learning models (LSTM)

Weather alert system

Multi-station network support

Sensor Expansion
Air quality sensor (PM2.5, PM10)

Soil moisture sensors

Lightning detection

Solar radiation pyranometer

📚 References
Al-Obeidat, F., et al. (2020). "Consistently accurate forecasts of temperature within buildings from sensor data using ridge and lasso regression"

Anjali, T., et al. (2019). "Temperature prediction using machine learning approaches"

Parashar, A. (2019). "IoT based automated weather report generation and prediction using machine learning"

👥 Contributors
Ahmed Thawhid Sabit - Hardware Design & Implementation

Bishal Roy - Machine Learning & System Architecture

Syed Abu Safwan - Cloud Integration & Data Management

Rayhan Ahmed Opi - Sensor Calibration & Testing

Md. Shahid Iqbal - Project Supervision

📄 License
This project is licensed under the MIT License - see the LICENSE.md file for details.

🙏 Acknowledgments
Sylhet Engineering College for research support

Visual Crossing for historical weather data

Arduino and ESP32 communities for open-source support
