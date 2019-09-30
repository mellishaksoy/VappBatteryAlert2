.NET Sensor Event Handler
```csharp
using Platform360.Devices.SDK.Client;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Service Call
            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var _deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            // Device Service Connection To Platform
            // Here the related resources and properties are loaded to device service objects
            await _deviceService.ConnectToPlatformAsync();
			
			// notification event when new sensor value is added.
            _deviceService.UseSensorValueChangeRequestHandler(async e =>
            {
                Console.WriteLine(e.SensorId);
                Console.WriteLine(e.Value);
            });

            // Sensor Creation
            // Parameters: Sensor Name and isBidirectional
            var sensorResult = await _deviceService.CreateSensorAsync("Sensor Name", true);
            Console.WriteLine("Sensor Id: " + sensorResult.SensorId + "Sensor Name: " + sensorResult.Name);
            
            // Add Sensor Data as raw value and unit
            await _deviceService.PushSensorDataToPlatformAsync(sensorResult.SensorId, "36.5 celcius degree", "celcius", "36.5");


            Console.ReadLine();
        }
    }
}
```

-------
REBOOT HANDLER

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;
using Platform360.Devices.SDK.Client;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

			// notification event when new reboot update request is processed
            deviceManagementService.UseDeviceManagementRebootFunctionHandler(async e =>
            {
                Console.WriteLine("Reboot update request is handled.");
                Console.WriteLine("Reboot = " + e.Reboot);
                Console.WriteLine("IsFactoryReset = " + e.IsFactoryReset);
            });
            // Reboot Update Request
            var updateRebootResult = await deviceManagementService.UpdateRebootAsync(new DeviceRebootUpdateData
            {
                DeviceRebootId = "DeviceRebootId",
                Reboot = true
            });
            Console.WriteLine(updateRebootResult.DeviceRebootId);
        }
    }
}
```

--------------
FIRMWARE HANDLER

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;
using Platform360.Devices.SDK.Client;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // notification event when new firmware update request is processed
            deviceManagementService.UseDeviceManagementFirmwareFunctionHandler(async e =>
            {
                Console.WriteLine("Firmware update request is handled.");
                Console.WriteLine("Update = " + e.Update);
            });

            // Firmware Update Request
            var updateFirmwareResult = await deviceManagementService.UpdateFirmwareAsync(new DeviceFirmwareUpdateData
            {
                Version = "Update Version",
                Update = true
            });
            Console.WriteLine(updateFirmwareResult.Version);
        }
    }
}
```

-------------
NODE.JS SDK
------------

----------------
Sensor Event Handler
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

// Mqtt Client Options
var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

// Create Sensor Service with Mqtt Options 
var sensorService = new SensorService(options);

(async () => {

  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();
  
  // notification event when new sensor value is added.
  sensorService.setSensorValueChangeRequestNotificationEventHandler(async(e) => {
    console.log(e.SensorId);
    console.log("pushed new data to sensor id = " + e.SensorId);
  });
  
  // create bidirectional sensor
  var bidirectionalSensor = await sensorService.createSensor("SensorName", true);
  console.log(bidirectionalSensor);

  await sensorService.pushSensorFormattedDataToPlatform(bidirectionalSensor.SensorId, "36.5 celcius degree", "celcius", "36.5");


})();
```

------------
REBOOT NOTIFICATION EVENT HANDLER

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceRebootUpdateData } from './devicemanagementservice/internal/device-reboot-update-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // notification event when new reboot update request is processed
  deviceManagementService.setRebootRequestNotificationEventHandler(async(e) => {
    console.log("updated reboot id = " + e.DeviceRebootId);
  });

  // set data to update Reboot
  var rebootUpdateData = new DeviceRebootUpdateData();
  rebootUpdateData.DeviceRebootId = "DeviceRebootId";
  rebootUpdateData.IsFactoryReset = true;

  // update Reboot request 
  var reboot = await deviceManagementService.updateReboot(rebootUpdateData);

  console.log(reboot);
})();,
```

--------------
FIRMWARE HANDLER

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceFirmwareUpdateData } from './devicemanagementservice/internal/device-firmware-update-data';
var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // notification event when new firmware update request is processed
  deviceManagementService.setFirmwareRequestNotificationEventHandler(async(e) => {
    console.log("updated firmware id = " + e.DeviceFirmwareId);
  });

  // set data to update Firmware
  var firmwareUpdateData = new DeviceFirmwareUpdateData();
  firmwareUpdateData.DeviceFirmwareId = "DeviceFirmwareId";
  firmwareUpdateData.Version = "Version2";
  firmwareUpdateData.Update = false;

  // update Firmware request 
  var firmware = await deviceManagementService.updateFirmware(firmwareUpdateData);

  console.log(firmware);
})();

```

-------------------------

2.	Device SDK for Node.js Supported Methods
2.1.	Device Registration Method
Request



```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { RegistrationService } from './registrationservice/registration-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

var registrationService = new RegistrationService(options);

(async () => {

let result = await registrationService.register('deviceName');
console.log(result);
})();

```

```js
export class DeviceRegistrationResult {
  public DeviceId: string;
  public DeviceName: string;
  public PoA: string[];
}

```

2.2.	Connect to Platform Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  var deviceManagementService = new DeviceManagementService(options);
  
  // Create Sensor Service with Mqtt Options 
var sensorService = new SensorService(options);

(async () => {

  // Hangi servisin fonksiyonları kullanılacaksa öncesinde o servisin platforma connect olması bekleniyor.
  // Bu connection işleminde gerekli resource ve propertiler servis objelerine yüklenmektedir.

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();
  
  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();
})();


```

2.3.	Sensor Related Methods
2.3.1.	Sensor Creation Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

// Mqtt Client Options
var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

// Create Sensor Service with Mqtt Options 
var sensorService = new SensorService(options);

(async () => {

  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();
  
  // create bidirectional sensor
  var bidirectionalSensor = await sensorService.createSensor("SensorName", true);
  console.log(bidirectionalSensor);
})();


```


Response

```js
export class SensorCreationResult {
  public SensorId: string;
  public Name: string;
}

```



---------------

2.3.2.	Sensor Deletion Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Sensor Service with Mqtt Options 
  var sensorService = new SensorService(options);

(async () => {

  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();

  // Delete Sensor
  let deletedSensor = await sensorService.deleteSensor("sensorId");
  console.log(deletedSensor);
  
})();


```

Response

```js
export class SensorDeletionResult {
  public SensorId: string;
  public Name: string;
}

```


2.3.3.	Push Sensor Data to Platform Method
Request 1
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Sensor Service with Mqtt Options 
  var sensorService = new SensorService(options);

(async () => {

  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();

  // Create sensor
  var sensor = await sensorService.createSensor("SensorName", false);
  console.log(sensor);

  // Push data to sensor
  await sensorService.pushSensorDataToPlatform(sensor.SensorId,"SensorValue");

})();


