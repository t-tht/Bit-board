# Implementattion of BLE HID Keyboard service in Mbed

This implementation requires the following library

```c++
#include "mbed.h"
#include "ble/BLE.h"
#include "KeyboardService.h"
#include "MicroBitPin.h"
#include "examples_common.h"
#include "MicroBit.h"
```

We must first construct a BLE and KeyboardService object, and also declare some of the pins we will use on the micro:bit as inputs and outputs

```c++
// Construct BLE and KeyboardService object
BLE ble;
KeyboardService *kbdServicePtr;

// pins on the micro:bit on the LED matrix
DigitalOut col9(P0_12, 0);
DigitalOut waiting_led(P0_13);
DigitalOut connected_led(P0_15);
 
// pins on the micro:bit for button A and B
InterruptIn button1(BUTTON_A);
InterruptIn button2(BUTTON_B);

// pins for touch switch and joystick
DigitalIn touch1(MICROBIT_PIN_P13);
AnalogIn x_axis(MICROBIT_PIN_P0);
AnalogIn y_axis(MICROBIT_PIN_P1);

```
Several functions are also defined here.
We first define callback functions in the event of a connection, disconnection and waiting for connection.
```c++
static void onDisconnect(const Gap::DisconnectionCallbackParams_t *params)
{
    HID_DEBUG("disconnected\r\n");
    connected_led = 0;
 
    ble.gap().startAdvertising(); // restart advertising
}
 
static void onConnect(const Gap::ConnectionCallbackParams_t *params)
{
    HID_DEBUG("connected\r\n");
    waiting_led = false;
}
 
static void waiting() {
    if (!kbdServicePtr->isConnected())
        waiting_led = !waiting_led;
    else
        connected_led = !connected_led;
}
 
```

```c++
void send_string(const char * c) {
    if (!kbdServicePtr)
        return;
 
    if (!kbdServicePtr->isConnected()) {
        HID_DEBUG("we haven't connected yet...");
    } else {
        int len = strlen(c);
        kbdServicePtr->printf(c);
        HID_DEBUG("sending %d chars\r\n", len);
    }
}

```
We define some functions for joystick and touch switch inputs
```c++
void JoystickControl(){
    float x,y;
 
    // threshold values for the joystick to convert analogue input to digital
    
    x = x_axis.read();
    if (x < 0.05f) send_left();
    if (x > 0.95f) send_right();
    
    y = y_axis.read();
    if (y < 0.05f) send_down();
    if (y > 0.95f) send_up();
    
    wait(0.05);
}
 
void touch_control(){
    if (touch1 == 1) send_space();
    wait(0.05);
}
```
### Within main
A ticker object is created to call waiting() in every certain amount of time, in this case, we will choose 1 second.
```c++
    Ticker heartbeat;
    
    // call function send_one and send_zero if button A and B is pressed on the rising edge
    button1.rise(send_one);
    button2.rise(send_zero);
 
    // call waiting every 1 second using the ticker object
    heartbeat.attach(waiting, 1);
```
The BLE object needs to be initialised after being constructed, the callback function onConnect and onDisconnect is also 'attached' in the event of a connect and disconnect sent from the GAP service.
```c++
// initialise ble object
    ble.init();
 
    // attach callback functions on the event of connection and disconnection
    ble.gap().onDisconnection(onDisconnect);
    ble.gap().onConnection(onConnect);
```
For the micro:bit to advertise itself as a BLE HID Keyboard, the ble object must contain the suitable security properties, device information and battery service information.
- [initializeSecurity(ble)](#inside-initializesecurity) adds security properties required 
- KeyboardService(ble) adds a keyboard service to the GATT table inside the BLE object (BLE_HID/KeyboardService.h)
- [initializeHOGP(ble)](#inside-initializehogp) adds more information to the BLE object, such as a pnp id and battery service information required for HOGP (HID Over Gatt Protocol)  
```c++
    initializeSecurity(ble);
 
    KeyboardService kbdService(ble);
    kbdServicePtr = &kbdService;

    initializeHOGP(ble);
```
GAP handles device discovery, link establishment, link termination and more. The GAP service is then used to advertise the micro:bit as a BLE HID Keyboard. 
```c++
// setting up gap service
    ble.gap().accumulateAdvertisingPayload(GapAdvertisingData::KEYBOARD);
    ble.gap().accumulateAdvertisingPayload(GapAdvertisingData::COMPLETE_LOCAL_NAME,
                                           (uint8_t *)DEVICE_NAME, sizeof(DEVICE_NAME));
    ble.gap().accumulateAdvertisingPayload(GapAdvertisingData::SHORTENED_LOCAL_NAME,
                                           (uint8_t *)SHORT_DEVICE_NAME, sizeof(SHORT_DEVICE_NAME));
    ble.gap().setDeviceName((const uint8_t *)DEVICE_NAME);
 
    // advertise device
    ble.gap().startAdvertising();
```
We then implement a polling rate of 125Hz for the keyboard. 125Hz is chosen instead of 1000Hz to conserve power.
```c++
while (true) {
        ble.waitForEvent();  
        JoystickControl();  
        touch_control();
        // poll every 8ms, or 125Hz
        wait_ms(8);
    }
```

### Inside initializeSecurity()
It is important to initialize the security properties for the BLE object, this is done to prevent the security issues during the pairing process, these security issues include:
- Passive eavesdropping
- MITM (man-in-the-middle) attacks 
- Identity tracking.
```c++
void initializeSecurity(BLE &ble)
{
    bool enableBonding = true;
    bool requireMITM = HID_SECURITY_REQUIRE_MITM;

    ble.securityManager().onSecuritySetupInitiated(securitySetupInitiatedCallback);
    ble.securityManager().onPasskeyDisplay(passkeyDisplayCallback);
    ble.securityManager().onSecuritySetupCompleted(securitySetupCompletedCallback);

    ble.securityManager().init(enableBonding, requireMITM, HID_SECURITY_IOCAPS);
}
```
### Inside initializeHOGP()
Device information and Battery service is required for the device to advertise itself to client device.
```c++
void initializeHOGP(BLE &ble)
{
    static const uint16_t uuid16_list[] =  {GattService::UUID_HUMAN_INTERFACE_DEVICE_SERVICE,
        GattService::UUID_DEVICE_INFORMATION_SERVICE,
        GattService::UUID_BATTERY_SERVICE};

    PnPID_t pnpID;
    pnpID.vendorID_source = 0x2; // from the USB Implementer's Forum
    pnpID.vendorID = 0x0D28; // NXP
    pnpID.productID = 0x0204; // CMSIS-DAP (well, it's a keyboard but oh well)
    pnpID.productVersion = 0x0100; // v1.0
    HIDDeviceInformationService deviceInfo(ble, "ARM", "m1", "abc", "def", "ghi", "jkl", &pnpID);

    BatteryService batteryInfo(ble, 80);

    ble.gap().accumulateAdvertisingPayload(GapAdvertisingData::BREDR_NOT_SUPPORTED |
            GapAdvertisingData::LE_GENERAL_DISCOVERABLE);
    ble.gap().accumulateAdvertisingPayload(GapAdvertisingData::COMPLETE_LIST_16BIT_SERVICE_IDS,
            (uint8_t *)uuid16_list, sizeof(uuid16_list));

    // see 5.1.2: HID over GATT Specification (pg. 25)
    ble.gap().setAdvertisingType(GapAdvertisingParams::ADV_CONNECTABLE_UNDIRECTED);
    // 30ms to 50ms is recommended (5.1.2)
    ble.gap().setAdvertisingInterval(50);
}
```

