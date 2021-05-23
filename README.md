# ESP8266 Slack Status Updater
Based on [Slack Status Updater With ESP8266](https://www.instructables.com/Slack-Status-Updater-With-ESP8266/) by Becky Stern

![EasySlackStatus](doc/easyslackstatus.jpg)

# Hardware 
- Wemos D1 Mini
- 128x64 pixel SH1106 oled display
- Rotary encoder with push button

# Obtain a unique to you Slack-Token
In order for the status to be correctly sent to your account, you must first obtain a unique `Slack-Token`.

Original Insructions are here https://github.com/witnessmenow/arduino-slack-api#getting-auth-token-this-may-change

Ask @Shane.Powell for help getting a slack token.


# Initial Setup
1. Plug in USB Power.  A fresh unconfigured device will automatically start up in **Access Point** mode.
    * You can force it to start in AP config mode by holding down the rotary dial while plugging into USB  power.
2. Connect your phones (or laptops) wifi to the **EasySlackStatus** Access Point.
3. The default WiFi password is ```12345678```
4. Once connected, open a browser to any URL. It will be auto redirected to the web-config.  The default IP is `192.168.4.1`
    * `Thing Name` = Just the simple name of this device. No need to change.
    * `AP Password` = Serves 2 purposes.  It becomes the new WiFi password when connecting to this device.  This also becomes the Admin password to this config page.  You MUST set this value to something.
    * `WiFi SSID` = The WiFi SSID/Name of your home wifi to connect to.
    * `WiFi Password` = Your home wifi password.
    * `Slack Token` = Your personal slack-access token (see below). Must be rebooted after setting this.
    * `Status 1-15` = The list of default status selections.  You can modify these later on using your computer after connecting to your local WiFi.  See below for more info.
5. Disconnect from the devices WiFi, it should trigger the device to try and connect to your WiFi.
6. If it all worked, it should present the list of status to choose from.


# Send a permanent status
1. Rotate the dial until the status you want is hilighted.
2. Single Press the dial.
    * The top right corner should show a `sending...`
    * Followed by chaning to the status you sent, as well as your name showing in the top left.
    * ERROR simply means.. well. .something went wrong.  Try again.


# Send an Expring status
1. Rotate the dial until the desired status is hilighted.
2. Double-Press (quickly) the rotary dial to enter `Expire In` selection screen.
3. Rotate the dial to set the minutes to expire.  The default minutes is pulled from the `Default Expire In Minutes` field on the webconfig.
4. Press the rotary dial once to send the status.

# View WiFi Info
1. Long Press the rotary dial to switch back-and-forth between the main status selection screen, and the WiFi Info Screen.

# Configure your Status List 
1. Long Press the rotary dial to view the connected IP address.
2. Open your laptop webbrowser to the URL shown (eg. http://10.0.1.4/)
3. Set any of the 1-15 different status sections to what ever you want.
    * `Status` - Simply the text name of your status.  Max 10 chars.
    * `Icon` - MUST be wrapped in ':' .  eg.. `:coffee:`  NOT `coffee`. A list of emoji icons can be found at https://www.webfx.com/tools/emoji-cheat-sheet/
    * `Default Expire in Minutes` - The value that is pre-populdated when you double-press the rotary to send an expiry status. 0-999 are valid values.


# FAQ
* *What is the default admin password?*
    * 12345678

* *How do I force into Config Mode?*
    * Unplug the power
    * hold down the rotary dial
    * plug in the power
    * wait for splash screen to change to the AP config screen

* *How do I find the current IP address and WiFi settings?*
    * Long Press the rotary dial.
    * Should show the current WiFi status, ip, config URL and Time.
 
* I forgot my AP Admin Password
    * Boot into a forced Config Mode. The default AP password `12345678` will be used.

* How can I view the serial output of the Arduino?
    * Simply connect the USB to your laptop, and open a serial terminal with a baud rate of `115200`.

# Build from Sources
1. install PlatformIO Core CLI toolset 
    * https://platformio.org/install/cli
    * Easiest is just one of the following:
        * `pip install -U platformio`
        * `homebrew install platformio`
2. Clone this repo
    1. cd into the lib directory
    2. git clone `git@github.com:shaneapowell/IotWebConf.git`
        * check out the branch `git checkout -b origin/AddStateCallback`
    3. git clone `git@github.com:shaneapowell/arduino-slack-api.git`
        * check out the branch `git checkout -b origin/feature/AddGetCurrentStatusCall`
3. `pio lib update`
4. `pio run --target clean`
4. `pio run --target upload` 

## Alter Boot-Splash
1. Start with the above build from sources.
2. The boot image is hard-coded in the `Adafruit_I2C_SH1106.cpp` file.
3. `static uint8_t buffer[BUFFER_SIZE]` contains the image.
4. Obtain an image exactly 128x64 px in size.
5. User an online image converter like (https://lvgl.io/tools/imageconverter) to create a new default buffer array.

## Wipe Flash Completely to start fresh
`pio run --target erase`

# Flash new Firmearer
1. Download latest `firmware.bin` file from here.
2. Open web browser to config page.
3. Scroll to bottom to find "firmware" update link.
4. Verify firmare version `before` and `after` update.
5. Upload new firmware. There is currently no filesystem update.

# License
MIT license
 