```

Request 2
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { SensorService } from './sensorservice/sensor-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Sensor Service with Mqtt Options 
  var sensorService = new SensorService(options);

(async () => {

  // Sensor Service Connection to Platform
  // Here Related resources and properties are loaded to sensorService objects
  await sensorService.connectToPlatformAsync();

  // Get All Sensors
  let sensors = await sensorService.getSensors();

  if(!sensors && sensors.length > 0){
    // push data first sensor on sensor list with formatted data as unit, extracted data and raw data
    await sensorService.pushSensorFormattedDataToPlatform(sensors[0].Id, "raw data", "data unit(ex: Celsius)","extracted value (ex: 25)");
  }
})();


```


-----------
--------------
2.4.	Battery Related Methods
2.4.1.	Create Battery Method
Request

```js

import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceBatteryCreateData } from './devicemanagementservice/internal/device-battery-create-data';
import { BatteryStatus } from './onem2m/resources/battery-status';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // data settings belongs to creating battery
  var deviceBatteryCreateData = new DeviceBatteryCreateData;
  deviceBatteryCreateData.Name = "BatteryName";
  deviceBatteryCreateData.BatteryStatus = BatteryStatus.Charging;
  deviceBatteryCreateData.BatteryLevel = 2;
  
  // Create battery request
  var battery = await deviceManagementService.createBattery(deviceBatteryCreateData);
  console.log(battery);
  
})();
```


Response

```js
import { BatteryStatus } from '../../onem2m/resources/battery-status';

export class DeviceBatteryResult {
  BatteryId: string;
  Name: string;
  BatteryLevel?: number;
  BatteryStatus?: BatteryStatus;
}

```

2.4.2.	Retrieve Battery Method

Request

```js

import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // retrieve battery request belongs to given battery id 
  var battery = await deviceManagementService.retrieveBattery("batteryId");

  console.log(battery);
})();
```



2.4.3.	Update Battery Method
Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceBatteryUpdateData } from './devicemanagementservice/internal/device-battery-update-data';
import { BatteryStatus } from './onem2m/resources/battery-status';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  var updateBatteryData = new DeviceBatteryUpdateData();
  updateBatteryData.BatteryId = "batteryId";
  updateBatteryData.BatteryLevel = 100;
  updateBatteryData.BatteryStatus = BatteryStatus.ChargingComplete;

  // update battery request belongs to given battery id with new data
  var battery = await deviceManagementService.updateBattery(updateBatteryData);

  console.log(battery);
})();

```



2.4.4.	Delete Battery Method
Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // delete battery request belongs to given battery id 
  var battery = await deviceManagementService.deleteBattery("batteryId");

  console.log(battery);
})();

```


2.5.	Memory Related Methods
2.5.1.	Create Memory Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceMemoryCreateData } from './devicemanagementservice/internal/device-memory-create-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set update memory data 
  var memoryCreateData = new DeviceMemoryCreateData();
  memoryCreateData.Name = "Memory Name";
  memoryCreateData.TotalMemory =  8589934592 ; // bytes - 1 GB
  memoryCreateData.AvailableMemory = 4194304000; // bytes - 500 MB

  // create memory request 
  var memory = await deviceManagementService.createMemory(memoryCreateData);

  console.log(memory);
})();
```


Response

```js
export class DeviceMemoryResult {
  MemoryId: string;
  Name: string;
  TotalMemory?: number;
  AvailableMemory?: number;
}
```


2.5.2.	Retrieve Memory Method

Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();


  // retrieve memory request 
  var memory = await deviceManagementService.retrieveMemory("memoryId");

  console.log(memory);
})();
```



2.5.3.	Update Memory Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceMemoryUpdateData } from './devicemanagementservice/internal/device-memory-update-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set update memory data 
  var memoryUpdateData = new DeviceMemoryUpdateData();
  memoryUpdateData.MemoryId = "memoryId";
  memoryUpdateData.AvailableMemory = 4194304000; // bytes - 500 MB

  // update memory request belongs to given memory id with new data
  var memory = await deviceManagementService.updateMemory(memoryUpdateData);

  console.log(memory);
})();
```



2.5.4.	Delete Memory Method

Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();


  // retrieve memory request 
  var memory = await deviceManagementService.deleteMemory("memoryId");

  console.log(memory);
})();
```


Response

```js

```


2.6.	Device Info Related Methods
2.6.1.	Create Device Info Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DevicePhysicalDeviceCreateData } from './devicemanagementservice/internal/device-physical-device-create-data'

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to create physical device with mandatory fields
  var physicalDeviceCreateData = new DevicePhysicalDeviceCreateData();
  physicalDeviceCreateData.Name = "Physical Device Name";
  physicalDeviceCreateData.DeviceLabel = "Device Label";
  physicalDeviceCreateData.Manufacturer = "Manufacturer";
  physicalDeviceCreateData.Model = "Model";
  physicalDeviceCreateData.DeviceType = "Device Type";


  // create physical device info request 
  var physicalDeviceInfo = await deviceManagementService.createPhysicalDeviceInfo(physicalDeviceCreateData);

  console.log(physicalDeviceInfo);
})();
```

Response

```js
export class DevicePhysicalDeviceInfoResult {
  PhysicalDeviceInfoId: string;
  Name: string;
  DeviceLabel: string;
  Manufacturer: string;
  Model: string;
  DeviceType: string;
  FirmwareVersion: string;
  SoftwareVersion: string;
  HardwareVersion: string;
  ManufacturerDetailsLink: string;
  ManufacturingDate: string;
  SubModel: string;
  DeviceName: string;
  OsVersion: string;
  Country: string;
  Location: string;
  SystemTime: string;
  SupportURL: string[];
  PresentationURL: string[];
  Protocol: string[];
}
```

2.6.2.	Retrieve Device Info Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // retrieve physical device info request 
  var physicalDeviceInfo = await deviceManagementService.retrievePhysicalDeviceInfo("physicalDeviceInfoId");

  console.log(physicalDeviceInfo);
})();
```

2.6.3.	Update Device Info Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DevicePhysicalDeviceUpdateData } from './devicemanagementservice/internal/device-physical-device-update-data'

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to update physical device 
  var physicalDeviceUpdateData = new DevicePhysicalDeviceUpdateData();
  physicalDeviceUpdateData.PhysicalDeviceInfoId = "PhysicalDeviceInfoId";
  physicalDeviceUpdateData.OsVersion  = "OsVersion";


  // update physical device info request 
  var physicalDeviceInfo = await deviceManagementService.updatePhysicalDeviceInfo(physicalDeviceUpdateData);

  console.log(physicalDeviceInfo);
})();
```

2.6.4.	Delete Device Info Method

Request

```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();


  // delete physical device info request 
  var physicalDeviceInfo = await deviceManagementService.deletePhysicalDeviceInfo("physicalDeviceInfoId");

  console.log(physicalDeviceInfo);
})();
```

2.7.	Area Network Info Related Methods
2.7.1.	Create Area Network Info Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { AreaNetworkInfoCreateData } from './devicemanagementservice/internal/area-network-info-create-data'

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to create Area Network Info
  var areaNetworkInfoCreate = new AreaNetworkInfoCreateData();
  areaNetworkInfoCreate.Name = "Area Network Info Name";
  areaNetworkInfoCreate.AreaNetworkType = "Area Network Type";
  areaNetworkInfoCreate.ListOfDevices = ["CreatedBeforeAreaNetworkDeviceInfoResourceId"];


  // create AreaNetworkInfo request 
  var areaNetworkInfo = await deviceManagementService.createAreaNetworkInfo(areaNetworkInfoCreate);

  console.log(areaNetworkInfo);
})();
```

