# BTLEUart: Uart connection with low energy bluetooth and Adafruit nRF8001 module (or similar)

I have created this library to make easier using low energy bluetooth as a UART connector.
This library allows you to connect to a BTLE module, as well as to send and receive data. 
The only requiments are an +18 API and a device with a built-in BTLE.

Feel free to commit any improvements and report if any problems occur :)

Sample code:

    public class YourActivity extends AppCompatActivity implements 
            BTLEUart.BTLEInit,
            BTLEUart.BTLEConnection,
            BTLEUart.BTLEData{

        private BTLEUart btleUart;

        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            //All listeners are optional
            btleUart = new BTLEUart(this)
                    .withBTLEConnectionListener(this)
                    .withBTLEInitListener(this)
                    .withBTLEDataListener(this)
                    .init();


            //To start connection use (only after successful initialization)
            btleUart.connectDevice("Your device bluetooth MAC address");

            //To stop connection use (only after successful connection)
            btleUart.disconnectDevice();

            //When device is connected you can use
            //(IMPORTANT - with nRF8001 max message length = 20)
            btleUart.writeMessage("Your message to send");

        }

        @Override
        public void onBTLEInitSuccess() {
            //Bluetooth initialization successful, now you can make new connection
        }

        @Override
        public void onBTLEInitFailed() {
            //Bluetooth fail - unable to use this library
        }

        @Override
        public void onConnected() {

        }

        @Override
        public void onDisconnected() {

        }

        @Override
        public void onDataAvailable(String received) {
            //Received data from connected device - library automatically converts it to String 
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            //VERY IMPORTANT
            if(btleUart != null)
                btleUart.onDestroy();
        }
    }
    
#Usage
The library is available by JitPack, so you can easily add it into Gradle dependencies.

root build.gradle

	allprojects {
		repositories {
			...
			maven { url "https://jitpack.io" }
		}
	}
        
app build.gradle

	dependencies {
		compile 'com.github.linean:btleuart:1.0.0'
	}
    
#Credentials
In this project I'm using the Service writted by Nordic Semiconductor.

      Copyright (c) 2015, Nordic Semiconductor
      All rights reserved.

      Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

     1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.

     2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

     3. Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this
     software without specific prior written permission.

     THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
     LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
     HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
     LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
     ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE
    USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

