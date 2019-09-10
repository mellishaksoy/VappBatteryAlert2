
Usage
-----

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