Response
```js
export class AreaNetworkInfoResult {
  AreaNetworkInfoId: string;
  Name: string;
  AreaNetworkType: string;
  ListOfDevices: string[];
}
```

2.7.2.	Retrieve Area Network Info Method

Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // retrieve Area Network Info by Id 
  var areaNetworkInfo = await deviceManagementService.retrieveAreaNetworkInfo("AreaNetworkInfoId");

  console.log(areaNetworkInfo);
})();
```

2.7.3.	Update Area Network Info Method

Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { AreaNetworkInfoUpdateData } from './devicemanagementservice/internal/area-network-info-update-data'

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to update AreaNetworkInfo
  var areaNetworkInfoUpdateData = new AreaNetworkInfoUpdateData();
  areaNetworkInfoUpdateData.AreaNetworkType = "Area Network Type";
  areaNetworkInfoUpdateData.ListOfDevices = ["CreatedBeforeAreaNetworkDeviceInfoResourceId","CreatedBeforeAreaNetworkDeviceInfoResourceId2"];


  // update Area Network Info request 
  var areaNetworkInfo = await deviceManagementService.updateAreaNetworkInfo(areaNetworkInfoUpdateData);

  console.log(areaNetworkInfo);
})();
```

2.7.4.	Delete Area Network Info Method

Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // delete Area Network Info by Id 
  var areaNetworkInfo = await deviceManagementService.deleteAreaNetworkInfo("AreaNetworkInfoId");

  console.log(areaNetworkInfo);
})();
```

2.8.	Area Network Device Info Related Methods
2.8.1.	Create Area Network Device Info Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { AreaNetworkDeviceInfoCreateData } from './devicemanagementservice/internal/area-network-device-info-create-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to create Area Network Device Info
  var areaNetworkDeviceInfoCreateData = new AreaNetworkDeviceInfoCreateData();
  areaNetworkDeviceInfoCreateData.Name = "Area Network Device Info Name";
  areaNetworkDeviceInfoCreateData.DevID = "DevID";
  areaNetworkDeviceInfoCreateData.DevType = "DevType";
  areaNetworkDeviceInfoCreateData.AreaNetworkId = "AreaNetworkId";
  areaNetworkDeviceInfoCreateData.ListOfNeighbors = ["Neighbor1"];


  // create Area Network Device Info request 
  var areaNetworkDeviceInfo = await deviceManagementService.createAreaNetworkDeviceInfo(areaNetworkDeviceInfoCreateData);

  console.log(areaNetworkDeviceInfo);
})();
```

Response
```js
export class AreaNetworkDeviceInfoResult {
  AreaNetworkDeviceInfoId: string;
  Name: string;
  DevID: string;
  DevType: string;
  DevStatus: string;
  AreaNetworkId: string;
  SleepInterval?: number;
  SleepDuration?: number;
  ListOfNeighbors: string[];
}
```

2.8.2.	Retrieve Area Network Device Info Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  
(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // retrieve Area Network Device Info request 
  var areaNetworkDeviceInfo = await deviceManagementService.retrieveAreaNetworkDeviceInfo("AreaNetworkDeviceInfoId");

  console.log(areaNetworkDeviceInfo);
})();
```

2.8.3.	Update Area Network Device Info Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { AreaNetworkDeviceInfoUpdateData } from './devicemanagementservice/internal/area-network-device-info-update-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to update Area Network Device Info
  var areaNetworkDeviceInfoUpdateData = new AreaNetworkDeviceInfoUpdateData();
  areaNetworkDeviceInfoUpdateData.AreaNetworkDeviceInfoId = "AreaNetworkDeviceInfoId";
  areaNetworkDeviceInfoUpdateData.DevStatus = "Sleeping";

  // update Area Network Device Info request 
  var areaNetworkDeviceInfo = await deviceManagementService.updateAreaNetworkDeviceInfo(areaNetworkDeviceInfoUpdateData);

  console.log(areaNetworkDeviceInfo);
})();
```
2.8.4.	Delete Area Network Device Info Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  
(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // delete Area Network Device Info request 
  var areaNetworkDeviceInfo = await deviceManagementService.deleteAreaNetworkDeviceInfo("AreaNetworkDeviceInfoId");

  console.log(areaNetworkDeviceInfo);
})();
```

2.9.	Reboot Related Methods
2.9.1.	Create Reboot Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceRebootCreateData } from './devicemanagementservice/internal/device-reboot-create-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to create Reboot
  var rebootCreateData = new DeviceRebootCreateData();
  rebootCreateData.Name = "Reboot Name";
  rebootCreateData.Reboot = true;

  // create Reboot request 
  var reboot = await deviceManagementService.createReboot(rebootCreateData);

  console.log(reboot);
})();
```


Response
```js
export class DeviceRebootResult {
  DeviceRebootId: string;
  Name: string;
  IsFactoryReset?: boolean;
  Reboot?: boolean;
}
```


2.9.3.	Update Reboot Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceRebootUpdateData } from './devicemanagementservice/internal/device-reboot-update-data';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to update Reboot
  var rebootUpdateData = new DeviceRebootUpdateData();
  rebootUpdateData.DeviceRebootId = "DeviceRebootId";
  rebootUpdateData.IsFactoryReset = true;

  // update Reboot request 
  var reboot = await deviceManagementService.updateReboot(rebootUpdateData);

  console.log(reboot);
})();
```

2.9.4.	Delete Reboot Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // delete Reboot request 
  var reboot = await deviceManagementService.deleteReboot("DeviceRebootId");

  console.log(reboot);
})();
```

2.10.	Firmware Related Methods
2.10.1.	Create Firmware Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceFirmwareCreateData } from './devicemanagementservice/internal/device-firmware-create-data';
var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to create Firmware
  var firmwareCreateData = new DeviceFirmwareCreateData();
  firmwareCreateData.Name = "Firmware Name";
  firmwareCreateData.Version = "Version";
  firmwareCreateData.URL = "URL";
  firmwareCreateData.Update = true;

  // create Firmware request 
  var firmware = await deviceManagementService.createFirmware(firmwareCreateData);

  console.log(firmware);
})();
```

Response
```js

```

2.10.3.	Update Firmware Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';
import { DeviceFirmwareUpdateData } from './devicemanagementservice/internal/device-firmware-update-data';
var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // set data to update Firmware
  var firmwareUpdateData = new DeviceFirmwareUpdateData();
  firmwareUpdateData.Version = "Version2";
  firmwareUpdateData.Update = false;

  // update Firmware request 
  var firmware = await deviceManagementService.updateFirmware(firmwareUpdateData);

  console.log(firmware);
})();
```

