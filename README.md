# ECE445_Lab_Notebook
## ECE 445 Timeline
**January 27th**
Today was the first project meeting where we shared project ideas. Finalized with Smart AC Unit. Will start idea proposal.

**January 28th**
We finished Idea Proposal.

**January 30th**
Met with professor regarding our Smart AC Unit design. Working on final proposal.

**January 31st**
With further discussions, we might add humidity functionality to the project. There will be more talks regarding if humidity is needed for an AC Unit. 

**February 6th**
Had first meeting with Douglas and was given good information for future adjustments of project.

**February 8th**
Met and finished the Project Proposal. 

**February 9th**
We met with the machine shop. Lots of concerns regarding the project. First issue is how will the machine shop be able to have access to our AC Unit to make the necessary equipment to turn the knobs. 

**February 11th**
The group started the github for all of our files. We discussed earlier that our app will maintain bluetooth functionality, so I have started research into how we shall implement bluetooth in our app. The app will be on Android Studios using java over kotlin.

**February 22nd**
Not much as been getting done in terms of actual progress for the project. Only been doing research for the app and how will we integrrate it with all of our other physical components. Also the group has started work on the Design Document and Project Proposal Review.

**February 29th**  
We have started looking for the necessary parts for our dev board/pcb. Therefore, we have started ordering our parts. I have also started looking at our app design

**March 19th**
The group has reordered some parts beacuse of some issues. App development beginning.

**March 20th**
The group has finished our Design Document Regrade

**March 25th**
We have begun the first steps of the project with working with our parts and the app.

**March 26th**
All of our parts for our first pcb design came in. Our first pcb prototype in the works. 

**March 27th**
Our initial PCB design looks completed and we know where things belong on our PCB. ESP32 and our sesnors also works.

**April 1st**
Lots of things went wrong today. We need drastic changes to our system. First of which our servo is not strong enough turn the knobs on the AC Unit, so a new, stronger servo motor is needed. Additionally, we decided that the original display we wanted isn't good enough becuase of the limited libraries that it comes with. ESP32 also damaged, so we need a new one.

**April 2nd**
I have been trying to find various ways to use bluetooth compatabilty that the esp32 has for our app. There are a lot of sources that utliize it in many different ways. Bluetooth functionality. Trying to learn via this stack overflow link: https://stackoverflow.com/questions/57105709/how-to-connect-android-app-to-esp32-via-bluetooth. ALso watched a couple YouTube vidoes. https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.youtube.com/watch%3Fv%3DaE8EbDmrUfQ&ved=2ahUKEwihl7Kbxu2FAxX04MkDHcLKANQQwqsBegQIDxAG&usg=AOvVaw0kwJC0zBNWrLpYWNhPz74E
PSEDOCODE: protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textViewStatus = (TextView) findViewById(R.id.textViewStatus);
        textViewDistance = (TextView) findViewById(R.id.textViewDistance);
        textViewPaired = (TextView) findViewById(R.id.textViewPaired);

        final BluetoothManager bluetoothManager =
                (BluetoothManager) getSystemService(Context.BLUETOOTH_SERVICE);
        bluetoothAdapter = bluetoothManager.getAdapter();

        if (bluetoothAdapter == null || !bluetoothAdapter.isEnabled()) {
            textViewStatus.setText("Bluetooth is not enabled, enabling bluetooth");
            Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
            startActivityForResult(enableBtIntent, REQUEST_ENABLE_BT);
        }

        textViewPaired.setText("Paired devices:");
        Set<BluetoothDevice> devices = bluetoothAdapter.getBondedDevices();
        for(BluetoothDevice device : devices){
            textViewPaired.append("\nDevice: " + device.getName() + ", " + device);
        }

        try{
            bluetoothDevice = bluetoothAdapter.getRemoteDevice(MAC);
            bluetoothSocket = bluetoothDevice.createInsecureRfcommSocketToServiceRecord(myUUID);
            bluetoothSocket.connect();
        } catch (IOException ex){
            Toast toast = Toast.makeText(this, ex.toString(), Toast.LENGTH_LONG);
            toast.show();
        }

**April 4th**
The group found a new servo motor that seems enough for our knobs to turn.

**April 14th**
PCB orders have come in. 

**April 16th**
Dev board completed and everything works perfectly with humidity and temperature displaying correct data as it was programmed to. The servo motor runs as well. It has the 270 degrees of rotation as specified on the data sheet. It is very powerful. I am still trying to implement bluetooth, but running into many issues that I am not been able to resolve using the resources I have been using.

**April 17th**
So far the app works on the built in emulator on Android Studios. For some reason, however, the emulator is not fully working on my mac. We have been using a pixel 2 as our emulator. So to test on my machine, I need Kevin's andoird device. So I was working on trying to connect my machine to Kevin's Android 14 by allowing for dev and wifi permissions on his phone, which worked. The GUI is a bit misaligned on the Andoird 11 as opposed to the Pixel 2 emulator.
 
**April 24th**
The group has done a fulle 180. We no longer want to continue with bluetooth functionality to connect to the ESP32. We have now decided on a new communication protocol, which is wifi. So I have begun research onto how we can get Wifi to work. I have been looking at sources online such as stack overflow. I have also realized that for some reason, trying to get the library android networking to work for wifi. But every solution from online is stil causing errors for me. Besides that Me and Kevin have been diligenlty working to get the pcb fully funcitonal. But we ran into some issues. Our inital pcb soldering was met with improper soldering of our switches. We did get wifi connectivity for a couple seconds before it fizzled out. Unfortunate events because our usb-c and esp32 were fully functional, so we need to restart. However, because we don't have the proper voltage regulator, the LM1117. We must wait until we get that part.
PSEDOCODE: 

1. void setup() {

  Serial.begin(115200);
  WiFi.mode(WIFI_AP);
  WiFi.softAP(ssid, password);
  Serial.print("IP address: ");
  Serial.println(WiFi.softAPIP());
  server.begin();      // Start the server
  pinMode(2, OUTPUT);  // Set onboard LED as an output
}

void loop() {
  WiFiClient client = server.available();  // Listen for incoming clients

  if (client) {                                  // If a client connects
    String data = client.readStringUntil('\n');  // Read the data from the client
    Serial.println(data);                        // Print the data to the serial monitor

    if (data == "Hello, ESP32!") {  // If the data is "Hello, ESP32!"
      // Flash the onboard LED twice
      for (int i = 0; i < 2; i++) {
        digitalWrite(2, HIGH);
        delay(200);
        digitalWrite(2, LOW);
        delay(200);
      }
    } else {
      // Flash the onboard LED once
      digitalWrite(2, HIGH);
      delay(200);
      digitalWrite(2, LOW);
      delay(200);
    }

    client.stop();  // Disconnect the client
  }
}

**April 25th**
We were able to get the voltage regulator and solder our parts onto the pcb using the paste and the oven. When we tested it the esp32, voltage regulator, usb-c, switches work properly, with wifi working. However, our display doesnt display any text, only lights up. We have discovered that our new NHD-C0220BIZ display is fried so we must come up with another way to display our humidity and temperature. But aside from that, we finished our PCB. We did realize that our sensors are too close to the esp32 causing improper temperature readings. Servo motor also work, and the app connects to the esp32 wifi and we are able to use the app to change the AC settings.

**April 26th**
We wanted a 3-D printed bracket to hold our motor but it broke, so we need some temporary fix to it. Everything runs smoothly.
