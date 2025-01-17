# Agri-Sense-and-Hydroponic-Space
#include <DHT.h> // Include the DHT library

// Define sensor pins
#define DHTPIN 2       // Pin connected to DHT11 sensor
#define DHTTYPE DHT11  // DHT 11 type
#define MOISTURE_PIN A0 // Analog pin connected to soil moisture sensor
#define LIGHT_PIN A1    // Analog pin connected to light sensor
#define RELAY_PIN 8     // Pin connected to relay module for water pump

// Initialize DHT sensor
DHT dht(DHTPIN, DHTTYPE);

// Threshold values
int moistureThreshold = 400; // Adjust based on soil sensor calibration (lower = dry)
int lightThreshold = 200;    // Adjust based on desired light intensity

void setup() {
  Serial.begin(9600); // Start serial communication
  dht.begin();        // Initialize the DHT sensor
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH); // Ensure pump is off initially
}

void loop() {
  // Read soil moisture
  int soilMoisture = analogRead(MOISTURE_PIN);
  
  // Read light intensity
  int lightIntensity = analogRead(LIGHT_PIN);
  
  // Read temperature and humidity
  float temp = dht.readTemperature();
  float humidity = dht.readHumidity();
  
  // Print readings to the Serial Monitor
  Serial.println("----- Sensor Readings -----");
  Serial.print("Soil Moisture: ");
  Serial.println(soilMoisture);
  Serial.print("Light Intensity: ");
  Serial.println(lightIntensity);
  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.println(" °C");
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");
  Serial.println("----------------------------");
  
  // Automatic irrigation control
  if (soilMoisture < moistureThreshold) {
    Serial.println("Soil is dry. Turning on water pump...");
    digitalWrite(RELAY_PIN, LOW); // Turn on pump (active LOW)
  } else {
    Serial.println("Soil moisture is sufficient. Turning off water pump...");
    digitalWrite(RELAY_PIN, HIGH); // Turn off pump
  }
  
  // Simulate further actions for light intensity if needed
  if (lightIntensity < lightThreshold) {
    Serial.println("Light intensity is low. Additional actions can be added here.");
  }
  
  // Add delay to avoid rapid sensor reads
  delay(2000);
}