2.10.4. Delete Firmware Method
Request
```js
import { DeviceClientMqttOptionsBuilder } from './internal/options/device-client-options-builder';
import { DeviceManagementService } from './devicemanagementservice/device-management-service';

var mqttOptionsBuilder = new DeviceClientMqttOptionsBuilder();

var options = mqttOptionsBuilder
  .withCSEId('bve-sol')
  .withClientId('CAE38dde70b-e7cc-4b4d-bf48-8c9adfdaca98')
  .withMqttOptions('127.0.0.1', 1886, 300000)
  .build();

  // Create Device Management Service with Mqtt Options 
  var deviceManagementService = new DeviceManagementService(options);
  

(async () => {

  // Device Management Service Connection to Platform
  await deviceManagementService.connectToPlatformAsync();

  // delete Firmaware request 
  var firmware = await deviceManagementService.deleteFirmware("FirmwareId");

  console.log(firmware);
})();
```








-------------------
.NET SDK
-------------------

Device Registration Method

Request

-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Client.RegistrationService;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Registration Service Call
            IRegistrationServiceFactory _registrationServiceFactory = new RegistrationServiceFactory();
            var _registrationService = _registrationServiceFactory.CreateRegistrationService(deviceMqttClientOptions);

            // Device Registration to system
            var deviceRegistrationResult = await _registrationService.RegisterDeviceAsync("Device Name");
            Console.WriteLine("Device ID: " + deviceRegistrationResult.DeviceId + " Device Name: " + deviceRegistrationResult.DeviceName);

            Console.ReadLine();
        }
    }
}

```

Response

-----

```csharp
public class DeviceRegistrationResult
    {
        public string DeviceId { get; set; }
        public string DeviceName { get; set; }
    }

```
-----

Connect to Platform Method

Request

-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();
            
            // Device Service Call
            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var _deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            // Device Service Connection To Platform
            // Here the related resources and properties are loaded to device service objects
            await _deviceService.ConnectToPlatformAsync();

            Console.ReadLine();
            

        }
    }
}

```



1.3.	Sensor Related Methods
1.3.1.	Sensor Creation Method

Request
-----

```csharp
	using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Service Call
            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var _deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            // Device Service Connection To Platform
            // Here the related resources and properties are loaded to device service objects
            await _deviceService.ConnectToPlatformAsync();

            // Sensor Creation
            // Parameters: Sensor Name and isBidirectional
            var sensorResult = await _deviceService.CreateSensorAsync("Sensor Name", true);
            Console.WriteLine("Sensor Id: " + sensorResult.SensorId + "Sensor Name: " + sensorResult.Name);
            Console.ReadLine();
        }
    }
}

```
Response
-----

```csharp
	public class SensorCreationResult
    {
        public string SensorId { get; set; }
        public string Name { get; set; }
    }
```

1.3.2.	Sensor Deletion Method

Request
-----

```csharp
	using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Service Call
            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var _deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            // Device Service Connection To Platform
            // Here the related resources and properties are loaded to device service objects
            await _deviceService.ConnectToPlatformAsync();

            // Sensor Creation
            // Parameters: Sensor Name and isBidirectional
            var sensorResult = await _deviceService.CreateSensorAsync("Sensor Name", true);
            Console.WriteLine("Sensor Id: " + sensorResult.SensorId + "Sensor Name: " + sensorResult.Name);

            // Sensor Deletion 
            // Parameters: Sensor Id
            var deleteSensorResult = await _deviceService.DeleteSensorAsync(sensorResult.SensorId);
            Console.WriteLine("Sensor Id: " + deleteSensorResult.SensorId + "Sensor Name: " + deleteSensorResult.SensorName);
            Console.ReadLine();
        }
    }
}

```
Response
-----

```csharp
	public class SensorDeletionResult
    {
        public string SensorId { get; set; }
        public string SensorName { get; set; }
    }
```

1.3.3.	Push Sensor Data to Platform Method

Request
-----

```csharp
	using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Service Call
            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var _deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            // Device Service Connection To Platform
            // Here the related resources and properties are loaded to device service objects
            await _deviceService.ConnectToPlatformAsync();

            // Sensor Creation
            // Parameters: Sensor Name and isBidirectional
            var sensorResult = await _deviceService.CreateSensorAsync("Sensor Name", true);
            Console.WriteLine("Sensor Id: " + sensorResult.SensorId + "Sensor Name: " + sensorResult.Name);

            // Add Sensor Data 
            await _deviceService.PushSensorDataToPlatformAsync("sensor Id", "sensor message");
            // Add Sensor Data as raw value and unit
            await _deviceService.PushSensorDataToPlatformAsync("sensor Id", "raw value of sensor", "unit of sensor value", "extracted data");
            
            Console.ReadLine();
        }
    }
}

```


1.4.	Battery Related Methods
1.4.1.	Create Battery Method
Request
-----

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var createdBatteryResult = await deviceManagementService.CreateBatteryAsync(new DeviceBatteryCreateData()
            {
                Name = "BatteryName",
                BatteryLevel = 100,
                BatteryStatus = BatteryStatus.Normal
            });
            Console.WriteLine(createdBatteryResult.BatteryId);
        }
    }
}

```
Response
-----

```csharp
	public class DeviceBatteryResult
    {
        public string BatteryId { get; set; }
        public string Name { get; set; }
        public int? BatteryLevel { get; set; }
        public BatteryStatus? BatteryStatus { get; set; }
    }
```

1.4.2.	Retrieve Battery Method
Request
-----
```csharp
	using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var retrieveBatteryResult = await deviceManagementService.RetrieveBatteryAsync("Battery Id");
            Console.WriteLine(retrieveBatteryResult.BatteryId);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceBatteryResult
    {
        public string BatteryId { get; set; }
        public string Name { get; set; }
        public int? BatteryLevel { get; set; }
        public BatteryStatus? BatteryStatus { get; set; }
    }
```


1.4.3.	Update Battery Method

Request
-----
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var updateBatteryResult = await deviceManagementService.UpdateBatteryAsync(new DeviceBatteryUpdateData
            {
                BatteryId = "Battery Id",
                BatteryLevel = 1,
                BatteryStatus = BatteryStatus.LowBattery
            });
            Console.WriteLine(updateBatteryResult.BatteryId);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceBatteryResult
    {
        public string BatteryId { get; set; }
        public string Name { get; set; }
        public int? BatteryLevel { get; set; }
        public BatteryStatus? BatteryStatus { get; set; }
    }
```

1.4.4.	Delete Battery Method

Request
-----
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var deleteBatteryResult = await deviceManagementService.DeleteBatteryAsync("battery Id");
            Console.WriteLine(deleteBatteryResult.BatteryId);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceBatteryResult
    {
        public string BatteryId { get; set; }
        public string Name { get; set; }
        public int? BatteryLevel { get; set; }
        public BatteryStatus? BatteryStatus { get; set; }
    }
```

1.5.	Memory Related Methods
1.5.1.	Create Memory Method
Request
-----
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var createMemoryResult = await deviceManagementService.CreateMemoryAsync(new DeviceMemoryCreateData
            {
                Name = "Memory Name",
                AvailableMemory = 4194304000, // 500 MB
                TotalMemory = 8589934592 // 1 GB
            });
            Console.WriteLine(createMemoryResult.MemoryId);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceMemoryResult
    {
        public string MemoryId { get; set; }
        public string Name { get; set; }
        public long? TotalMemory { get; set; }
        public long? AvailableMemory { get; set; }
    }
```


1.5.2.	Retrieve Memory Method

