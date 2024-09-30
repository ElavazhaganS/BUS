#include <SPI.h>
#include <RFID.h>

RFID rfid(10, 9); // Adjust pins as necessary
int readCount = 0;

void setup() {
    Serial.begin(9600);
    rfid.init();
}

void loop() {
    if (rfid.isCard()) {
        if (rfid.readCardSerial()) {
            String busNumber = String(rfid.serNum[0]); // Assuming the first byte is your bus number
            Serial.println(busNumber);
            saveToCSV(busNumber);
            readCount++;
        }
    }
    rfid.halt();
}

void saveToCSV(String busNumber) {
    File dataFile = SD.open("bus_data.csv", FILE_WRITE);
    if (dataFile) {
        dataFile.println(busNumber);
        dataFile.close();
    }
}

