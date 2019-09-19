Device Registration Method

Request

-----

```csharp
using Platform360.Devices.SDK.Client.Options;
using Platform360.Devices.SDK.Client.RegistrationService;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace Platform360.Devices.SDK.Example.ConsoleApp
{
    class Program
    {
        async static Task Main(string[] args)
        {
            // Definition of device Client Options
            var deviceMqttClientOptions = new DeviceClientOptionsBuilder()
                    .WithClientId("Allowed AE ID")
                    .WithCSEId("CSE ID")
                    .WithMqttOptions("Mqtt Server PoA", 1883, 120)
                    .Build();
            
            // Registration Service Call
            IRegistrationServiceFactory _registrationServiceFactory = new RegistrationServiceFactory();
            var _registrationService = _registrationServiceFactory.CreateRegistrationService(deviceMqttClientOptions);
            
            // Device Registration to system
            var deviceRegistrationResult = await _registrationService.RegisterDeviceAsync("Device Name", new List<string> { "http://pointOfAccessUrl.com" });
            Console.WriteLine("Device ID: " + deviceRegistrationResult.DeviceId + " Device Name: " +
                deviceRegistrationResult.DeviceName +" Point of Access: "+ deviceRegistrationResult.PoA);

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
        public IList<string> PoA { get; set; }
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

-----------------
-----------------
BURAYA KADAR
------------------
------------------

1.4.	Battery Related Methods
1.4.1.	Create Battery Method
Request
-----

```csharp
	public class SensorDeletionRequestDto 
    {
        public string SensorId { get; set; }
    }

```
Response
-----

```csharp
	public class SensorDeletionResponseDto 
    {
        public string SensorId { get; set; }
        public IList<string> NotificationUrls{ get; set; }
		public string Identifier { get; set; }
    }
```


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