Request
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var retrieveMemoryResult = await deviceManagementService.RetrieveMemoryAsync("memory Id");
            Console.WriteLine("Available Memory: " + retrieveMemoryResult.AvailableMemory);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceMemoryResult
    {
        public string MemoryId { get; set; }
        public string Name { get; set; }
        public long? TotalMemory { get; set; }
        public long? AvailableMemory { get; set; }
    }
```

1.5.3.	Update Memory Method
Request
-----
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var updateMemoryResult = await deviceManagementService.UpdateMemoryAsync(new DeviceMemoryUpdateData {
                MemoryId = "memory Id",
                AvailableMemory = 1677721600 // 200 MB
            });
            Console.WriteLine(updateMemoryResult.AvailableMemory);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceMemoryResult
    {
        public string MemoryId { get; set; }
        public string Name { get; set; }
        public long? TotalMemory { get; set; }
        public long? AvailableMemory { get; set; }
    }
```

1.5.4.	Delete Memory Method

Request
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Call
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            var deleteMemoryResult = await deviceManagementService.DeleteMemoryAsync("memory Id");
            Console.WriteLine(deleteMemoryResult.AvailableMemory);
        }
    }
}
```


Response
-----
```csharp
	public class DeviceMemoryResult
    {
        public string MemoryId { get; set; }
        public string Name { get; set; }
        public long? TotalMemory { get; set; }
        public long? AvailableMemory { get; set; }
    }
```

1.6.	Device Info Related Methods
1.6.1.	Create Device Info Method

Request
-----
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Physical Device Info Create Request
            var createPhysicalDeviceInfoResult = await deviceManagementService.CreatePhysicalDeviceInfoAsync(new DeviceGeneralInfoCreateData
            {
                Name = "Physical Device Name",
                DeviceLabel = "Device Label",
                Manufacturer = "Manufacturer",
                Model = "Device Model",
                DeviceType = "Device Type"
            });
            Console.WriteLine(createPhysicalDeviceInfoResult.DeviceGeneralInfoId);
        }
    }
}
```


Response
-----
```csharp
	 public class DeviceGeneralInfoResult
    {
        public string DeviceGeneralInfoId { get; set; }

        public string Name { get; set; }

        public string DeviceLabel { get; set; }

        public string Manufacturer { get; set; }

        public string Model { get; set; }

        public string DeviceType { get; set; }

        public string FirmwareVersion { get; set; }

        public string SoftwareVersion { get; set; }

        public string HardwareVersion { get; set; }

        public string ManufacturerDetailsLink { get; set; }

        public string ManufacturingDate { get; set; }

        public string SubModel { get; set; }

        public string DeviceName { get; set; }

        public string OsVersion { get; set; }

        public string Country { get; set; }

        public string Location { get; set; }

        public string SystemTime { get; set; }

        public List<string> SupportURL { get; set; }

        public List<string> PresentationURL { get; set; }

        public List<string> Protocol { get; set; }

    }
```

1.6.2.	Retrieve Device Info Method

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Physical Device Info Retrieve Request
            var retrievePhysicalDeviceInfoResult = await deviceManagementService.RetrievePhysicalDeviceInfoAsync("Physical Device Info Id");
            Console.WriteLine(retrievePhysicalDeviceInfoResult.DeviceName);
        }
    }
}
```
1.6.3.	Update Device Info Method

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // // Physical Device Info Update Request
            var updatePhysicalDeviceInfoResult = await deviceManagementService.UpdatePhysicalDeviceInfoAsync(new DeviceGeneralInfoUpdateData
            {
                DeviceGeneralInfoId = "Physical Device Id",
                OsVersion = "Os Version"
            });
            Console.WriteLine(updatePhysicalDeviceInfoResult.DeviceName);
        }
    }
}
```


1.6.3.	Delete Device Info Method

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Physical Device Info Delete Request
            var deletePhysicalDeviceInfoResult = await deviceManagementService.DeletePhysicalDeviceInfoAsync("Physical Device Info Id");
            Console.WriteLine(deletePhysicalDeviceInfoResult.DeviceName);
        }
    }
}
```

1.7.	Area Network Info Related Methods
1.7.1.	Create Area Network Info Method
Request
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Info Create Request
            var createAreaNetworkInfoResult = await deviceManagementService.CreateAreaNetworkInfoAsync(new DeviceAreaNetworkInfoCreateData
            {
                Name = "Name",
                AreaNetworkType = "Area Network Type",
                ListOfDevices = new List<string> {"CreatedBeforeAreaNetworkDeviceInfoResourceId" }
            });
            Console.WriteLine(createAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```
Response

```csharp

 public class DeviceAreaNetworkInfoResult
    {
        public string AreaNetworkInfoId { get; set; }
        public string Name { get; set; }
        public string AreaNetworkType { get; set; }
        public List<string> ListOfDevices { get; set; }
    }

```

1.7.2.	Retrieve Area Network Info Method
```csharp

using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Info Retrieve Request
            var retrieveAreaNetworkInfoResult = await deviceManagementService.RetrieveAreaNetworkInfoAsync("AreaNetworkInfoId");
            Console.WriteLine(retrieveAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}

```

1.7.3.	Update Area Network Info Method

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Info Update Request
            var updateAreaNetworkInfoResult = await deviceManagementService.UpdateAreaNetworkInfoAsync(new DeviceAreaNetworkInfoUpdateData
            {
                AreaNetworkInfoId = "AreaNetworkInfoId",
                AreaNetworkType = "Update Area Network Type",
                ListOfDevices = new List<string> { "CreatedBeforeAreaNetworkDeviceInfoResourceId", "CreatedBeforeAreaNetworkDeviceInfoResourceId2" }
            });
            Console.WriteLine(updateAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```


1.7.4.	Delete Area Network Info Method

```csharp

using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Info Delete Request
            var deleteAreaNetworkInfoResult = await deviceManagementService.DeleteAreaNetworkInfoAsync("AreaNetworkInfoId");
            Console.WriteLine(deleteAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}

```


1.8.	Area Network Device Info Related Methods
1.8.1.	Create Area Network Device Info Method

```csharp

using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Device Info Create Request
            var createAreaNetworkDeviceInfoResult = await deviceManagementService.CreateAreaNetworkDeviceInfoAsync(new DeviceAreaNetworkDeviceInfoCreateData
            {
                Name = "Name",
                DevID = "DevID",
                DevType = "DevType",
                AreaNetworkId = "AreaNetworkId",
                ListOfNeighbors = new List<string> { "ListOfNeighbors"}
            });
            Console.WriteLine(createAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Response
```csharp
 public class DeviceAreaNetworkDeviceInfoResult
    {
        public string AreaNetworkDeviceInfoId { get; set; }

        public string Name { get; set; }

        public string DevID { get; set; }

        public string DevType { get; set; }

        public string AreaNetworkId { get; set; }

        public int? SleepInterval { get; set; }

        public int? SleepDuration { get; set; }

        public string DevStatus { get; set; }

        public List<string> ListOfNeighbors { get; set; }
    }
```

1.8.2.	Retrieve Area Network Device Info Method
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Device Info Retrieve Request
            var retrieveAreaNetworkDeviceInfoResult = await deviceManagementService.RetrieveAreaNetworkDeviceInfoAsync("AreaNetworkDeviceInfoId");
            Console.WriteLine(retrieveAreaNetworkDeviceInfoResult.Name);
        }
    }
}
```

1.8.3.	Update Area Network Device Info Method
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Device Info Update Request
            var updateAreaNetworkDeviceInfoResult = await deviceManagementService.UpdateAreaNetworkDeviceInfoAsync(new DeviceAreaNetworkDeviceInfoUpdateData
            {
                AreaNetworkDeviceInfoId = "AreaNetworkDeviceInfoId",
                DevStatus = "Sleeping"
            });
            Console.WriteLine(updateAreaNetworkDeviceInfoResult.DevStatus);
        }
    }
}
```

1.8.4.	Delete Area Network Device Info Method
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Area Network Device Info Delete Request
            var deleteAreaNetworkDeviceInfoResult = await deviceManagementService.DeleteAreaNetworkDeviceInfoAsync("AreaNetworkDeviceInfoId");
            Console.WriteLine(deleteAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```




1.9.	Reboot Related Methods 
1.9.1.	Create Reboot Method

```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Reboot Create Request
            var createRebootResult = await deviceManagementService.CreateRebootAsync(new DeviceRebootCreateData
            {
                Name = "Name"
            });
            Console.WriteLine(createRebootResult.DeviceRebootId);
        }
    }
}
```

Response 
```csharp
public class DeviceRebootResult
    {
        public string DeviceRebootId { get; set; }
        public string Name { get; set; }
        public bool? IsFactoryReset { get; set; }
        public bool? Reboot { get; set; }
    }
```

1.9.2.	Retrieve Reboot Method

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Reboot Retrieve Request
            var retrieveRebootResult = await deviceManagementService.RetrieveRebootAsync("DeviceRebootId");
            Console.WriteLine(retrieveRebootResult.Name);
        }
    }
}
```
1.9.3.	Update Reboot Method
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Reboot Update Request
            var updateRebootResult = await deviceManagementService.UpdateRebootAsync(new DeviceRebootUpdateData
            {
                DeviceRebootId = "DeviceRebootId",
                Reboot = true
            });
            Console.WriteLine(updateRebootResult.DeviceRebootId);
        }
    }
}
```

1.9.4.	Delete Reboot Method
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Reboot Delete Request
            var deleteRebootResult = await deviceManagementService.DeleteRebootAsync("DeviceRebootId");
            Console.WriteLine(deleteRebootResult.Name);
        }
    }
}
```


1.10.	Firmware Related Methods
1.10.1.	Create Firmware Method
Request
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Create Update Request
            var createFirmwareResult = await deviceManagementService.CreateFirmwareAsync(new DeviceFirmwareCreateData
            {
                Name = "Firmware Name",
                Version = "Version",
                URL = "URL",
                Update = true
            });
            Console.WriteLine(createFirmwareResult.DeviceFirmwareId);
        }
    }
}
```

Response
```csharp
public class DeviceFirmwareResult
    {
        public string DeviceFirmwareId { get; set; }
        public string Version { get; set; }
        public string Name { get; set; }
        public string URL { get; set; }
        public bool? Update { get; set; }
        public ActionStatus UpdateStatus { get; set; }
    }
```


1.10.2.	Retrieve Firmware Method
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Firmware Retrieve Request
            var retrieveFirmwareResult = await deviceManagementService.RetrieveFirmwareAsync("DeviceFirmwareId");
            Console.WriteLine(retrieveFirmwareResult.Version);
        }
    }
}
```



1.10.3.	Update Firmware Method
```csharp
using Platform360.Devices.SDK.Client.DeviceManagementService.Internal;
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Firmware Update Request
            var updateFirmwareResult = await deviceManagementService.UpdateFirmwareAsync(new DeviceFirmwareUpdateData
            {
                Version = "Update Version",
                Update = true
            });
            Console.WriteLine(createFirmwareResult.Version);
        }
    }
}
```


1.10.4.	Delete Firmware Method

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Mqtt Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt PoA", 1883, 120)
                    .Build();

            // Device Management Service Creation with Mqtt Client Options
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            // Device Management Service Connection To Platform
            // Here the related resources and properties are loaded to device management service objects
            await deviceManagementService.ConnectToPlatformAsync();

            // Firmware Delete Request
            var deleteFirmwareResult = await deviceManagementService.DeleteFirmwareAsync("DeviceFirmwareId");
            Console.WriteLine(deleteFirmwareResult.DeviceFirmwareId);
        }
    }
}
```






--------------------------
---------------------------

BURAYA KADAR

----------------------------
-----------------------------


Usage
-----

1.Device Registration Method
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "127.0.0.1";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CLIENT-ID")
           .WithCSEId("CSE-ID")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            var deviceName = "AE-" + Guid.NewGuid().ToString();
            var poa = new List<string>() { "http://localhost:3005" };

            var registerDeviceResult = await deviceService.RegisterDeviceAsync(deviceName, poa);

            Console.WriteLine(registerDeviceResult.DeviceId);
        }
    }
}


```

DeviceRegistrationResult
-----

```csharp
public class DeviceRegistrationResult
    {
        public string DeviceId { get; set; }
        public string DeviceName { get; set; }
        public IList<string> PoA { get; set; }
    }

```

2.	Connect to Platform Method
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "127.0.0.1";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CLIENT-ID")
           .WithCSEId("CSE-ID")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            await deviceService.ConnectToPlatformAsync();
        }
    }
}


```

3.	Create Sensor Method
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "127.0.0.1";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CLIENT-ID")
           .WithCSEId("CSE-ID")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);

            var sensorName = "CONTAINE-" + Guid.NewGuid().ToString();
            var createSensorResult = await deviceService.CreateSensorAsync(sensorName, true);
            Console.WriteLine(createSensorResult.SensorId);
        }
    }
}


```

3.	SensorCreationResult
-----

```csharp
public class SensorCreationResult
   {
       public string SensorId { get; set; }
       public string Name { get; set; }
   }



```

4.	Delete Sensor Method
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "127.0.0.1";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CLIENT-ID")
           .WithCSEId("CSE-ID")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);
            var sensorDeletionResult = await deviceService.DeleteSensorAsync("SensorID");
        }
    }
}

```

SensorDeletionResult
-----

```csharp
public class SensorDeletionResult
   {
       public string SensorId { get; set; }
       public string SensorName { get; set; }
   }


```

5.	Push Sensor Data to Platform Method
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "127.0.0.1";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CLIENT-ID")
           .WithCSEId("CSE-ID")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceServiceFactory _deviceServiceFactory = new DeviceServiceFactory();
            var deviceService = _deviceServiceFactory.CreateDeviceService(deviceMqttClientOptions);
            await deviceService.PushSensorDataToPlatformAsync("SensorID", "Pushed Data");
        }
    }
}

```


Reboot Create Example
-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();
            
            var createdRebootResult = await deviceManagementService.CreateRebootAsync(new DeviceRebootData
            {
                Name = "RebootName",
                IsFactoryReset = true,
                Reboot = false
            });

            Console.WriteLine(createdRebootResult.DeviceRebootId);
        }
    }
}

```

Reboot Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedRebootResult = await deviceManagementService.RetrieveRebootAsync("/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b");

            Console.WriteLine(retrievedRebootResult.DeviceRebootId);
        }
    }
}

```

Reboot Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var updatedRebootResult = await deviceManagementService.UpdateRebootAsync(new DeviceRebootData
            {
                DeviceRebootId = "/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b",
                IsFactoryReset = true,
                Reboot = false
            });
            Console.WriteLine(updatedRebootResult.DeviceRebootId);
        }
    }
}

```

Reboot Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
             var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedRebootResult = await deviceManagementService.DeleteRebootAsync("/cos-ins/mgo_f031687b-48b6-4448-82a5-608890390c0b");
            Console.WriteLine(deletedRebootResult.DeviceRebootId);
        }
    }
}

```

Battery Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdBatteryResult = await deviceManagementService.CreateBatteryAsync(new DeviceBatteryData()
            {
                Name = "BatteryName",
                BatteryLevel = 100,
                BatteryStatus = BatteryStatus.Normal
            });
            Console.WriteLine(createdBatteryResult.BatteryId);
        }
    }
}

```

Battery Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedBatteryResult = await deviceManagementService.RetrieveBatteryAsync("/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0");
            Console.WriteLine(retrievedBatteryResult.BatteryId);
        }
    }
}

```

Battery Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var updatedBatteryResult = await deviceManagementService.UpdateBatteryAsync(new DeviceBatteryData
            {
                BatteryId = "/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0",
                BatteryLevel = 101,
                BatteryStatus = BatteryStatus.Charging
            });
            Console.WriteLine(updatedBatteryResult.BatteryId);
        }
    }
}

```

Battery Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using Platform360.Devices.SDK.OneM2M.Resources.MgmtResources;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedBatteryResult= await deviceManagementService.DeleteBatteryAsync("/cos-ins/mgo_b6e3b697-8848-4585-af61-b2060270aba0");
            Console.WriteLine(deletedBatteryResult.BatteryId);
        }
    }
}

```

Memory Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdMemoryResult = await deviceManagementService.CreateMemoryAsync(new DeviceMemoryData()
            {
                Name = "Memory_" + Guid.NewGuid().ToString(),
                TotalMemory = 4096,
                AvailableMemory = 970
            });
            Console.WriteLine(createdMemoryResult.MemoryId);
        }
    }
}

```

Memory Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedMemoryResult = await deviceManagementService.RetrieveMemoryAsync("/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea");
            Console.WriteLine(retrievedMemoryResult.MemoryId);
        }
    }
}

```

Memory Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();
            
            var updatedMemoryResult = await deviceManagementService.UpdateMemoryAsync(new DeviceMemoryData()
            {
                MemoryId = "/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea",
                Name = "Memory_Update_" + Guid.NewGuid().ToString(),
                TotalMemory = 4097,
                AvailableMemory = 977
            });
            Console.WriteLine(updatedMemoryResult.MemoryId);
        }
    }
}

```

Memory Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var deletedMemoryResult = await deviceManagementService.DeleteMemoryAsync("/cos-ins/mgo_b620c0c2-a88a-49f8-8232-cd49e54813ea");
            Console.WriteLine(deletedMemoryResult.MemoryId);
        }
    }
}

```

Device Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();
            
            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);
            
            await deviceManagementService.ConnectToPlatformAsync();

            var createdPhysicalDeviceInfoResult = await deviceManagementService.CreatePhysicalDeviceInfoAsync(new DeviceGeneralInfoData
            {
                Name = "DeviceInfo_" + Guid.NewGuid().ToString(),
                DeviceType = "Health",
                Manufacturer = "kd_health",
                Model = "A1018_CreateTest",
                DeviceLabel = "Red",
                Country = "TR",
                DeviceName = "DeviceName",
                FirmwareVersion = "v1",
                HardwareVersion = "v1",
                Location = "Location",
                ManufacturerDetailsLink = "ManufacturerDetailsLink",
                ManufacturingDate = "ManufacturingDate",
                OsVersion = "OsVersion",
                PresentationURL = new List<string> { "PresentationURL" },
                Protocol = new List<string> { "Protocol" },
                SoftwareVersion = "SoftwareVersion",
                SubModel = "SubModel",
                SupportURL = new List<string> { "SupportURL" },
                SystemTime = "SystemTime"
            });
            Console.WriteLine(createdPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}


```

Device Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedPhysicalDeviceInfoResult = await deviceManagementService.RetrievePhysicalDeviceInfoAsync("/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9");
            Console.WriteLine(retrievedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}


```

Device Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedPhysicalDeviceInfoResult = await deviceManagementService.UpdatePhysicalDeviceInfoAsync(new DeviceGeneralInfoData
            {
                PhysicalDeviceInfoId = "/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9",
                DeviceType = "Health_U",
                DeviceLabel = "Red_U",
                DeviceName = "DeviceName_U",
                FirmwareVersion = "v1_U",
                Location = "Location_U",
                ManufacturerDetailsLink = "ManufacturerDetailsLink_U",
                OsVersion = "OsVersion_U",
                PresentationURL = new List<string> { "PresentationURL_U" },
                Protocol = new List<string> { "Protocol_U" },
                SoftwareVersion = "SoftwareVersion_U",
                SupportURL = new List<string> { "SupportURL_U" },
                SystemTime = "SystemTime_U"
            });
            Console.WriteLine(updatedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}
```

Device Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedPhysicalDeviceInfoResult = await deviceManagementService.DeletePhysicalDeviceInfoAsync("/cos-ins/mgo_cbc0c2eb-78de-4871-b1a8-a0b761820dd9");
            Console.WriteLine(deletedPhysicalDeviceInfoResult.PhysicalDeviceInfoId);
        }
    }
}

```

Area Network Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var areaNetworkInfoResult = await deviceManagementService.CreateAreaNetworkInfoAsync(new AreaNetworkInfoData
            {
                Name = "AreaNetworkInfo_" + Guid.NewGuid().ToString(),
                AreaNetworkType = "IPbased",
                ListOfDevices = new List<string> { "phone", "watch" }
            });
            Console.WriteLine(areaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}

```

Area Network Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var createdAreaNetworkInfoResult = await deviceManagementService.RetrieveAreaNetworkInfoAsync("/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35");
            Console.WriteLine(createdAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedAreaNetworkInfoResult = await deviceManagementService.UpdateAreaNetworkInfoAsync(new AreaNetworkInfoData
            {
                AreaNetworkInfoId = "/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35",
                AreaNetworkType = "IPbased_U",
                ListOfDevices = new List<string> { "phone_U", "watch_U" }
            });
            Console.WriteLine(updatedAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedAreaNetworkInfoResult = await deviceManagementService.DeleteAreaNetworkInfoAsync("/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35");
            Console.WriteLine(deletedAreaNetworkInfoResult.AreaNetworkInfoId);
        }
    }
}
```

Area Network Device Info Create Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var createdAreaNetworkDeviceInfoResult = await deviceManagementService.CreateAreaNetworkDeviceInfoAsync(new AreaNetworkDeviceInfoData
            {
                Name = "AreaNetworkDeviceInfo_" + Guid.NewGuid().ToString(),
                AreaNetworkId = "/cos-ins/mgo_4b4a1713-6b0b-4add-9f67-ccfd6ed8ce35", 
                DevID = "1",
                DevType = "1",
                ListOfNeighbors = new List<string> { "neighbor1", "neighbor2" },
                DevStatus = "DevStatus",
                SleepDuration = 1,
                SleepInterval = 5
            });
            Console.WriteLine(createdAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Retrieve Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var retrievedAreaNetworkDeviceInfoResult = await deviceManagementService.RetrieveAreaNetworkDeviceInfoAsync("/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a");
            Console.WriteLine(retrievedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Update Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using Platform360.Devices.SDK.Contracts.DeviceManagement;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var updatedAreaNetworkDeviceInfoResult = await deviceManagementService.UpdateAreaNetworkDeviceInfoAsync(new AreaNetworkDeviceInfoData
            {
                AreaNetworkDeviceInfoId = "/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a",
                DevID = "1U",
                DevType = "1U",
                ListOfNeighbors = new List<string> { "neighbor1U", "neighbor2U" },
                DevStatus = "DevStatusU",
                SleepDuration = 2,
                SleepInterval = 3
            });
            Console.WriteLine(updatedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Area Network Device Info Delete Example
-----
```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Communicator;
using System;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            var mqttBrokerPoa = "172.18.228.29";
            var mqttBrokerPort = 1883;
            int timeOut = 120;
            int keepAlive = 10;

            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
           .WithClientId("CAE93a8573f-11f7-4902-950b-f63ee71f5685")
           .WithCSEId("cos-ins")
           .WithMqttOptions(mqttBrokerPoa, mqttBrokerPort, timeOut, keepAlive)
           .Build();

            IDeviceManagementServiceFactory _deviceManagementServiceFactory = new DeviceManagementServiceFactory();
            var deviceManagementService = _deviceManagementServiceFactory.CreateDeviceManagementService(deviceMqttClientOptions);

            await deviceManagementService.ConnectToPlatformAsync();

            var deletedAreaNetworkDeviceInfoResult = await deviceManagementService.DeleteAreaNetworkDeviceInfoAsync("/cos-ins/mgo_a7278ca0-fc6e-402d-9b10-97484c0a004a");
            Console.WriteLine(deletedAreaNetworkDeviceInfoResult.AreaNetworkDeviceInfoId);
        }
    }
}
```

Device Reboot Data
-----
```csharp

public class DeviceRebootData
{
    public string DeviceRebootId { get; set; }
    public string Name { get; set; }
    public bool? IsFactoryReset { get; set; }
    public bool? Reboot { get; set; }
}

```
Device Reboot Response
-----
```csharp

public class DeviceRebootResult
{
    public string DeviceRebootId { get; set; }
    public string Name { get; set; }
    public bool? IsFactoryReset { get; set; }
    public bool? Reboot { get; set; }
}

```

Device Battery Data
-----
```csharp

public class DeviceBatteryData
{
    public string BatteryId { get; set; }
    public string Name { get; set; }
    public int? BatteryLevel { get; set; }
    public BatteryStatus? BatteryStatus { get; set; }
}


```
Device Battery Response
-----
```csharp

public class DeviceBatteryResult
{
    public string BatteryId { get; set; }
    public string Name { get; set; }
    public int? BatteryLevel { get; set; }
    public BatteryStatus? BatteryStatus { get; set; }
}

```

Device Memory Data
-----
```csharp
public class DeviceMemoryData
{
    public string MemoryId { get; set; }
    public string Name { get; set; }
    public long? TotalMemory { get; set; }
    public long? AvailableMemory { get; set; }
}

```
Device Memory Response
-----
```csharp
public class DeviceMemoryResult
{
    public string MemoryId { get; set; }
    public string Name { get; set; }
    public long? TotalMemory { get; set; }
    public long? AvailableMemory { get; set; }
}

```

Device General Info Data
-----
```csharp
public class DeviceGeneralInfoData
{
    public string PhysicalDeviceInfoId { get; set; }
    public string Name { get; set; }
    public string DeviceLabel { get; set; }
    public string Manufacturer { get; set; }
    public string Model { get; set; }
    public string DeviceType { get; set; }
    public string FirmwareVersion { get; set; }
    public string SoftwareVersion { get; set; }
    public string HardwareVersion { get; set; }
    public string ManufacturerDetailsLink { get; set; }
    public string ManufacturingDate { get; set; }
    public string SubModel { get; set; }
    public string DeviceName { get; set; }
    public string OsVersion { get; set; }
    public string Country { get; set; }
    public string Location { get; set; }
    public string SystemTime { get; set; }
    public List<string> SupportURL { get; set; }
    public List<string> PresentationURL { get; set; }
    public List<string> Protocol { get; set; }
}


```
Device General Info Response
-----
```csharp

public class DeviceGeneralInfoResult
{
    public string PhysicalDeviceInfoId { get; set; }
    public string Name { get; set; }
    public string DeviceLabel { get; set; }
    public string Manufacturer { get; set; }
    public string Model { get; set; }
    public string DeviceType { get; set; }
    public string FirmwareVersion { get; set; }
    public string SoftwareVersion { get; set; }
    public string HardwareVersion { get; set; }
    public string ManufacturerDetailsLink { get; set; }
    public string ManufacturingDate { get; set; }
    public string SubModel { get; set; }
    public string DeviceName { get; set; }
    public string OsVersion { get; set; }
    public string Country { get; set; }
    public string Location { get; set; }
    public string SystemTime { get; set; }
    public List<string> SupportURL { get; set; }
    public List<string> PresentationURL { get; set; }
    public List<string> Protocol { get; set; }
        
}


```

Device Area Network Info Data
-----
```csharp

public class AreaNetworkInfoData
{
    public string AreaNetworkInfoId { get; set; }
    public string Name { get; set; }
    public string AreaNetworkType { get; set; }
    public List<string> ListOfDevices { get; set; }
}


```
Device Area Network Info Response
-----
```csharp

public class AreaNetworkInfoResult
{
    public string AreaNetworkInfoId { get; set; }
    public string Name { get; set; }
    public string AreaNetworkType { get; set; }
    public List<string> ListOfDevices { get; set; }
}


```

Device Area Network Device Info Data
-----
```csharp
public class AreaNetworkDeviceInfoData
{
    public string AreaNetworkDeviceInfoId { get; set; }
    public string Name { get; set; }
    public string DevID { get; set; }
    public string DevType { get; set; }
    public string AreaNetworkId { get; set; }
    public int? SleepInterval { get; set; }
    public int? SleepDuration { get; set; }
    public string DevStatus { get; set; }
    public List<string> ListOfNeighbors { get; set; }
}

```
Device Area Network Device Info Response
-----
```csharp
public class AreaNetworkDeviceInfoResult
{
	public string AreaNetworkDeviceInfoId { get; set; }
	public string Name { get; set; }
	public string DevID { get; set; }
	public string DevType { get; set; }
	public string AreaNetworkId { get; set; }
	public int? SleepInterval { get; set; }
	public int? SleepDuration { get; set; }
	public string DevStatus { get; set; }
	public List<string> ListOfNeighbors { get; set; }
}

```

